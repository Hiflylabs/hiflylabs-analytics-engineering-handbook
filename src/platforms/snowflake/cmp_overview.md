## 📋 Overview

Snowflake's compute power is provided by **virtual warehouses**, which are clusters of cloud-based resources (CPU, memory and temporary storage) managed entirely by Snowflake.

When a query runs, it uses the compute resources of an active warehouse.

The **underlying infrastructure (e.g., AWS EC2, Azure VM)** is fully abstracted. Users do not interact with or manage it.

Snowflake compute follows a **pay-as-you-go** pricing model:
- You are billed based on **warehouse size × active runtime**
- Charges are calculated in **credits** that reflect actual cloud costs

💡 Compute charges are separate from storage costs. You pay only for the time warehouses are active

More details: [Snowflake Pricing](https://www.snowflake.com/en/pricing-options/)