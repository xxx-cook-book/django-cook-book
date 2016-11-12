# Encapsulation

## utils/error/error_utils.py

```python
# -*- coding: utf-8 -*-

from kkconst import BaseConst, ConstIntField


class BaseStatusCode(BaseConst):
    class Meta:
        allow_duplicated_value = False  # Status_code should be no duplicated value


class StatusCodeField(ConstIntField):
    def __init__(self, status_code, message='', description=''):
        ConstIntField.__init__(status_code, verbose_name=message, description=description)
        self.message = message


class UserStatusCode(BaseStatusCode):
    """ 用户相关错误码  4001xx """
    USER_NOT_FOUND = StatusCodeField(400101, u'User Not Found', description=u'用户不存在')
```

## utils/error/response_utils.py

```python
# -*- coding: utf-8 -*-

from django.http import JsonResponse

from utils.error.errno_utils import StatusCodeField


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