# redis-py

## Installation

```shell
$ sudo pip install redis
```

## Usage

redis-py exposes two client classes that implement these commands.

* The StrictRedis class attempts to adhere to the official command syntax. 


* The Redis class, a subclass of StrictRedis, overrides several other commands to provide backwards compatibility with older versions of redis-py.

```python
>>> import redis
>>> r = redis.StrictRedis(host='localhost', port=6379, db=0)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
'bar'
```

## Connection Pools

Behind the scenes, redis-py uses a connection pool to manage connections to a Redis server. By default, each Redis instance you create will in turn create its own connection pool. You can override this behavior and use an existing connection pool by passing an already created connection pool instance to the connection_pool argument of the Redis class. You may choose to do this in order to implement client side sharding or have finer grain control of how connections are managed.

```python
>>> pool = redis.ConnectionPool(host='localhost', port=6379, db=0)
>>> r = redis.StrictRedis(connection_pool=pool)
```

## Parsers

Parser classes provide a way to control how responses from the Redis server are parsed. redis-py ships with two parser classes, the PythonParser and the HiredisParser. By default, redis-py will attempt to use the HiredisParser if you have the hiredis module installed and will fallback to the PythonParser otherwise.

Hiredis is a C library maintained by the core Redis team. Pieter Noordhuis was kind enough to create Python bindings. Using Hiredis can provide up to a 10x speed improvement in parsing responses from the Redis server. The performance increase is most noticeable when retrieving many pieces of data, such as from LRANGE or SMEMBERS operations.

#### hiredis

* Installation

  ```python
  $ sudo pip install hiredis
  ```

## Pipelines

Pipelines are a subclass of the base Redis class that provide support for buffering multiple commands to the server in a single request. They can be used to dramatically increase the performance of groups of commands by reducing the number of back-and-forth TCP packets between the client and server.

```python
>>> r = redis.Redis(...)
>>> r.set('bing', 'baz')
>>> # Use the pipeline() method to create a pipeline instance
>>> pipe = r.pipeline()
>>> # The following SET commands are buffered
>>> pipe.set('foo', 'bar')
>>> pipe.get('bing')
>>> # the EXECUTE call sends all buffered commands to the server, returning
>>> # a list of responses, one for each command.
>>> pipe.execute()
[True, 'baz']

>>> pipe.set('foo', 'bar').sadd('faz', 'baz').incr('auto_number').execute()
[True, True, 6]
```

In addition, pipelines can also ensure the buffered commands are executed atomically as a group. This happens by default. If you want to disable the atomic nature of a pipeline but still want to buffer commands, you can turn off transactions.

```python
>>> pipe = r.pipeline(transaction=False)
```

## References

[1] andymccurdy@Github, [redis-py — The Python interface to the Redis key-value store.](https://github.com/andymccurdy/redis-py)

[2] redis@Github, [hiredis — Minimalistic C client for Redis >= 1.2](https://github.com/redis/hiredis)

[3] redis@Github, [hiredis-py — Python wrapper for hiredis](https://github.com/redis/hiredis-py)