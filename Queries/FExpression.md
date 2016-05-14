# F Expression

## Increment IntField Atomic

```python
from django.db.models import F

TipReceiverInfo.objects.filter(receiver_id=receiver_id).update(receiver_total_fee=F('receiver_total_fee') + amount)
```

## References

[1] Ty.@StackOverflow, [Django: Increment blog entry view count by one. Is this efficient?](http://stackoverflow.com/questions/447117/django-increment-blog-entry-view-count-by-one-is-this-efficient)