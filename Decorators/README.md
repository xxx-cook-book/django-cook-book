# Decorators

## Not Support Parameters

```python
from functools import wraps

def xxx(func):
    @wraps(func)
    def returned_wrapper(request, *args, **kwargs):
        # do something
        return func(request, *args, **kwargs)
    return returned_wrapper
```

* Usage

  ```python
  @xxx
  def foo(request):
      # do something
  ```
  *Tips: equals to ``xxx(foo)(request)``*

## Just Support Parameters

```python
def xxx(param1=None):
    def decorator(func):
        @wraps(func)
        def returned_wrapper(request, *args, **kwargs):
            # do something
            return func(request, *args, **kwargs)
        return returned_wrapper
    return decorator
```

* Usage

  ```python
  @xxx(param1='/')
  def foo(request):
      # do something

  # () is necessary
  # Or will raise error
  # takes exactly 1 argument (0 given)
  @xxx()
  def bar(request):
      # do something
  ```
  *Tips: equals to ``xxx(param1)(foo)(request)``* 

## Support Parameters Or Not

```python
def xxx(func=None, param1=None):
    def decorator(func):
        @wraps(func)
        def returned_wrapper(request, *args, **kwargs):
            # do something
            return func(request, *args, **kwargs)
        return returned_wrapper

    if not func:
        def decorator2(func):
            return decorator(func)
        return decorator2

    return decorator(func)
```

* Usage

  ```python
  @xxx(param1='/')
  def foo(request):
      # do something

  @xxx
  def bar(request):
      # do something
  ```
  *Tips: equals to ``xxx(param1)(foo)(request)`` and ``xxx(bar)(request)``* 

## References
