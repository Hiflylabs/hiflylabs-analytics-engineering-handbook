## ⚙️ Warehouses: Sizing and Threads

**Choosing the right warehouse size is critical for performance and cost efficiency.**

Snowflake offers warehouses in T-shirt sizes (X-Small to 6X-Large), but exact hardware specifications are not public.

[**Best practices for warehouse sizing:**](https://select.dev/posts/snowflake-warehouse-sizing)
- Start with **X-Small**
- Increase size until **query durations stop halving**. At that point, you've likely found the cost-effective size
- There can be hidden costs and uptime, so avoid relying solely on runtime seconds and theory
  - Be cautious when increasing to XL sizes, for instance
- Doubling warehouse size **does not always** halve query time

**Made-up example for illustrating warehouse cost:**

| Warehouse Size | Runtime | Credits per minute | Total Credits Consumed |
|----------------|---------|--------------------|------------------------|
| Small (S) | 10 min | 1 | 10 |
| Medium (M) | 5 min | 2 | 10 |

In this example, running a query on a **Small** warehouse takes 10 minutes and consumes 1 credit per minute, resulting in 10 total credits.  
Running the same query on a **Medium** warehouse takes only 5 minutes but consumes 2 credits per minute, leading to the **same total credit usage**, with less time.  
In this case, using the Medium warehouse offers the same cost **and** better runtime!

**Key takeaway**: Larger warehouses can reduce runtime without increasing total cost, as long as the performance improvement is proportional to the size increase.

⚠ Applying this method to complex DAGs and pipelines can be more challenging than for single queries

**Threads**:  
| Warehouse Size | Recommended threads |
| --- | --- |
| X-Small | 4–8 |
| Small | 8–16 |
| Medium or larger | 16+ (monitor for queuing) |

Increasing threads beyond a warehouse's concurrency limit causes **query queuing**, increasing total runtime and delaying DAG execution.  

⚠ Over-threading (too many dbt threads) can cause query queuing and may slow down execution rather than speeding it up

📊 Use Snowflake's **Query History** view to check if your warehouse is fully utilized or if queries are queuing

[**Auto-suspend**](https://docs.snowflake.com/en/user-guide/warehouses-overview#auto-suspension-and-auto-resumption) should be configured to 60 seconds or less to avoid paying for idle compute when the warehouse is not actively processing queries.