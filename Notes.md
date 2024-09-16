# Class Notes

rod@exgnosis.ca

## Migrating DB2 to PostgreSQL

[Migrating from DB2 to PostgreSQL – What You Should Know](https://severalnines.com/blog/migrating-db2-postgresql-what-you-should-know/)

[Converting from other Databases to PostgreSQL](https://wiki.postgresql.org/wiki/Converting_from_other_Databases_to_PostgreSQL)

[IBM DB2 to PostgreSQL Migration](http://www.sqlines.com/db2-to-postgresql)

Additional guide has been added to the extras directory

## Migrating SQL server to PostgreSQL

[Migrate a SQL Server Database to a PostgreSQL Database](https://www.mssqltips.com/sqlservertip/8039/migrate-a-sql-server-database-to-a-postgresql-database/)

[SQL Server to Postgres – A Step-by-Step Migration Journey](https://bryteflow.com/sql-server-vs-postgres-how-to-migrate-sql-to-postgresql/)

[Migrating from SQL Server to PostgreSQL: A Comprehensive Guide](https://pradeepl.com/blog/migrating-from-sql-server-to-postgresql/)


    

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

A Foreign Data Wrapper (FDW) allows access and manipulation of data stored in external databases as if they were part of the database. 

To connect to an oracle database

- Define the connection details for the Oracle server. 

```sql
CREATE EXTENSION oracle_fdw;

CREATE SERVER oracle_server
FOREIGN DATA WRAPPER oracle_fdw
OPTIONS (dbserver '//oracle_host:1521/orclpdb1');
```

- Map a PostgreSQL user to an Oracle user with the appropriate credentials:

```sql
CREATE USER MAPPING FOR postgres
SERVER oracle_server
OPTIONS (user 'oracle_user', password oracle_password');
```

- Define a foreign table that corresponds to the Oracle table

```sql
CREATE FOREIGN TABLE oracle_employees (
    emp_id INTEGER,
    emp_name TEXT,
    emp_salary NUMERIC
)
SERVER oracle_server
OPTIONS (schema 'ORACLE_SCHEMA', table 'EMPLOYEES');
```

- query the Oracle table as if it were a native table:

```sql
SELECT * FROM oracle_employees WHERE emp_salary > 50000;
```

---

## Monitoring tools

## Links to Monitoring tools

[pgAdmin Official Website](https://www.pgadmin.org/)

[pgBadger GitHub Repository](https://github.com/darold/pgbadger)

[pg_top GitHub Repository](https://github.com/markwkm/pg_top)

[PostgreSQL Exporter GitHub Repository](https://github.com/prometheus-community/postgres_exporter)

[pgmetrics GitHub Repository](https://github.com/rapidloop/pgmetrics)

[Percona Monitoring and Management Website](https://www.percona.com/software/database-tools/percona-monitoring-and-management)

[pg_stat_kcache GitHub Repository](https://github.com/powa-team/pg_stat_kcache)

[TimescaleDB Official Website](https://www.timescale.com/)

[pgCluu GitHub Repository](https://github.com/darold/pgcluu)

---

## Oracle to PostgreSQL migration links

[Oracle to PostgreSQL Migration: Challenges and Solutions](https://dataengineeracademy.com/blog/oracle-to-postgresql-migration-challenges-and-solutions/)

[The Complete Oracle to Postgres Migration Guide: Tools, Schema, and Data](https://www.enterprisedb.com/blog/the-complete-oracle-to-postgresql-migration-guide-tutorial-move-convert-database-oracle-alternative)

[Migrating from Oracle to PostgreSQL: A Comprehensive Guide](https://dev.to/pawnsapprentice/migrating-from-oracle-to-postgresql-a-comprehensive-guide-5e8i)

[My Oracle to PostgreSQL Migration: The 7 Tools That Made It Possible](https://www.eversql.com/my-oracle-to-postgresql-migration-the-7-tools-that-made-it-possible/)

---
## Back up and restore

[pgBackRest User Guide](https://pgbackrest.org/user-guide.html)

---

## Using active directory

[Postgres and AD](https://www.strongdm.com/blog/connecting-postgres-to-active-directory-for-authentication)

[Integrating PostgreSQL with Active Directory for LDAP Authentication](https://medium.com/@kemalozz/integrating-postgresql-with-active-directory-for-ldap-authentication-360526dfdb25)

---

## Replication

[Patroni](https://patroni.readthedocs.io/en/latest/)

[PostgreSQL Replication: A Comprehensive Guide](https://kinsta.com/blog/postgresql-replication/)

[Setting Up PostgreSQL Replication: A Step-by-Step Guide](https://medium.com/@umairhassan27/setting-up-postgresql-replication-on-slave-server-a-step-by-step-guide-1ff36bb9a47f)


