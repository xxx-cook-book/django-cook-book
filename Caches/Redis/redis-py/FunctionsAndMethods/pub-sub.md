# Pub/Sub

## Pub

```python
publish(channel, message)
```

Publish `message` on `channel`. Returns the number of subscribers the message was delivered to.

```python
In [31]: r.publish('test', 'this will reach the listener')
Out[31]: 1L
```

## Sub

```python
pubsub(shard_hint=None)
```

Return a Publish/Subscribe object. With this object, you can subscribe to channels and listen for messages that get published to them.

```python
In [3]: pubsub = r.pubsub()

In [4]: pubsub.subscribe('test')

In [5]: pubsub.get_message()
Out[5]: {'channel': 'test', 'data': 1L, 'pattern': None, 'type': 'subscribe'}

In [6]: pubsub.get_message()

In [7]: pubsub.get_message()
Out[7]:
{'channel': 'test',
 'data': 'this will reach the listener',
 'pattern': None,
 'type': 'message'}

In [8]: pubsub.subscribed
Out[8]: True

In [9]: pubsub.
pubsub.PUBLISH_MESSAGE_TYPES      pubsub.ignore_subscribe_messages
pubsub.UNSUBSCRIBE_MESSAGE_TYPES  pubsub.listen
pubsub.channels                   pubsub.on_connect
pubsub.close                      pubsub.parse_response
pubsub.connection                 pubsub.patterns
pubsub.connection_pool            pubsub.psubscribe
pubsub.decode_responses           pubsub.punsubscribe
pubsub.encode                     pubsub.reset
pubsub.encoding                   pubsub.run_in_thread
pubsub.encoding_errors            pubsub.shard_hint
pubsub.execute_command            pubsub.subscribe
pubsub.get_message                pubsub.subscribed
pubsub.handle_message             pubsub.unsubscribe​
```

## References