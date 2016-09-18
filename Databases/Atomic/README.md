# Atomic

## TransactionManagementError

* Codes

  ```python
  @transaction.atomic
  def foo():
      while:
          # Queries
  ```

* Error

  ```shell
  django.db.transaction.TransactionManagementError: An error occurred in the current transaction. You can't execute queries until the end of the 'atomic' block.
  ```

* Reasons

  * ``while`` makes ``atomic`` not end

* Solutions

  ```python
  def foo():
      while:
          with transaction.atomic():
              # Queries
  ```

## IntegrityError

* Codes

  ```python
  with transaction.atomic():
      try:
          profile = Profile.objects.select_for_update().get(uid=uid)
      except Profile.DoesNotExist:
          pass

  profile.foo = 'bar'
  profile.save()
  ```

* Error

  ```shell
  django.db.utils.IntegrityError: (1062, "Duplicate entry 'xxx' for key 'ooo'")
  ```

* Reasons

  * TOFOUND

* Solutions

## OperationalError

* Codes

  ```python
  with transaction.atomic():
      try:
          profile = Profile.objects.select_for_update().get(uid=uid)
      except Profile.DoesNotExist:
          pass
  ```

* Error

  ```shell
  django.db.utils.OperationalError: (1213, 'Deadlock found when trying to get lock; try restarting transaction')
  ```

* Reasons

  * innodb_lock_wait_timeout

    * Terminal1

      ```python
      with transaction.atomic():
          profile = Profile.objects.select_for_update().get(uid=uid)
          time.sleep(10)
      ```

    * Terminal2

      ```python
      with transaction.atomic():
          profile = Profile.objects.select_for_update().get(uid=uid)  # Wait
      ```

  * TOFOUND

* Solutions

  ```python
  from django.db.utils import OperationalError

  with transaction.atomic():
      try:
          profile = Profile.objects.select_for_update().get(uid=uid)
      except Profile.DoesNotExist:
          pass
      except OperationalError:
          # TODO
  ```

## Override

* Codes

  * Terminal1

    ```python
    with transaction.atomic():
        profile = Profile.objects.get(uid=uid)
        profile.diamond = 10
        time.sleep(10)
        profile.save()
    ```

  * Terminal2

    ```python
    with transaction.atomic():
        profile = Profile.objects.select_for_update().get(uid=uid)  # Not Wait
        profile.diamond = 100
        profile.save()
    ```
  * Diamond
    ```python
    Profile.objects.get(uid=uid).diamond  # 10, not 100
    ```

* Solutions
  * Only get ``profile`` by ``Profile.objects.select_for_update().get(uid=uid)`` if ``profile.save()``