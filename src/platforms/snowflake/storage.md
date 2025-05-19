# 💾 Storage
This section covers how data is stored, protected and managed in Snowflake, including table types, data retention and other advanced features.

### 📋 Overview
- [🛡️ **Table Types**](./st_table_types.md) → Permanent vs. Transient tables and best practices for dbt
- [🗄️ **Data Protection & Retention**](./st_storage_related_features.md) → Time Travel and Fail-safe and how they impact cost
- [🧩 **Snowflake-specific advanced features**](./st_advanced_features.md) → Secure Data Sharing and Zero-Copy Clone for data collaboration and environment cloning

## ✅ Practical Summary
- dbt defaults to using transient table to minimize cost
- Time Travel and Fail-safe offer data protection, but can increase storage costs
- Use `SNOWFLAKE.ACCOUNT_USAGE.TABLE_STORAGE_METRICS` to monitor storage consumption
- Full-refresh models recreate tables after each run and typically do not require extended retention features
- Permanent tables are recommended only for critical incremental models
- Snowflake provides advanced features like Secure Data Sharing and Zero-Copy Clone for collaboration and testing workflows