# Corrupted mysql


## Restoring
1. create new blank database
1. copy dtp API instance and create a temporary instance
1. modify new instance to use new database
1. rebuild schema (run framework's database migration scrip)
1. import lost table data to new database (take from old backups where data exist)
1. create msqldump of up-to-date data
1. import up-to-date data
1. test
1. switch production database to new one
1. remove temporary instance