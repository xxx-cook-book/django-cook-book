# Q Expression

## Example

```python
from django.db.models import Q

# Profile.objects.filter(unionid='qwertyuiop', Q(phone='') | Q(phone__isnull=True))
# SyntaxError: non-keyword arg after keyword arg
Profile.objects.filter(Q(unionid='qwertyuiop'), Q(phone='') | Q(phone__isnull=True))
# Tips: No Q Expression after Q is OK
Profile.objects.filter(Q(phone='') | Q(phone__isnull=True), unionid='qwertyuiop')
```

## References
