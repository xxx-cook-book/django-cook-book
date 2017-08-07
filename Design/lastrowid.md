# lastrowid

## Djagno
* [lastrowid](https://github.com/django/django/search?utf8=%E2%9C%93&q=lastrowid&type=)
* [last_insert_id](https://github.com/django/django/search?utf8=%E2%9C%93&q=last_insert_id&type=)

## MySQL
Syntax:
```sql
id = cursor.lastrowid
```
Doc:
```doc
This read-only property returns the value generated for an AUTO_INCREMENT column 
by the previous INSERT or UPDATE statement or None when there is no such value available. 
For example, if you perform an INSERT into a table that contains an AUTO_INCREMENT column, 
lastrowid returns the AUTO_INCREMENT value for the new row.
```
* [10.5.13 MySQLCursor.lastrowid Property](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-lastrowid.html)

## References
[1] Blairg23@StackOverflow, [Python mysql.connector cursor.lastrowid always returns 0](https://stackoverflow.com/questions/27260363/python-mysql-connector-cursor-lastrowid-always-returns-0)
