# Select For Update

## Transaction

```python
In [1]: from django.db import transaction

In [2]: from account.models import Profile

In [6]: with transaction.atomic():
   ...:     Profile.objects.select_for_update().get(uid='dUKzzQvx7nuGHQszA8xdkC')
```

## Select_for_update

* filter()

  ```python
  try:
      profile = Profile.objects.select_for_update().filter(uid=uid)[0]
  except IndexError:
      return response(ProfileStatusCode.Profile_NOT_FOUND)
  ```

* get()

  ```
  try:
      profile = Profile.objects.select_for_update().get(uid=uid)
  except Profile.DoesNotExist:
      return response(ProfileStatusCode.Profile_NOT_FOUND)
  ```

* get_or_create()

  ```python
  profile, created = Profile.objects.select_for_update().get_or_create(uid=uid)
  ```

## References

[1] Blog@[NOAH KANTROWITZ](https://coderanger.net/), [SELECT FOR UPDATE in Django](https://coderanger.net/select-for-update/)

[2] Indradhanush Gupta@StackOverflow, [How to use select_for_update to 'get' a Query in Django?](http://stackoverflow.com/questions/17159471/how-to-use-select-for-update-to-get-a-query-in-django)

[3] Django@Github, [django/tests/select_for_update/tests.py](https://github.com/django/django/blob/master/tests/select_for_update/tests.py)

