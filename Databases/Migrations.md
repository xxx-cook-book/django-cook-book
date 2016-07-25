# Migrations

## Since Django 1.7

_New In Django 1.7_

#### Commands

* Makemigrations

  ```shell
  python manage.py makemigrations
  ```

* Migrate

  ```shell
  python manage.py migrate
  ```
* Forwards／Backwards

  * Database and ``Django_migrations`` Both Change

    ```shell
    python manage.py migrate app_name 0001
    ```

  * Database Not and ``Django_migrations`` Change

    ```shell
    python manage.py migrate app_name 0001 --fake
    ```

#### Problems

* ValueError: Cannot serialize:
  * Models

    ```python
    uid = models.CharField(_(u'uid'), max_length=255, default=shortuuid.uuid, help_text=u'User UUID', db_index=True)
    ```

  * Description
    ```
    ValueError: Cannot serialize: <bound method ShortUUID.uuid of <shortuuid.main.ShortUUID object at 0x1025eab90>>
    There are some values Django cannot serialize into migration files.
    For more, see https://docs.djangoproject.com/en/1.8/topics/migrations/#migration-serializing
    ```

  * Solution
    * Use ShortUUIDField

    * Installation
      ```shell
      pip install django-shortuuidfield
      ```

    * ​Usage
      ```python
      from shortuuidfield import ShortUUIDField

      uid = ShortUUIDField(_(u'uid'), max_length=255, help_text=u'User UUID', db_index=True)
      ```

#### South

* Conflict

  ```
  There is no South database module 'south.db.mysql' for your database. Please either choose a supported database, check for SOUTH_DATABASE_ADAPTER[S] settings, or remove South from INSTALLED_APPS.
  ```

##  Before Django 1.7

#### South

* Install

  ```shell
  pip install south
  ```

#### Commands

* Initial

  ```shell
  python manage.py schemamigration app_name --initial
  ```

* Add-On Field

  ```shell
  python manage.py schemamigration app_name --auto
  ```

* Exists Projects Convert to Use South

  ```shell
  python manage.py syncdb  # Syncdb has already changed by South, to create table ``south_migrationhistory`` in Database
  python manage.py convert_to_south app_name  # create 0001_initial.py, insert record in table ``south_migrationhistory``
  ```

* Forwards／Backwards

  * Database and ``South_migrationhistory`` Both Change

    ```shell
    python manage.py migrate app_name 0001
    ```

  * Database Not and ``South_migrationhistory`` Change

    ```shell
    python manage.py migrate app_name 0001 --fake
    ```


#### Problems

* Multiple Branches

  * Suppose app_name developed in two branches(master and branch2) from 0004

  * At the time of merge, mater is 0007 and branch2 is 0008, and they don't add the same field

  * Solve multiple branches merge

    * Exec command in master branch

      ```shell
      git merge branch2
      ```

    * Solve conflict

    * south_migrationhistory back to 004

      ```shell
      python manage.py migrate app_name 0004 --fake
      ```

    * delete migrations after 0004

    * create new migration

      ```shell
      python manage.py schemamigration app_name --auto
      ```

    * south_migrationhistory back to 0005

      ```shell
      python manage.py migrate app_name 0005 --fake
      ```

## References

[1] ferprez@StackOverflow, [There is no South database module 'south.db.postgresql_psycopg2' for your database](http://stackoverflow.com/questions/29478400/there-is-no-south-database-module-south-db-postgresql-psycopg2-for-your-databa)