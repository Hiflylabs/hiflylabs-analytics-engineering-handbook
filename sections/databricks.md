# Databricks - dbt

## Setup

### Databricks

__Databricks Platform__:

Can run on AWS, Azure, GCP
 - Running on Azure: Called Azure Databricks, provided by Microsoft
 - Enterprise Edition only available on AWS

Pricing:
 - [AWS](https://www.databricks.com/product/aws-pricing)
 - [Azure](https://www.databricks.com/product/azure-pricing)
 - [GCP](https://www.databricks.com/product/gcp-pricing)

Generally AWS provides the most features, Azure is the most expensive option.

__Community Edition__:

Free community tool for exploration and learning. Runs on AWS. You don't have to pay neither for Databricks nor the the cloud resources that you use.
Supports only limited size clusters, and not all features are available.
eg.: Workflows,
[Databricks SQL](https://community.databricks.com/s/question/0D58Y00008hcu0KSAQ/databricks-community-edition-enable-databricks-sql),
token generation

## Unity Catalog

Enable unity catalog for better data management. Guide: https://docs.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/get-started

Unity catalog notebook: https://docs.microsoft.com/en-us/Azure/Databricks/_static/notebooks/unity-catalog-example-notebook.html

## dbt

Vendor supprted adapter: https://github.com/databricks/dbt-databricks

setup guide: https://github.com/databricks/dbt-databricks/blob/main/docs/local-dev.md

You can run dbt models aganst Databricks SQL endpoints or Databricks clusters. Running against SQL endpoint is recommended
> as they provide the latest SQL features and optimizations.

Databricks SQL endpoints (warehouses) is _not supperted_ in the commuity edition and the Standard Edition. That leaves us with Premium or Enterprise Editions.

You can't create a PAT (personal access tokens) for community edition.

## Default setup and learnings

- Once databricks is set up correctly connecting dbt is quite straightforward.

- The Databricks SQL warehouse is a [Hive](https://hive.apache.org/) instance.

- Tables are stored as parquet files on the DBFS (Databricks File System).

- Clusters can access these files and tables through spark.

- Won't work with dbt audit helper paackage. Some macros are not implemented

## Loading data to databricks

There are several ways to load data to databricks.
It is supported by most of the EL tools:
 - [Fivetran](https://fivetran.com/docs/destinations/databricks)
 - [Airbyte](https://airbyte.com/connectors/databricks-1)

There are multiple other ways to get data into databricks.

You can upload files, create tables from data on DBFS that you can manipuléate through Spark jobs or use a Connector.
Available Connectors depend on the cloud service that Dataricks is running on.
For Azure Databricks these are:
 - Azure Blob Storage
 - Azure Data Lake Store
 - Cassandra
 - JDBC
 - Kafka
 - Redis
 - Elasticsearch

Databricks has example notebooks that show how you can load from these sources into Databricks.

### Connecting to Azure

When running Databricks on Azure several ways are provided to conviently load data that's residing on a Data Lake in Azure.

This is explained here: https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/azure/azure-storage
You'll need to create a key vault in Azure to store the keys that databricks uses to access Azure.


## Administration

https://www.databricks.com/blog/2022/08/26/databricks-workspace-administration-best-practices-for-account-workspace-and-metastore-admins.html

![admin](https://cms.databricks.com/sites/default/files/inline-images/db-320-blog-img1.png)

|                            |                                                                             Account Admin                                                                            |                                                                                                                     Metastore Admin                                                                                                                    |                                                                                                                                                                                                                                                                                         Workspace Admin                                                                                                                                                                                                                                                                                         |   |
|:--------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|---|
| Workspace Management       | - Create, Update, Delete workspaces - Can add other admins                                                                                                           | Not Applicable                                                                                                                                                                                                                                         | - Only Manages assets within a workspace                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |   |
| User Management            | - Create users, groups and service principals or use SCIM to sync data from IDPs. - Entitle Principals to Workspaces with the Permission Assignment API              | Not Applicable                                                                                                                                                                                                                                         | - We recommend use of the UC for central governance of all your data assets(securables). Identity Federation will be On for any workspace linked to a Unity Catalog (UC) Metastore. - For workspaces enabled on Identity Federation, setup SCIM at the Account Level for all Principals and stop SCIM at the Workspace Level. - For non-UC Workspaces, you can SCIM at the workspace level (but these users will also be promoted to account level identities). - Groups created at workspace level will be considered "local" workspace-level groups and will not have access to Unity Catalog |   |
| Data Access and Management | - Create Metastore(s) - Link Workspace(s) to Metatore - Transfer ownership of metastore to Metastore Admin/group                                                     | With Unity Catalog: -Manage privileges on all the securables (catalog, schema, tables, views) of the metastore - GRANT (Delegate) Access to Catalog, Schema(Database), Table, View, External Locations and Storage Credentials to Data Stewards/Owners | - Today with Hive-metastore(s), customers use a variety of constructs to protect data access, such as Instance Profiles on AWS, Service Principals in Azure, Table ACLs, Credential Passthrough, among others. -With Unity Catalog, this is defined at the account level and ANSI GRANTS will be used to ACL all securables                                                                                                                                                                                                                                                                     |   |
| Cluster Management         | Not Applicable                                                                                                                                                       | Not Applicable                                                                                                                                                                                                                                         | - Create clusters for various personas/sizes for DE/ML/SQL personas for S/M/L workloads - Remove allow-cluster-create entitlement from default users group. - Create Cluster Policies, grant access to policies to appropriate groups - Give Can_Use entitlement to groups for SQL Warehouses                                                                                                                                                                                                                                                                                                   |   |
| Workflow Management        | Not Applicable                                                                                                                                                       | Not Applicable                                                                                                                                                                                                                                         | - Ensure job/DLT/all-purpose cluster policies exist and groups have access to them - Pre-create app-purpose clusters that users can restart                                                                                                                                                                                                                                                                                                                                                                                                                                                     |   |
| Budget Management          | - Set up budgets per workspace/sku/cluster tags - Monitor Usage by tags in the Accounts Console (roadmap) - Billable usage system table to query via DBSQL (roadmap) | Not Applicable                                                                                                                                                                                                                                         | Not Applicable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |   |
| Optimize / Tune            | Not Applicable                                                                                                                                                       | Not Applicable                                                                                                                                                                                                                                         | - Maximize Compute; Use latest DBR; Use Photon - Work alongside Line Of Business/Center Of Excellence teams to follow best practices and optimizations to make the most of the infrastructure investment                                                                                                                                                                                                                                                                                                                                                                                        |   |

## Partner connect

Databricks offers easy integration with some of its partners

## Working with dbt

SQL Warehouses in Databricks are configured to shut down after being idle for some time.
If you want to run your models against a warehouse that is not running at the moment you'll have to wait until it starts. This is triggered automatically, however it takes some time as Databricks needs to spin up cloud instances.

## Chossing the right cluster

![cluster](https://cms.databricks.com/sites/default/files/inline-images/db-320-blog-img-6.png)
