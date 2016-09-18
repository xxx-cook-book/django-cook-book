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