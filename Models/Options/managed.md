# managed

## managed=False

* makemigrations

  * https://github.com/django/django/blob/master/django/core/management/commands/makemigrations.py
  * Create Migrations with ``'managed': False`` in ``migrations.CreateModel``

    ```python
    class Migration(migrations.Migration):
        initial = True
        dependencies = [
        ]
        operations = [
            migrations.CreateModel(
                name='',
                fields=[
                ],
                options={
                    'managed': False,
                },
            ),
        ]
    ```

* migrate

  * https://github.com/django/django/blob/master/django/core/management/commands/migrate.py
  * Insert into table ``django_migrations``
  * Skip ``managed=False``

## References

[1] Docs@DjangoProject, [Model Meta options - Options.managed](https://docs.djangoproject.com/en/dev/ref/models/options/#django.db.models.Options.managed)

[2] thekorn@StackOverflow, [django: exclude models from migrations](http://stackoverflow.com/questions/33385618/django-exclude-models-from-migrations)

[3] Ticket@DjangoProject, [Migrations do not ignore unmanaged models (unlike syncdb)](https://code.djangoproject.com/ticket/22331)