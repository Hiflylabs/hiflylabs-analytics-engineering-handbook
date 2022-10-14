# Staging

## dbt-external-tables Setup

The backbone of the whole process of ingesting and refreshing data sources coming from S3 (and not as Snowflake Shares) is powered by the [dbt-external-tables](https://github.com/dbt-labs/dbt-external-tables) package.

## How to install the package

Simply include it in your packages.yml file within your project directory

```yml
packages:
 - package: dbt-labs/dbt_external_tables
    version: [">=0.7.0", "<0.9.0"]
```
and run `dbt deps` to install.

## Configuration of sources

External table mappings are defined in YAML configs for each table under the appropriate grouping (e.g. `source1/`, `source2/`).

Store your AWS credentials in your environment variables. For example, in your `./venv/bin/activate` file:

```bash
export AWS_KEY_ID=<your_aws_access_key_id>
export AWS_SECRET_KEY=<your_aws_secret_access_key>
```

### Step 1. Create an external stage pointing to the S3 bucket with the source files

```sql
  CREATE STAGE IF NOT EXISTS order_lines_data
      URL = 's3://path/to/your/bucket/folder'
      CREDENTIALS = (
          AWS_KEY_ID = '{{ env_var('AWS_KEY_ID') }}'
          AWS_SECRET_KEY = '{{ env_var('AWS_SECRET_KEY') }}'
```
### Step 2. Create file format for source files

```sql
  CREATE FILE FORMAT IF NOT EXISTS csv_format
      TYPE = CSV
      SKIP_HEADER = 1
      SKIP_BLANK_LINES = TRUE
      FIELD_OPTIONALLY_ENCLOSED_BY = '"'
      DATE_FORMAT = 'YYYY-MM-DD' ;
```
### Step 3. Populate YAML mappings

Each table per source company shuld follow the template below:

```yml
version: 2

sources:
  - name: your_source_name  #name from which we read the source w/ {{ source('your_name_source', xxx) }}
    database: RAW       #target database
    schema: SCHEMA_NAME       #target schema
    loader: s3      #identifier (optional)
    quoting:        #conventions (optional)
      database: true
      schema: true
      identifier: true

    tables:         #here you start to define your tables associated with the source
      - name: transactions      #reference name when sourcing with {{source('your_source_name', 'transactions')}}
        identifier: ORDER_LINES_DATA_RAW      #output table name in Snowflake
        description: transactions data stored as csv files on s3
        external:
          location: '@order_lines_data'       #stage reference in Step1.
          pattern: ".*transactions.csv"             #pattern of target files in S3 (optional)
          file_format: "(format_name = csv_format)"         #format reference in Step 2.
          auto_refresh: true        #auto metadata refresh with AWS SQS (optional)
          partitions:       # optional
            - name: collector_hour
              data_type: timestamp
              expression: to_timestamp(substr(metadata$filename, 8, 13), 'YYYY/MM/DD/HH24')

        # Specify ALL column names + datatypes.
        # Column order must match for CSVs, column names must match for other formats.
        # Some databases support schema inference (Snowflake's schema detection is only in private preview as of 11/18/2021).

        columns:       
          - name: store_name
            data_type: varchar(255)
            description: ""
          - name: channel
            data_type: varchar(255)
            description: ""
          - name: user_id
            data_type: varchar(255)
            description: ""
            ...
```
### Step 4. Load external table from S3 stage

Option 1.: Refresh every datasource

```bash
# iterate through all source nodes, create if missing, refresh metadata
$ dbt run-operation stage_external_sources

# iterate through all source nodes, create or replace (+ refresh if necessary)
$ dbt run-operation stage_external_sources --vars "ext_full_refresh: true"
```
Option 2.: Refresh specific ones

```bash
# stage all Snowplow and Logs external sources:
$ dbt run-operation stage_external_sources --args "select: snowplow logs"

# stage a particular external source table:
$ dbt run-operation stage_external_sources --args "select: snowplow.event"
```
>Note: You can also combine the arguments listed above in both options

After initiating the macros to source form the YAML files, you should see something like this:

```bash
16:27:43 + 3 of 7 START external source ORDER_LINES_DATA_RAW
16:27:44 + 3 of 7 (1) create or replace external table "RAW"."SOURCE...  
16:27:45 + 3 of 7 (1) SUCCESS 1
```

### Step 5. Auto-refresh via AWS SQS and Snowpipe

Once you are done adding your external table, head to the AWS Console to set up your auto-ingesting via Snowpipe and SQS notifications.

1. Retrieve ARN notification string assigned to the external table

```sql
SHOW EXTERNAL TABLES 
--from the output, copy arn field
```

2. Head to the staged bucket
3. Navigate to bucket `Properties`
4. Check `Event Notification` pane and create a new one
5. Select `ObjectCreate (All)` and `ObjectRemoved` event sensors
6. Set the destination to SQS queue, and Enter SQS queue ARN
7. Paste the string retrieved from Snowflake

Check out further resources on the dbt-external-tables package:
- https://github.com/dbt-labs/dbt-external-tables
- https://medium.com/slateco-blog/doing-more-with-less-usingdbt-to-load-data-from-aws-s3-to-snowflake-via-external-tables-a699d290b93f
- https://stackoverflow.com/questions/62992304/querying-with-dbt-from-external-source
- https://docs.snowflake.com/en/user-guide/data-load-s3-create-stage.html
- https://docs.snowflake.com/en/sql-reference/sql/create-file-format.html
- https://docs.snowflake.com/en/user-guide/tables-external-s3.html

## Ingest Data from Snowflake Shares

Ingesting data to the warehouse from a Snowflake share is much more simple.

1. Include data share (or view name) in the `sources.yml` confiuration with the same logic as in the case of S3 ingestion, but the database and schema should be defined as per the share object.

```yml
  - name: template
    database: TEMPLATE
    schema: SHARED_DATA
    quoting:
      database: true
      schema: true
      identifier: true

    tables:
      - name: ACCOUNTS
        identifier: ACCOUNTS_HIERARCHY
      - name: BOOKINGS
        identifier: BOOKINGS
      - name: PRODUCT_EVENTS
        identifier: PRODUCT_EVENTS
      - name: FINANCE_TRANSACTIONS
        identifier: TRANSACTIONS
      - name: USER_SIGNUPS
        identifier: USER_SIGNUPS
      - name: MARKETING_CHANNELS
        identifier: MARKETING_HIERARCHY
      - name: TRANSACTIONS
        identifier: '-'
      - name: ORDERS
        identifier: '-'
      - name: STORES
        identifier: '-'
      - name: WEB_TRAFFIC
        identifier: '-'
      - name: CUSTOMERS
        identifier: '-'
```
