# ⚙️ Compute

This section explains how Snowflake's compute layer works, how costs are calculated and how to optimize warehouse configurations for both performance and budget.

### 📋 Overview
- [📋 **Overview**](./cmp_overview.md) → Introduction to Snowflake’s compute model, credit-based pricing and architecture basics
- [⚙️ **Warehouses: Sizing and Threads**](./cmp_wh.md) → How to select the right warehouse size, configure threads and optimize for cost and performance
- [🗂️ **Clustering and Micro-partitions**](./cmp_clustering.md) → How clustering improves query performance and when to use it
- [🚀 **Other Serverless Features**](./cmp_other_serverless.md) → Optional compute accelerators

## ✅ Practical Summary
- Snowflake compute is billed based on warehouse size and active runtime (credits per second)
- Start with the smallest warehouse (X-Small) and scale up until query duration stops halving
- Match the number of dbt threads to warehouse size. Avoid over-threading, which can cause query queuing and slower performance
- Set auto-suspend to 60 seconds or less to minimize idle compute costs
- Use clustering selectively on large, frequently filtered tables, and monitor reclustering credit usage closely
- Advanced serverless features like Query Acceleration and Multi-Cluster Warehouses can improve performance, but should be used cautiously and only when fully understood
- Leverage special SQL features to improve efficiency
- Continuously monitor performance and credit usage using in-house or third-party solutions