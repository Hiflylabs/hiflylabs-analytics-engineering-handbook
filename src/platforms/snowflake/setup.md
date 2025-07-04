## 🔧 Setup

### Setting up dbt with Snowflake:
- [dbt Snowflake adapter](https://github.com/dbt-labs/dbt-snowflake)
- [Official guide](https://docs.getdbt.com/docs/core/connect-data-platform/snowflake-setup)

### Authentication options:

[**Key pair authentication**](https://docs.getdbt.com/docs/core/connect-data-platform/snowflake-setup#key-pair-authentication)
- **This is the recommended method** for authenticating into Snowflake when using dbt
- It provides a secure setup suitable for automated workflows without requiring additional logins

[**User-password MFA**](https://docs.getdbt.com/docs/core/connect-data-platform/snowflake-setup#user--password-authentication)
- To avoid push notifications for every model build, ensure the ```ALLOW_CLIENT_MFA_CACHING``` parameter is set to ```TRUE``` at the [account level](https://docs.getdbt.com/docs/core/connect-data-platform/snowflake-setup#user--password--duo-mfa-authentication) when using user-password MFA
- Snowflake also supports other TOTP-based MFA methods (e.g. Google Authenticator). While suitable for one-time logins, they are not compatible with running dbt models