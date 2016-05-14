# Group By

## itertools.groupby

```python
users = TipReceiverInfo.objects.filter(packet_id=packet_id, status=True).order_by('receiver_room').values()
users = map(lambda x: {'receiver_room': x[0], 'receiver_user': [y for y in x[1]]}, itertools.groupby(users, lambda x: x['receiver_room']))
```

## Raw SQL

## References