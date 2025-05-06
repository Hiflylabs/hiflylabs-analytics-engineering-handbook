## 🧩 Snowflake-specific features

*Snowflake offers several advanced platform-native features that can be valuable for specific use cases:*

[**Secure Data Sharing**](https://docs.snowflake.com/en/user-guide/data-sharing-intro)
- Share data between Snowflake accounts without copying or moving data
- Enables real-time collaboration across business units or regions
- Available across most editions (advanced sharing options may require Enterprise license)

[**Zero-Copy Clone**](https://docs.snowflake.com/en/user-guide/tables-storage-considerations#label-cloning-tables)
- Instantly create copies of databases, schemas or tables without duplicating storage
- Clones are metadata pointers and free until changes are made (copy-on-write model)
- Useful for testing, backups or branching environments (CI/CD)