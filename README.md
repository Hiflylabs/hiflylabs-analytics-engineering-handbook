# axe-dbt-standards
dbt best practices and standards used in the AXE team at Hiflylabs.

**NOTE: This is a work in progress and only applicable for projects powered by Snowflake/BigQuery.**

# Table of Contents
1. [Onboarding](#onboarding)
2. [Dev Tools](#dev-tools)
3. [Branching](#branching)
4. [Pre-commit](#pre-commit)
    - [SQLFluff](#sqlfluff)
    - [YAML Check](#yaml-check)
5. [CI Pipeline](#ci-pipeline)
6. [Deployment](#deployment)
    - [Snowflake](#snowflake)
    - [BigQuery](#bigquery)
7. [Testing](#testing)
8. [dbt Cloud](#dbt-cloud)
9. [Cost Monitoring](#cost-monitoring)


## Onboarding

### Greenfield

1. Make sure that you use a virtual environment to make it easier navigating between projects with different dependencies

```bash
python3 -m venv venv
source venv/bin/activate
```
> Make sure you add `venv` to `.gitignore` 

2. Install dbt dependencies

```bash
pip3 install dbt-core dbt-snowflake dbt-bigquery
```

Add `requirements.txt`

```bash
dbt-core==1.2.0
dbt-snowflake==1.2.0
dbt-bigquery==1.2.0
pre-commit==2.19.0
sqlfluff==1.2.0
sqlfluff-templater-dbt==1.2.0
dbt-osmosis
```

3. Initialize dbt project

```
dbt init <project_name>
```

### Brownfield

**Mach Speed: Copy-Edit-Paste**

> Prerequisities: Python >= 3.5

*In case the project is not brownfield and you have already a repo and dependency*

Mac/Linux

```bash
git clone https://github.com/<org>/<repo>.git
cd <repo>
python3 -m venv venv
source venv/bin/activate
python3 -m pip install --upgrade pip
python3 -m pip install -r requirements.txt
source venv/bin/activate
dbt build
dbt docs generate
dbt docs serve
```

Windows

```bash
git clone https://github.com/<org>/<repo>.git
cd <repo>
python -m venv venv
venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
venv\Scripts\Activate.ps1
dbt build
dbt docs generate
dbt docs serve
```

3. store credentials and secrets in the environment by adding the following to your `./venv/bin/activate` file

### Snowflake:

```bash
export SNOWFLAKE_USER=<your_snowflake_user>
export SNOWFLAKE_PASSWORD=<your_snowflake_password>
export SNOWFLAKE_ACCOUNT=<your_snowflake_account>
```
### BigQuery:

```bash
export BIGQUERY_PROJECT=<your_bigquery_project_id>
```

4. Setup your profile and project file

### Snowflake

For more, consult the [dbt docs](https://docs.getdbt.com/reference/warehouse-profiles/snowflake-profile)

```yaml
sf_project_name:
  outputs:
    dev:
      account: "{{ env_var('SNOWFLAKE_ACCOUNT') }}"
      database: analytics_dev
      password: "{{ env_var('SNOWFLAKE_PASSWORD') }}"
      query_tag: dbt
      role: TRANSFORMER
      schema: <your personal schema name>
      threads: 4
      type: snowflake
      user: "{{ env_var('SNOWFLAKE_USER') }}"
      warehouse: TRANSFORMING
  target: dev
```

### BigQuery

For more consult the [dbt docs](https://docs.getdbt.com/reference/warehouse-profiles/bigquery-profile)

```yaml
bq_project_name:
  outputs:
    dev:
      dataset: <your developer dataset>
      location: <your dataset location>
      method: oauth
      priority: interactive
      project: "{{ env_var('SNOWFLAKE_PASSWORD') }}"
      retries: 1
      threads: 4
      timeout_seconds: 300
      type: bigquery
  target: dev
```

5. Check your connections

```bash
dbt debug
```
## Dev Tools

### VSCode plugins

Make sure you check in the util readme if you need to configure IDE settings to make them work!

- [vscode-dbt](https://marketplace.visualstudio.com/items?itemName=analyst-snowflake.vscode-dbt)
- [dbt Power User](https://marketplace.visualstudio.com/items?itemName=analyst-collective.dbt-power-user)
- [dbt-osmosis](https://github.com/z3z1ma/dbt-osmosis)
- [CSV Viewer tool](https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)

## Branching

For branching we use the [git flow](https://nvie.com/posts/a-successful-git-branching-model/) branching model. This means that we have two main branches: `master` and `develop`. The `master` branch is used for production deployments and the `develop` branch is used for staging deployments. For each feature, we create a new branch from `develop` and merge it back into `develop` once it's ready to be deployed to staging. Once we are ready to deploy to production, we merge `develop` into `master`.

For a further staging layer, please consult [this](https://docs.getdbt.com/blog/the-case-against-git-cherry-picking) blogpost.

## Pre-commit
### SQLFluff
### YAML Check
## CI Pipeline
## Deployment
### Snowflake
### BigQuery
## Testing
## dbt Cloud
## Cost Monitoring
