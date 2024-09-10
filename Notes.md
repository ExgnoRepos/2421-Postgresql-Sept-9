# Class Notes

## Incrementally refreshable materialized views

PostgreSQL 16 introduced support for incrementally refreshable materialized views,
- Uses the command
```sql 
REFRESH MATERIALIZED VIEW ... WITH DATA
```

- Requires the pg_snapshot extension to be installed
- Provides the ability to track changes in tables efficiently. 
- Change tracking involves capturing the changes (inserts, updates, deletes) made to the data in the base tables that are used by a materialized view. 
- Can use logical replication slots or triggers

```sql
CREATE MATERIALIZED VIEW order_totals 
WITH (incremental_refresh = true) AS
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id
WITH NO DATA;

REFRESH MATERIALIZED VIEW order_totals WITH DATA;

```

## Partitioned Index

Supported in PQ

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    amount DECIMAL
) PARTITION BY RANGE (order_date);


CREATE TABLE orders_2023 PARTITION OF orders
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE orders_2024 PARTITION OF orders
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');


CREATE INDEX ON orders USING btree (order_date);

```

## Foreign data wrappers

