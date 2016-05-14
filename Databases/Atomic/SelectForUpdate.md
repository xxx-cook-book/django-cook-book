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
      customer = CustomerAccountInfo.objects.select_for_update().filter(account_id=account_id)[0]
  except IndexError:
      return response(CustomerStatusCode.CUSTOMER_NOT_FOUND)
  ```

* get()

  ```
  try:
      customer = CustomerAccountInfo.objects.select_for_update().get(account_id=account_id)[0]
  except CustomerAccountInfo.DoesNotExist:
      return response(CustomerStatusCode.CUSTOMER_NOT_FOUND)
  ```

* get_or_create()

  ```python
  customer, created = CustomerAccountInfo.objects.select_for_update().get_or_create(app_id=app.app_id, phone=phone, user_id=user_id)
  ```

## References

[1] Blog@[NOAH KANTROWITZ](https://coderanger.net/), [SELECT FOR UPDATE in Django](https://coderanger.net/select-for-update/)

[2] Indradhanush Gupta@StackOverflow, [How to use select_for_update to 'get' a Query in Django?](http://stackoverflow.com/questions/17159471/how-to-use-select-for-update-to-get-a-query-in-django)

[3] Django@Github, [django/tests/select_for_update/tests.py](https://github.com/django/django/blob/master/tests/select_for_update/tests.py)

