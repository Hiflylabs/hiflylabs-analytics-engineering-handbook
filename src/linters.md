# Linters

https://docs.getdbt.com/best-practices/how-we-style/2-how-we-style-our-sql
https://docs.getdbt.com/docs/cloud/dbt-cloud-ide/lint-format
https://marketplace.visualstudio.com/items?itemName=dorzey.vscode-sqlfluff

SQL and YAML styles are enforced by linters that runs automatically before any commit.

1. https://github.com/sqlfluff/sqlfluff
2. https://github.com/adrienverge/yamllint

## Dialect specific SQLFluff configurations 
```yaml
[sqlfluff]
dialect = bigquery
templater = dbt
runaway_limit = 10
max_line_length = 120
indent_unit = space

[sqlfluff:templater:jinja]
apply_dbt_builtins = True

[sqlfluff:indentation]
tab_space_size = 4
indented_using_on = False
allow_implicit_indents = True

[sqlfluff:layout:type:comma]
spacing_before = touch
line_position = trailing

[sqlfluff:rules:capitalisation.keywords]
capitalisation_policy = lower

[sqlfluff:rules:aliasing.table]
aliasing = explicit

[sqlfluff:rules:aliasing.column]
aliasing = explicit

[sqlfluff:rules:aliasing.expression]
allow_scalar = False

[sqlfluff:rules:capitalisation.identifiers]
extended_capitalisation_policy = lower

[sqlfluff:rules:capitalisation.functions]
capitalisation_policy = lower

[sqlfluff:rules:capitalisation.literals]
capitalisation_policy = lower

[sqlfluff:rules:ambiguous.column_references]  # Number in group by
group_by_and_order_by_style = consistent

```
### Snowflake
### BigQuery