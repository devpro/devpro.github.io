# PostgreSQL

[Home](../readme.md) > [PostgreSQL](./readme.md)

This is a set of resources and documentation about [PostgreSQL](https://www.postgresql.org/).

| Action | Request |
| ------ | ------- |
| Update a user password | `` |

## Queries

### SQL

#### Update a user password

```sql
ALTER USER user WITH PASSWORD 'newpassword';
```

#### Get all databases

```sql
SELECT * FROM pg_catalog.pg_database;
```

#### Get all tables

```sql
SELECT
    table_schema || '.' || table_name
FROM
    information_schema.tables
WHERE
    table_type = 'BASE TABLE'
AND
    table_schema NOT IN ('pg_catalog', 'information_schema');

SELECT * FROM pg_catalog.pg_tables WHERE schemaname = 'gracethd';
```

#### Get all columns of a table

```sql
SELECT *
FROM information_schema.columns
WHERE table_schema = 'gracethd'
  AND table_name   = 'adresse';
```
