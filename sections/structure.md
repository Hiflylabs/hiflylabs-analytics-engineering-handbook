# How we structure and style our projects

We follow the [Matt Mazur SQL style guide](https://github.com/mattm/sql-style-guide) and the one by [Fishtown Analytics for dbt-specific behaviors](https://github.com/fishtown-analytics/corp/blob/master/dbt_coding_conventions.md#sql-style-guide).

We also follow the [best practices documented on the dbt website](https://docs.getdbt.com/docs/guides/best-practices/).

**We LOVE CTEs, we don’t use subqueries!**

**We LOVE trailing commas!**

SQL and YAML styles are enforced by linters that runs automatically before any commit.

1. https://github.com/sqlfluff/sqlfluff
2. https://github.com/adrienverge/yamllint

Find a good project checklist [here](https://discourse.getdbt.com/t/your-essential-dbt-project-checklist/1377)

## Other great resources to check out

- [How we structure our dbt projects](https://discourse.getdbt.com/t/how-we-structure-our-dbt-projects/355)
- [How we configure Snowflake](https://blog.getdbt.com/how-we-configure-snowflake/)
- [The Ultimate Guide to Using dbt With Snowflake](https://medium.com/geekculture/the-ultimate-guide-to-using-dbt-with-snowflake-2d4bfc37b2fc)
- [Snowflake-Terraform-bootstrap project](https://github.com/Hiflylabs/snowflake-terraform-boostrap)
- [How to set up a dbt data-ops workflow, using dbt cloud and Snowflake](https://www.startdataengineering.com/post/cicd-dbt/)