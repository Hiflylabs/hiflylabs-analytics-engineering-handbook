# 📐 Pre-commit

[What is precommit?](https://pre-commit.com/)

### Installation

```bash
pip3 install pre-commit
```

We are mainly using the following four repos:
- [Pre-commit hooks](https://github.com/pre-commit/pre-commit-hooks)
- [SQLFluff](https://docs.sqlfluff.com/en/stable/production.html)
- [pre-commmit-dbt](https://github.com/Montreal-Analytics/dbt-gloss)
- [sqlfmt](http://sqlfmt.com/)


### Configuration

Add to `project/.pre-commit-config.yaml`

```yaml
repos:
- repo: https://github.com/Montreal-Analytics/dbt-gloss
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
```
### Initialization

```bash
pre-commit install
```