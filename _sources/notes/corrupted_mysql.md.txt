# [FIX] Corrupted Mysql InnoDB
In this case, the InnoDB engine got errors. There is an unaccessilble table.
You are also unable to manually fix the issue because you have no access to RDS itself.

The solution is to build a backup file based on backup(s) and re-build the database from scratch.


## Restoring
1. create new blank database
1. copy dtp API instance and create a temporary instance
1. modify new instance to use new database
1. rebuild schema (run framework's database migration script)
1. backup old database
1. import lost table data to new database (take from old backups where data exist)
1. create msqldump of up-to-date data
1. import up-to-date data to new database
1. test (script below to check difference)
1. switch production database to new one
1. remove temporary instance


## Exporting

`--insert-ignore` will not insert if already existing

`--set-gtid-purged=OFF` there are things in dump that causes an error. This fixed it. 
I don't understand why yet.

`--extended-insert=FALSE` do not merge rows as a single insert command.
It causes some missing tables.

    # dump schema
    mysqldump id --insert-ignore --set-gtid-purged=OFF -h{hostname} -u{username} -p{pssword} {database} > file.sql
    # dump data
    mysqldump --extended-insert=FALSE --no-create-info=TRUE --set-gtid-purged=OFF -h{hostname} -u{username} -p{pssword} {database} > file.sql

## Importing

    mysql -h{hostname} -u{username} -p{password} {database} < {database}.sql

## checking difference

    mysqldbcompare --server1=user:pass@host:port --server2=user:pass@host:port db1:db2