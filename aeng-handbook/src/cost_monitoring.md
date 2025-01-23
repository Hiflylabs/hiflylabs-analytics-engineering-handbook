# 💰 Cost Monitoring

## 🥡 Out-of-the-box

[dbt_artifacts](https://github.com/brooklyn-data/dbt_artifacts)

There are built-in cost monitoring solutions in BI tools as well but they are not parsing the `manifest.json`, only checks warehouse metrics.

[Looker - Snowflake Cost and Usage](https://marketplace.looker.com/marketplace/detail/snowflake-cost-v2)

## Simplified (in-house) Artifacts

[dbt-snowflake-artifacts](https://github.com/Hiflylabs/dbt-snowflake-artifacts)

## 📊 How to set up Snowflake resource monitoring:

1. First, accountadmins should enable notifications
1. Use the ACCOUNTADMIN system role. If you aren’t, in the drop-down menu next to your name in the upper-right corner, select Switch role » ACCOUNTADMIN.
1. In the same drop-down menu, select Preferences » Notifications.
Select one of the options.

> You have to create the resource monitor first, then assign it to (multiple) warehouses.

 - Notify: sends mail
 - Suspend: stops the warehouse after queries have finished
 - Suspend immediately: stops all processes immediately

To create an account-level resource monitor that starts immediately (based on the current timestamp), resets monthly on the same day, has no end date or time, and suspends the assigned warehouse when the used credits reach 100% of the quota:

(you can also go beyond 100% when defining the resource monitor)

```sql
use role accountadmin;

create or replace resource monitor limit1 with credit_quota=1000
    frequency = monthly
    start_timestamp = immediately
    triggers on 100 percent do suspend;
alter warehouse wh1 set resource_monitor = limit1;
```

To create a resource monitor that starts at a specific date and time in the future, resets weekly on the same day, has no end date or time, and performs two different suspend actions at different thresholds:

```sql
use role accountadmin;

create or replace resource monitor limit1 with credit_quota=2000
    frequency = weekly
    start_timestamp = '<your-ts> PST'
    triggers on 60 percent do notify
             on 80 percent do suspend
             on 100 percent do suspend_immediate;

alter warehouse wh1 set resource_monitor = limit1;
```