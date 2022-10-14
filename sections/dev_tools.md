# Dev Tools

We recommend using VS Code if you don't currently have a preference.
## VSCode plugins

Make sure you check in the util readme if you need to configure IDE settings to make them work!

- [vscode-dbt](https://marketplace.visualstudio.com/items?itemName=analyst-snowflake.vscode-dbt)
- [dbt Power User](https://marketplace.visualstudio.com/items?itemName=analyst-collective.dbt-power-user)
- [Rainbow CSV](https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Better Jinja](https://marketplace.visualstudio.com/items?itemName=samuelcolvin.jinjahtml)
- [Find Related Files](https://marketplace.visualstudio.com/items?itemName=patbenatar.advanced-new-file)
- [GitHub Pull Requests and Issues](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)
- [Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)
- [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

### Specific configurations (to be added in the settings.json of VS Code)

The following will remap Markdown, Yaml and SQL files to use the Jinja-flavoured interpreter:

```json
    "files.associations":{
        "*.md": "jinja-md",
        "*.yml": "jinja-yaml",
        "*.sql": "jinja-sql",
    },
```

The following will configure Related Files rules for raw / compiled models:

```json
"findrelated.rulesets": [
        {
            "name": "sql",
            "rules": [
                {
                    "pattern": "^(.*/)?models/(.*/)?(.+\\.sql)$",
                    "locators": [
                        "**/compiled/**/$3"
                    ]
                },
                {
                    "pattern": "^(.*/)?compiled/(.*/)?(.+\\.sql)$",
                    "locators": [
                        "**/run/**/$3"
                    ]
                },
                {
                    "pattern": "^(.*/)?run/(.*/)?(.+\\.sql)$",
                    "locators": [
                        "**/models/**/$3"
                    ]
                }
            ]
        }
    ],
```

The following will configure spacing/indentation rules to follow the style guide:

```json
    "[yaml]": {
        "editor.insertSpaces": true,
        "editor.tabSize": 2
    },
    "[sql]": {
        "editor.insertSpaces": true,
        "editor.tabSize": 4
    },
    "[jinja-yaml]": {
        "editor.insertSpaces": true,
        "editor.tabSize": 2
    },
    "[jinja-sql]": {
        "editor.insertSpaces": true,
        "editor.tabSize": 4
    },
    "editor.detectIndentation": false,
    "editor.rulers": [
        { "column": 80, "color": "#403558" }
    ],
```

## dbt packages

- [dbt-codegen](https://github.com/dbt-labs/dbt-codegen)
## pip modules

- [dbt-osmosis](https://github.com/z3z1ma/dbt-osmosis)