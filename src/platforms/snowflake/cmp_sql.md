## 🧩 Special SQL Features

Snowflake offers several unique SQL functions that can simplify queries and improve efficiency.

A few examples:
- [**`QUALIFY`**](https://docs.snowflake.com/en/sql-reference/constructs/qualify) → Post-filter window functions directly in the `SELECT` statement
- [**`MAX_BY()`**](https://docs.snowflake.com/en/sql-reference/functions/max_by) → Retrieve the maximum value along with another column
- [**`EXCLUDE`**](https://docs.snowflake.com/en/sql-reference/sql/select#selecting-all-columns-except-one-column) → Easily exclude columns from a `SELECT` without manually listing them
- [**`HLL (HyperLogLog)`**](https://docs.snowflake.com/en/sql-reference/functions/hll) → Approximate distinct count, useful for very large datasets