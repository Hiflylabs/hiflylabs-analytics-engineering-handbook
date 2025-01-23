# Paradime.io

## Github Action to verify paradime schedules.
Invalid paradime schedule.yml configurations could break Bolt configuration. Therefore we validate the schedule configurations.

```yml
name: Check paradime_schedule.yml Changes

# Trigger the workflow when code is pushed to any branch
on:
  push:
    paths:
      - paradime_schedules.yml

jobs:
  run_script:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11.6' 

      - name: Install Paradime CLI
        run: |
          python -m pip install --upgrade pip
          pip install paradime-io==4.6.1
      - name: Verify Schedule
        run: |
          paradime bolt verify
```