# Linters
The following article is a good starting point to learn about the SQL style 
used by most dbt projects:

https://docs.getdbt.com/best-practices/how-we-style/2-how-we-style-our-sql

The linting/formatting tools available in dbt cloud are outlined in this article 
https://docs.getdbt.com/docs/cloud/dbt-cloud-ide/lint-format

We prefer to use SQLFluff because it offers more customization options than sqlfmt.
There are plenty of other SQL formatting options, but for interoperabilty with users
using dbt cloud it is preferred to use the tools available there.

For dbt-core + vscode development environment the following extension is recommended:
https://marketplace.visualstudio.com/items?itemName=dorzey.vscode-sqlfluff
Follow the dbt setup instructions in the above link.


## SQLFluff configurations
Place the following in a file named .sqlfluff in the root of your dbt project
replace <dialect> with your db engine 
```yaml
[sqlfluff]
dialect = <dbengine>
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
