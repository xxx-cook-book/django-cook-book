# Multiple Databases

## Settings.py

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'emoji_utf8',
        'USER': 'root',
        'PASSWORD': '',
    },
    'utf8mb4': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'emoji_utf8mb4',
        'USER': 'root',
        'PASSWORD': '',
        'OPTIONS': {
            'charset': 'utf8mb4',
        },
    }
}
```

## Syncdb

```shell
python manage.py syncdb # default
python manage.py syncdb --database='utf8mb4' # set to syncdb utf8mb4
```

## South

```shell
python manage.py migrate --database=utf8mb4
```

## Using

* Models
  ```python
  models.objects.using('utf8mb4').filter()
  ```
* Connections
  ```python
  from django.db import connections
  connections['utf8mb4']
  cursor = connection.cursor()
  cursor.execute('raw_sql')
  rows = cursor.fetchall()
  ```
  * https://github.com/django/django/blob/master/django/db/__init__.py#L21

