# Emoji

## Since MySQL 5.5.3

_New In MySQL 5.5.3_

#### Charset

```sql
CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```

* DATABASE

  ```sql
  ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
  ```

* TABLE

  ```sql
  ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
  ```

* COLUMN

  ```sql
  ALTER TABLE table_name MODIFY COLUMN col VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

  ALTER TABLE table_name MODIFY COLUMN col VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL;

  ALTER TABLE table_name MODIFY COLUMN col VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL;
  ```
  * Attention ``NOT NULL`` or ``DEFAULT NULL``
  * ERROR 1265 (01000): Data truncated for column 'nickname' at row 2


#### Settings.py

```python
DATABASES = {
    'default': {
        'ENGINE':'django.db.backends.mysql',
        ...
        'OPTIONS': {'charset': 'utf8mb4'},
    }
}
```

* ``charset`` in ``DatabaseWrapper`` in ``django/db/backends/mysql/base.py`` default ``utf8``
* ``charset`` setting as ``utf8mb4``  in ``DATABASES`` in ``Settings.py``

##  Before MySQL 5.5.3

#### Unicode_escape

```python
user.last_name = u'Slatkevičius'.encode('unicode_escape')
user.save()

print user.last_name
>>> Slatkevi\u010dius
print user.last_name.decode('unicode_escape')
>>> Slatkevičius
```

#### Pyemoji

* Emoji Convert & Replace

* Installation

  ```shell
  pip install pyemoji
  ```

* Usage

  ```python
  In [1]: import pyemoji

  In [2]: pyemoji.encode(u'笑脸表情：😄')
  Out[2]: '\\u7b11\\u8138\\u8868\\u60c5\\uff1a\\U0001f604'

  In [3]: print pyemoji.encode(u'笑脸表情：😄')
  \u7b11\u8138\u8868\u60c5\uff1a\U0001f604

  In [4]: pyemoji.decode('\\u7b11\\u8138\\u8868\\u60c5\\uff1a\\U0001f604')
  Out[4]: u'\u7b11\u8138\u8868\u60c5\uff1a\U0001f604'

  In [5]: print pyemoji.decode('\\u7b11\\u8138\\u8868\\u60c5\\uff1a\\U0001f604')
  笑脸表情：😄
  ```

# References

[1] Brightcells@Github, [pyemoji — Emoji Convert &Replace](https://github.com/Brightcells/pyemoji)