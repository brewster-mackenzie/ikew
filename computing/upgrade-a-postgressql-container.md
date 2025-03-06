# Upgrade a PostgresSQL container

#postgres #docker #sql 

-----

## Export the current data

Stop the stack and any containers that depend on the database

```bash
docker compose down
```

Start the database container only

```bash
docker container start pg_sql
```

Backup the database to a SQL file

```bash
docker exec -it pg_sql pg_dumpall -U postgres > $HOME/backup.sql
```

Write a script to extract the desired database

```bash
#!/bin/bash
[ $# -lt 2 ] && { echo "Usage: $0 <postgresql dump> <dbname>"; exit 1; }
sed "/connect.*$2/,\$!d" $1 | sed "/PostgreSQL database dump complete/,\$d"
```

Run the script 

```bash
chmod +x ./pg_extract.sh

./pg_extract.sh ./backup.sql mydb >> ./backup-mydb.sql
```

## Update PostgreSQL

Either run a different container, or edit the compose file accordingly.

Start the container

Connect to the database and check the new version is correct

```sql
select version()
```

## Import the data

Import the data into the new database

```bash 
cat $HOME/backup.sql | docker exec -i pg_sql psql -U postgres 
```

Finally, restart any contains which depend on this database.

## References

[Thomas Bandt](https://thomasbandt.com/postgres-docker-major-version-upgrade)
