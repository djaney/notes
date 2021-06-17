# [HOWTO] Change collation of all tables in mysql

```sql
SELECT CONCAT("ALTER TABLE ", TABLE_SCHEMA, '.', TABLE_NAME," CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;") AS    ExecuteTheString
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA="dtpcmsdb"
AND TABLE_TYPE="BASE TABLE";
```