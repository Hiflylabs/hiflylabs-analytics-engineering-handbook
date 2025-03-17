# Managing YML configurations

- dbt package: [dbt-codegen](https://github.com/dbt-labs/dbt-codegen)
- pip module: [dbt-osmosis](https://github.com/z3z1ma/dbt-osmosis)


## dbt-codegen
### What is dbt-codegen
It is a dbt packages containing macros for generating basic yaml files and dbt models. The macros can be used from a terminal by running the `dbt run-operation <macro_name> --args '{"<key>": "<value>"}'` command.

### Main features
- Generate source yaml files based on the database schema (`generate_source`)
- Generate base models (select + rename) (`generate_base_model`, `base_model_creation`)
- Generate model yaml files based on the corresponding database object (`generate_model_yaml`)
- Generate boilerplate sql for importing referenced models as CTEs (`generate_model_import_ctes`)

### Usage note 
- Calling the macros using the `dbt run-operation` command will output the result to the terminal, you have to manually create the corresponding yaml files and then copy (or pipe) the generated content into them
- Using the `generate_model_yaml` macro, columns can inherit their description from upstream models (when the column name is matching with the upstream column)

## dbt-osmosis
### What is dbt-osmosis
It is a python based tool that can be used for automated yaml file generation and management, it can speed up development, automate documentation maintenance and ensure consistency across your dbt project.

### Main features
- Create yaml files for undocumented sources or models
- Move and merge yaml files based on the configuration
- Add or remove columns based on the corresponding database object
- Order columns or models in yaml files based on configured rules
- Inherit tags, descriptions and meta fields from upstream columns (based on matching column names)
- Directly compile or run jinja SQL snippets (workbench - Streamlit application, REPL like environment to run dbt models against your database)

### Usage notes
- Configuration is written into the `dbt_project.yml` file
    - For sources, it can be defined under `vars.dbt-osmosis.sources`
    - For models, using the `+dbt-osmosis` and `+dbt-osmosis-options` keys under `models.<dbt project name>`
- The yaml file management strategy is highly configurable (eg. one file per model / folder, file name, file path)
- It can be run on-demand from a terminal or in an automated way as a pre-commit hook or part of the CI pipeline

## dbt-codegen vs dbt-osmosis
| dbt-codegen | dbt-osmosis |  
|---|---|
|  manual usage | manual or automated usage  |
|  creates text output, doesn't modify or manage files | creates and modifies yaml files  |
|  config passed as arguments in the terminal | config saved into `dbt_project.yml`  |