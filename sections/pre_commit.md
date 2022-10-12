# Pre-commit

### Installation

```bash
pip3 install pre-commit
```

We are mainly using the following three repos:
- [Pre-commit hooks](https://github.com/pre-commit/pre-commit-hooks)
- [SQLFluff](https://docs.sqlfluff.com/en/stable/production.html)
- [pre-commmit-dbt](https://github.com/offbi/pre-commit-dbt)


### Configuration

Add to `.pre-commit-config.yaml`

```yaml
repos:
- repo: https://github.com/offbi/pre-commit-dbt
  rev: v1.0.0
  hooks:
  - id: check-script-semicolon
  - id: check-script-has-no-table-name
  - id: dbt-test
  - id: dbt-docs-generate
  - id: check-model-has-all-columns
    name: Check columns - core
    files: ^models/core
  - id: check-model-has-all-columns
    name: Check columns - mart
    files: ^models/mart
  - id: check-model-columns-have-desc
    files: ^models/mart
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v4.0.1
  hooks:
    - id: trailing-whitespace
    - id: end-of-file-fixer
    - id: check-yaml
- repo: https://github.com/sqlfluff/sqlfluff
  rev: 1.2.0
  hooks:
    - id: sqlfluff-lint
      files: 'models/'
      additional_dependencies: ['dbt-snowflake==1.2.0', 'sqlfluff-templater-dbt']
    - id: sqlfluff-fix
      files: 'models/'
      additional_dependencies: ['dbt-snowflake==1.2.0', 'sqlfluff-templater-dbt']
```
### Initialization

```bash
pre-commit install
```