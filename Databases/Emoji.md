# Migrations

## After MySQL 5.5.3

_New In MySQL 5.5.3_

#### Charset

```mysql
CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```

* DATABASE
* TABLE
* COLUMN

## Settings.py

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
*  ``charset`` setting as ``utf8mb4``  in ``DATABASES`` in ``Settings.py``

##  Before MySQL 5.5.3

## unicode_escape

```python
user.last_name = u'Slatkevičius'.encode('unicode_escape')
user.save()

print user.last_name
>>> Slatkevi\u010dius
print user.last_name.decode('unicode_escape')
>>> Slatkevičius
```

## pyemoji

* Emoji Convert & Replace

# References

[1] Brightcells@Github, [pyemoji —— Emoji Convert &Replace](https://github.com/Brightcells/pyemoji)