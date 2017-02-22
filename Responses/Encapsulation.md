# Encapsulation

## StatusCode

```shell
pip install StatusCode
```

* [Python Status Code based on ``kkconst``](https://github.com/errorless/StatusCode)
* [Define const field and const class, customize it as You Like (Python)](https://github.com/kaka19ace/kkconst)

## utils/error/error_utils.py

```python
# -*- coding: utf-8 -*-

from StatusCode import BaseStatusCode, StatusCodeField


class UserStatusCode(BaseStatusCode):
    """ User Relative StatusCode 4001xx """
    USER_NOT_FOUND = StatusCodeField(400101, 'User Not Found', description=u'用户不存在')
```

## utils/error/response_utils.py

```python
# -*- coding: utf-8 -*-

from django.http import JsonResponse
from StatusCode import StatusCodeField


def response_data(status_code=200, message=None, description=None, data={}, **kwargs):
    return dict({
        'status': status_code,
        'message': message,
        'description': description,
        'data': data,
    }, **kwargs)


def response(status_code=200, message=None, description=None, data={}, **kwargs):
    message, description = (message or status_code.message, description or status_code.description) if isinstance(status_code, StatusCodeField) else (message, description)
    return JsonResponse(response_data(status_code, message, description, data, **kwargs), safe=False)
```

## Usage

```python
from utils.error.errno_utils import UserStatusCode
from utils.error.response_utils import response

def xxx(request):
	return response(UserStatusCode.USERNAME_HAS_REGISTERED)
```

## References