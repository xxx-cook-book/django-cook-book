# Migrations

## Django 1.7 及以后

##### Commands

* Makemigrations

  ```shell
  python manage.py makemigrations
  ```

* Migrate

  ```shell
  python manage.py migrate
  ```

##### Problems

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

    * Install
      ```shell
      pip install django-shortuuidfield
      ```

    * Import

      ```python
      from shortuuidfield import ShortUUIDField
      ```

    * ​Usage
      ```python
      uid = ShortUUIDField(_(u'uid'), max_length=255, help_text=u'User UUID', db_index=True)
      ```

##  Django 1.7 以前

##### 3rd Library

* Install

  ```shell
  pip install south
  ```

##### Commands

* Initial

  ```shell
  python manage.py schemamigration app_name --initial
  ```

* Add Field

  ```shell
  python manage.py schemamigration app_name --auto
  ```

* Exists Projects Convert to Use South

  ```shell
  python manage.py syncdb  # Syncdb has already changed by South, to create table ``south_migrationhistory`` in Database
  python manage.py convert_to_south app_name  # create 0001_initial.py, insert record in table ``south_migrationhistory``
  ```

* Forwards／Backwards

  * Database and South_migrationhistory Both Change

    ```shell
    python manage.py migrate app_name 0001
    ```

  * Database Not and South_migrationhistory Change

    ```shell
    python manage.py migrate app_name 0001 --fake
    ```


* Multiple Branches

  * Suppose app_name developed in two branches(master and branch2) from 0004.

  * At the time of merge, mater is 0007 and branch2 is 0008, and they don't add the same field.

  * Solve multiple branches merge:

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