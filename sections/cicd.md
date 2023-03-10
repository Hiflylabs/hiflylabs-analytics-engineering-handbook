# 🪡 CI/CD

### Standard Approach

To setup a the Github integration, you have to have an admin account on the Github repository. This is because you need to add a webhook to the repository. This webhook will trigger the CI pipeline when a new commit is pushed to the repository.

Find the official documentation [here](https://docs.getdbt.com/docs/dbt-cloud/using-dbt-cloud/cloud-enabling-continuous-integration)

### ZCC CI

The zero-copy-clone CI creates a mirror environment of the target database and builds the models in that environment, ensuring that there are no missing external references in the environment.

Inspired by the idea from [here](https://medium.com/airtribe/test-sql-pipelines-against-production-clones-using-dbt-and-snowflake-2f8293722dd4)

```sql
{% macro clone_prod_update_permissions(db, clone_db, role, schemas_to_clone) -%}

    {{ log("Cloning database " ~ db ~ " to " ~ clone_db, info=True) }}

    {{ log("The following schemas will be cloned: " ~ schemas_to_clone, info=True)}}

    {% call statement(name, fetch_result=True) %}

        DROP DATABASE IF EXISTS {{clone_db}};
        CREATE DATABASE {{clone_db}};
        {% for schema_name in schemas_to_clone %}
            {% if '_' in schema_name %}
                {% set custom = schema_name.split('_')[1] %}
                create schema {{target.schema}}_{{custom}} clone {{db}}.{{schema_name}};
            {% else %}
                create schema {{target.schema}} clone {{db}}.{{schema_name}};
            {% endif %}
        {% endfor %}
        GRANT ALL ON DATABASE {{clone_db}} TO ROLE {{role}};

        {# grant access for debugging #}
        GRANT USAGE ON DATABASE {{clone_db}} TO ROLE TRANSFORMER;

    {% endcall %}

    {{ log("Database cloning completed!", info=True) }}

{%- endmacro %}

{% macro pr_clone_pre_hook() %}

    {% if target.name == "dev_pr" %}

        {# If it is a PR from the feature bracnh against development, then clone the ANALYTICS_DEV #}
         {{ clone_prod_update_permissions(var('dev_db'), var('development_clone_db'), var('clone_role'), var('schemas_to_clone')) }}

    {% elif target.name == "prod_pr" %}

        {# If it is a PR from the development against main, then clone the ANALYTICS_PROD #}
        {{ clone_prod_update_permissions(var('prod_db'), var('production_clone_db'), var('clone_role'), var('schemas_to_clone')) }}

    {% else %}

        {{ log(target.name ~" is not meant to be used for PR testing", info=True) }}

    {% endif %}
  
{% endmacro %}
```

Add these to your `dbt_project.yml` file

```yaml
vars:
# Databases
  "dev_db" : "ANALYTICS_DEV"
  "prod_db" : "ANALYTICS_PROD"
  "production_clone_db" : "ANALYTICS_PROD_CLONE"
  "development_clone_db" : "ANALYTICS_DEV_CLONE"
  # Roles
  "clone_role" : "SYSADMIN"
  # Schemas to clone
  "schemas_to_clone" : ["ANALYTICS", "ANALYTICS_STAGING", "ANALYTICS_SEED", "ANALYTICS_RAW_SAMPLE"]
```

Override `generate_schema_name.sql` macro with the script below. This ensures that the CI runs within the cloned environment.

Ref: https://gist.github.com/jeremyyeo/759d8675f9b36abfa8ba462c32f7c3e3#snowflake-workflow

```sql
{% macro generate_schema_name(custom_schema_name, node) -%}

    {%- set default_schema = target.schema -%}

    {# forcing slim CI to use standard schemas avoiding dependency issues on the warehouse level #}

    {%- if target.schema[:9] == 'dbt_cloud' and custom_schema_name is none -%}

        {{ 'ANALYTICS' }}

    {%- elif target.schema[:9] == 'dbt_cloud' and custom_schema_name -%}

        {{ 'ANALYTICS_' ~ custom_schema_name | trim }}

    {%- elif custom_schema_name is none -%}

        {{ default_schema }}

    {%- else -%}

        {{ default_schema }}_{{ custom_schema_name | trim }}

    {%- endif -%}

{%- endmacro %}
```

### Limitations
Note that this can cause discrepancies if you run multiple PR as they can overwrite each other. This is why we recommend to test PRs one by one with this solution. The future implementation would create custom clone databases for each PR.
