# Troubleshooting

## General

To debug an error, consult first the [documentation](https://docs.getdbt.com/guides/legacy/debugging-errors).

## dbt-osmosis

This is a must have to preview model excerpts and test out blocks of a complex logic causing a test to fail or wrong data in the output.

Repo [here](https://github.com/z3z1ma/dbt-osmosis)

## Store Test Failures

We are able to write test failures and warnings directly into our datawarehouse. It helps in identifying records responisble for the anomaly.

Add to `dbt_project.yml`

Optional schema parameter will store audit tables in `target.schema_audit` schema

```yaml
tests:
  +store_failures: true
  +schema: 'audit'
```

For more check out the official [documentation](https://docs.getdbt.com/reference/resource-configs/store_failures)
