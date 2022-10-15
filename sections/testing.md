# Testing

## Useful packages

- [dbt_utils](https://github.com/dbt-labs/dbt-utils#relationships_where-source)
- [dbt_expectations](https://github.com/calogica/dbt-expectations)
- [dbt-project-evaluator](https://github.com/dbt-labs/dbt-project-evaluator)
- [dbt-unit-testing](https://github.com/EqualExperts/dbt-unit-testing)

## Non-negotiables

Primary (surrogate) keys should always be:

- unique
- not null

Referential integrity tests:

```yml
models:
  - name: model_name
    columns:
      - name: pk
        tests:
          - dbt_utils.relationships_where:
              to: ref('other_model_name')
              field: client_pk
```

Currencies have to be the same for all transactions.

```yml
models:
  - name: transactions
    description: Interface table for transactions (payments)
        - name: currency
        tests:
            - not_null
            - accepted_values:
                values: ['USD']
```

Monetary, quanity values should be non-negative

```yml
models:
  - name: spending
    description: Spending table
    columns:
      - name: spend
        tests:
          - not_null
          - dbt_utils.accepted_range:
              min_value: 0
              inclusive: true
```

Date field can't be in the future (there can be special cases!)

```yml
models:
  - name: model_name
    tests:
      - dbt_utils.expression_is_true:
          expression: "created_at <= CURRENT_TIMESTAMP"
```

TODO: add more examples/conventions
