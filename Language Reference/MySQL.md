## Common Queries

| Query                                                               | Description                                                            |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `SHOW DATABASES`                                                    | Displays all of the databases that exist on the server                 |
| `USE database_name`                                                 | Sets databaseName as the current database                              |
| `DESCRIBE table_name`                                               | Lists the details of each column in the table                          |
| `SHOW TABLES`                                                       | Lists all tables in the current database                               |
| `SELECT NOW()`                                                      | Gets the current data and time                                         |
| `CREATE DATABASE database_name`                                     | Creates a new database called `database_name`                          |
| `DROP DATABASE database_name`                                       | Deletes the database called `database_name`                            |
| `CREATE TABLE table_name(col1, col2)`                               | Creates a new database table with the columns `col1` and `colu2`       |
| `AUTO_INCREMENT`                                                    | Generates a unique identifier for each row                             |
| `DROP TABLE table_name`                                             | Deletes the table named `table_name`                                   |
| `RENAME TABLE old_table_name TO new_table_name`                     | Renames the table                                                      |
| `ALTER TABLE table_name ADD(column3, column4`                       | Adds two new columns to the database                                   |
| `ALTER TABLE table_name DROP(column3, column4`                      | Deletes `column3` and `column4` from the table specified               |
| `INSERT INTO table_name(col1,col2) VALUES('val',1)`                 | Inserts a new record into the database                                 |
| `UPDATE table_name SET col1 = val1, column2 = val2 WHERE condition` | Updates records in the table that met the condition                    |
| `DELETE FROM table_name WHERE condition`                            | Deletes records from the table that met the condition statement        |
| `SELECT col1, col2, col3 FROM table_name WHERE condition`           | Retrieves the records in the table that meet the condition             |
| `SELECT * FROM table_name`                                          | Get the entire table                                                   |
| `COUNT`                                                             |                                                                        |

## Users
### Create a User

```sql
# single user
CREATE USER 'username'@'hostname' IDENTIFIED BY 'password';
# multipe users
CREATE USER 
'user1'@'hostname' IDENTIFIED BY 'password',
'user2'@'hostname' IDENTIFIED BY 'password';
```

### Viewing User Permissions

```sql
SHOW GRANTS FOR 'user'@'localhost';
```

### Delete a User

```sql
DROP USER 'username'@'host';
```

### Changing a User Password

https://www.geeksforgeeks.org/mysql-change-user-password/

### User Permissions

https://docs.digitalocean.com/products/databases/mysql/how-to/modify-user-privileges/

### Other
> [!note]
> The USER() command is used to return the current user account information

## Databases

Databases are created through the MySQL command line client.

```sql
CREATE DATABASE IF NOT EXISTS myDatabase
```

Run the following command to begin making query in that database

```sql
USE DATABASE_NAME;
```