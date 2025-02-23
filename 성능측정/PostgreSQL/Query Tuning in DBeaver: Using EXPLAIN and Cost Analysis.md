# Query Tuning in DBeaver: Using EXPLAIN and Cost Analysis

DBeaver is a powerful database management tool that supports query analysis and optimization. One useful shortcut in DBeaver is **Ctrl + Shift + E**, which allows you to view the execution plan of your queries. This feature helps identify performance bottlenecks and optimize queries effectively.

## 1. Understanding Sequential Scans (Seq Scan)
When a query execution plan shows **Seq Scan**, it means that the database is performing a sequential scan on the table instead of using an index. This usually happens when:
- No suitable index exists.
- The query optimizer determines that a full table scan is more efficient than using an index.
- The statistics are outdated, causing the optimizer to make incorrect decisions.

## 2. Identifying High-Cost Queries
In the execution plan, the **cost** values indicate the estimated resource usage of different query operations. Queries with high cost should be prioritized for optimization. You can identify these by:
- Looking for operations with significantly high cost values.
- Checking if a sequential scan is being used unnecessarily.
- Comparing estimated rows with actual retrieved rows to see if statistics need updating.

## 3. Steps to Optimize Queries
If you find that your query is using a **Seq Scan** and has a high cost, you can apply the following optimizations:

1. **Create Indexes**
   - If your query filters results using a WHERE clause, adding an index can improve performance.
   ```sql
   CREATE INDEX idx_column_name ON table_name(column_name);
   ```
2. **Force Index Usage (if necessary)**
   - In some cases, the optimizer might ignore an index. You can force its usage explicitly:
   ```sql
   SET enable_seqscan TO OFF;
   ```
3. **Update Table Statistics**
   - If the optimizer is making incorrect choices, updating table statistics might help:
   ```sql
   ANALYZE table_name;
   ```
4. **Use Partitioning for Large Tables**
   - If your table contains massive amounts of data, consider partitioning to improve performance.

## 4. Conclusion
DBeaver’s execution plan tool, accessed via **Ctrl + Shift + E**, is essential for identifying slow queries and optimizing their performance. By checking for **Seq Scan** operations and high-cost queries, you can take steps such as adding indexes, updating statistics, and restructuring queries to improve database efficiency. Always analyze the execution plan before making changes to ensure effective query optimization.

---

Dbeaver에서 컨트롤 shift E

Seq Scan을 타면 인덱스 스캔을 안타는 것임
Cost가 높은 곳을 찾아서 튜닝 하기.
