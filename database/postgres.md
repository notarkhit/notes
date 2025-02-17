# Postgres installation in  archlinux

> Install the [postgresql](https://archlinux.org/packages/?name=postgresql) package.  

```bash
sudo pacman -S postgresql
```
This will create a system user called *postgres*  

Initialize the database cluster before postgresql can work properly.  

```postgres
[postgres]$ initdb -D /var/lib/postgres/data
```

where -D is the default location where the database cluster must be stored.

```postgres
[postgres]$ initdb --locale=C.UTF-8 --encoding=UTF8 -D /var/lib/postgres/data --data-checksums
```

### create the first database user

```postgres
[postgres]$ createuser --interactive 
```

### create database 
```postgres
[postgres]$ createdb myDatabaseName
```

### access the database shell 

```postgres
[postgres]$ psql -d myDatabaseName
```
