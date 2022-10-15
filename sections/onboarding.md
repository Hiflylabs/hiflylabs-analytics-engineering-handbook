# Onboarding

## 🟢 Greenfield

1. dbt requires a functional Python environment on your workstation. It's best to avoid Python that comes pre-installed with your OS, and we recommend creating a virtual environment dedicated to the project. This can be done using any of Python's virtual environment tools, although **we recommend virtualenv** for ease of use.

> Use the latest version of Python 3.10 if possible.

**Mac:**

There are many ways to install Python on a Mac. We recommend using [Homebrew](https://brew.sh/):

1. Install Homebrew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Install Python 3.10: `brew install python@3.10`
3. Install Virtualenv: `pip3 install virtualenv`
4. Go to the directory where you store your codebases. If you don't have any, we recommend creating a `~/Code` directory.
5. Clone the git repository. If the code is store on GitHub, we recomment using the [GitHub CLI](https://cli.github.com/)
6. Go to the `project` directory.
7. Create a virtual environment: `python3 -m venv .venv`
8. Activate the virtual environment: `source .venv/bin/activate`

**Windows:**

0. CLI Based installer for Git, Python dbt, VS-Code: [https://github.com/Hiflylabs/dbt-develop-install](https://github.com/Hiflylabs/dbt-develop-install)
1. Install Python 3.10: [https://www.python.org/downloads/release/python-3106/](https://www.python.org/downloads/release/python-3106/)
2. Install Virtualenv: `python -m pip install virtualenv`
3. Go to the directory where you store your codebases. If you don't have any, we recommend creating a `C:\Users\YourName\dbt_projects\` directory.
4. Clone the git repository. If the code is store on GitHub, we recomment using the [GitHub CLI](https://cli.github.com/)
5. Go to the `project` directory.
6. Create a virtual environment: `python -m venv .venv`
7. Activate the virtual environment: `.\.venv\Scripts\activate`

> Make sure you add your virtualenv directory to `.gitignore` 

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

## 🟤 Brownfield

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

### ❄️ Snowflake:

```bash
export SNOWFLAKE_USER=<your_snowflake_user>
export SNOWFLAKE_PASSWORD=<your_snowflake_password>
export SNOWFLAKE_ACCOUNT=<your_snowflake_account>
```
### 🔍 BigQuery:

```bash
export BIGQUERY_PROJECT=<your_bigquery_project_id>
```

4. Setup your profile and project file

### ❄️ Snowflake

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

### 🔍 BigQuery

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
