# Sorted Set

```python
In [20]: r.z
r.zadd              r.zincrby           r.zrange            r.zrank             r.zremrangebyrank   r.zrevrangebyscore  r.zscan_iter
r.zcard             r.zinterstore       r.zrangebylex       r.zrem              r.zremrangebyscore  r.zrevrank          r.zscore
r.zcount            r.zlexcount         r.zrangebyscore     r.zremrangebylex    r.zrevrange         r.zscan             r.zunionstore
```

## Insert

* Add Element Into Sorted Set

  ```python
  zadd(name, *args, **kwargs)
  ```
  Set any number of score, element-name pairs to the key `name`. Pairs can be specified in two ways:

  As [*](https://redis-py.readthedocs.io/en/latest/#id5)args, in the form of: score1, name1, score2, name2, ... or as [**](https://redis-py.readthedocs.io/en/latest/#id7)kwargs, in the form of: name1=score1, name2=score2, ...

  The following example would add four values to the ‘my-key’ key: redis.zadd(‘my-key’, 1.1, ‘name1’, 2.2, ‘name2’, name3=3.3, name4=4.4)

  ```python
  In [51]: r.zadd('sorted_set', 1, 'a')
  Out[51]: 1

  In [52]: r.zadd('sorted_set', 2, 'b', 3, 'c')
  Out[52]: 2

  In [53]: r.zadd('sorted_set', **{'d': 4})
  Out[53]: 1

  In [54]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[54]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0)]
  ```

  Tips: Some Redis Version, Can Just Add One Element One Time

## Select

* Select Elements By Index
  * Sorted In Ascending Order

    ```python
    zrange(name, start, end, desc=False, withscores=False, score_cast_func=<type 'float'>)
    ```

    Return a range of values from sorted set `name` between `start` and `end` sorted in ascending order.

    `start` and `end` can be negative, indicating the end of the range.

    `desc` a boolean indicating whether to sort the results descendingly.

    `withscores` indicates to return the scores along with the values. The return type is a list of (value, score) pairs.

    `score_cast_func` a callable used to cast the score return value.

    ```python
    In [65]: r.zrange('sorted_set', 0, 100, withscores=True)
    Out[65]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0)]

    In [66]: r.zrange('sorted_set', 0, 100, desc=True, withscores=True)
    Out[66]: [('d', 4.0), ('c', 3.0), ('b', 2.0), ('a', 1.0)]
    ```

  * Sorted In Descending Order

    ```python
    zrevrange(name, start, end, withscores=False, score_cast_func=<type 'float'>)
    ```

    Return a range of values from sorted set `name` between `start` and `end` sorted in descending order.

    `start` and `end` can be negative, indicating the end of the range.

    `withscores` indicates to return the scores along with the values. The return type is a list of (value, score) pairs.

    `score_cast_func` a callable used to cast the score return value.

    ```pythn
    In [7]: r.zrevrange('sorted_set', 0, 100, withscores=True)
    Out[7]: [('d', 4.0), ('c', 3.0), ('b', 2.0), ('a', 1.0)]
    ```
    Tips: ``zrange desc`` equals ``zrevrange``

* Select Elements By Score
  * Sorted In Ascending Order

    ```python
    zrangebyscore(name, min, max, start=None, num=None, withscores=False, score_cast_func=<type 'float'>)
    ```

    Return a range of values from the sorted set `name` with scores between `min` and `max`.

    If `start` and `num` are specified, then return a slice of the range.

    `withscores` indicates to return the scores along with the values. The return type is a list of (value, score) pairs.

    score_cast_func` a callable used to cast the score return value.

    ```python
    In [13]: r.zrangebyscore('sorted_set', 0, 100, start=0, num=1, withscores=True)
    Out[13]: [('a', 1.0)]
    ```

  * Sorted In Descending Order

    ```python
    zrevrangebyscore(name, max, min, start=None, num=None, withscores=False, score_cast_func=<type 'float'>)
    ```

    Return a range of values from the sorted set `name` with scores between `min` and `max` in descending order.

    If `start` and `num` are specified, then return a slice of the range.

    `withscores` indicates to return the scores along with the values. The return type is a list of (value, score) pairs.

    `score_cast_func` a callable used to cast the score return value.

    ```python
    In [19]: r.zrevrangebyscore('sorted_set', 100, 0, start=0, num=1, withscores=True)
    Out[19]: [('d', 4.0)]
    ```


* Select Elements By Alphabet

  * Sorted In Ascending Order

    ```python
    zrangebylex(name, min, max, start=None, num=None)
    ```
    Return the lexicographical range of values from sorted set ``name`` between ``min`` and ``max``.
    If ``start`` and ``num`` are specified, then return a slice of the range.

    ```python
    In [72]: r.zrangebylex('sorted_set', '[a', '(b')
    Out[72]: ['a']

    In [73]: r.zrangebylex('sorted_set', '[a', '[b')
    Out[73]: ['a', 'b']
    ```

  * Sorted In Descending Order

    ```python
    zrevrangebylex(name, max, min, start=None, num=None)
    ```

    Return the reversed lexicographical range of values from sorted set ``name`` between ``max`` and ``min``.
    If ``start`` and ``num`` are specified, then return a slice of the range.

* Count Elements

  * All ``(-∞ < score < ∞)``

    ```python
    zcard(name)
    ```

    Return the number of elements in the sorted set `name`

    ```python
    In [48]: r.zcard('sorted_set')
    Out[48]: 4
    ```

  * Score ``(min <= score <= max)``

    ```python
    zcount(name, min, max)
    ```

    Returns the number of elements in the sorted set at key ``name`` with a score between ``min`` and ``max``.

    ```python
    In [26]: r.zcount('sorted_set', 0, 100)
    Out[26]: 4L
    ```

  * Alphabet ``(alphabet between min and max)``

    ```python
    zlexcount(name, min, max)
    ```

    Return the number of items in the sorted set ``name`` between the lexicographical range ``min`` and ``max``.


* Get Element's Score

  ```python
  zscore(name, value)
  ```

  Return the score of element `value` in sorted set `name`

  ```python
  In [29]: r.zscore('sorted_set', 'd')
  Out[29]: 4.0
  ```

* Get Element's Rank Index

  * Sorted In Ascending Order

    ```python
    zrank(name, value)
    ```

    Returns a 0-based value indicating the rank of `value` in sorted set `name`

    ```python
    In [40]: r.zrank('sorted_set', 'a')
    Out[40]: 0

    In [41]: r.zrank('sorted_set', 'd')
    Out[41]: 3
    ```

  * Sorted In Descending Order

    ```pyrhon
    zrevrank(name, value)
    ```

    Returns a 0-based value indicating the descending rank of `value` in sorted set `name`

    ```python
    In [43]: r.zrevrank('sorted_set', 'a')
    Out[43]: 3

    In [44]: r.zrevrank('sorted_set', 'd')
    Out[44]: 0
    ```

## Update

* Increase Element's Score

  ```python
  zincrby(name, value, amount=1)
  ```

  Increment the score of `value` in sorted set `name` by `amount`

  ```python
  In [50]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[50]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0)]

  In [51]: r.zincrby('sorted_set', 'e', 5)
  Out[51]: 5.0

  In [52]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[52]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0), ('e', 5.0)]

  In [53]: r.zincrby('sorted_set', 'e', 5)
  Out[53]: 10.0

  In [54]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[54]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0), ('e', 10.0)]
  ```

## Delete

* Delete Elements By Element

  ```python
  zrem(name, *values)
  ```

  Remove member `values` from sorted set `name`

  ```python
  In [54]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[54]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0), ('e', 10.0)]

  In [55]: r.zrem('sorted_set', 'e')
  Out[55]: 1
      
  In [56]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[56]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0)]
  ```

* Delete Elements By Index

  ```python
  zremrangebyrank(name, min, max)
  ```

  Remove all elements in the sorted set `name` with ranks between `min` and `max`. Values are 0-based, ordered from smallest score to largest. Values can be negative indicating the highest scores. Returns the number of elements removed

  ```python
  In [60]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[60]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0)]

  In [61]: r.zremrangebyrank('sorted_set', 2, -1)
  Out[61]: 2

  In [62]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[62]: [('a', 1.0), ('b', 2.0)]
  ```

* Delete Elements By Score

  ```python
  zremrangebyscore(name, min, max)
  ```

  Remove all elements in the sorted set `name` with scores between `min` and `max`. Returns the number of elements removed.

  ```python
  In [62]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[62]: [('a', 1.0), ('b', 2.0)]

  In [63]: r.zremrangebyscore('sorted_set', 0, 100)
  Out[63]: 2

  In [64]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[64]: []
  ```

* Delete Elements By Alphabet

  ```python
  zremrangebylex(name, min, max)
  ```

  Remove all elements in the sorted set ``name`` between the lexicographical range specified by ``min`` and ``max``.
  Returns the number of elements removed.

  ```python
  In [77]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[77]: [('a', 1.0), ('b', 2.0), ('c', 3.0), ('d', 4.0)]

  In [78]: r.zremrangebylex('sorted_set', '[a', '[b')
  Out[78]: 2

  In [79]: r.zrange('sorted_set', 0, 100, withscores=True)
  Out[79]: [('c', 3.0), ('d', 4.0)]
  ```

## Other

* Intersect

  ```python
  zinterstore(dest, keys, aggregate=None)
  ```

  Intersect multiple sorted sets specified by `keys` into a new sorted set, `dest`. Scores in the destination will be aggregated based on the `aggregate`, or SUM if none is provided.

* Union

  ```python
  zunionstore(dest, keys, aggregate=None)
  ```

  Union multiple sorted sets specified by `keys` into a new sorted set, `dest`. Scores in the destination will be aggregated based on the `aggregate`, or SUM if none is provided.

## Usage

* Select Top N

  ```python
  In [55]: r.zrange('sorted_set', 0, 100, desc=True, withscores=True)
  Out[55]: [('d', 4.0), ('c', 3.0), ('b', 2.0), ('a', 1.0)]

  In [56]: r.zrevrange('sorted_set', 0, 100, withscores=True)
  Out[56]: [('d', 4.0), ('c', 3.0), ('b', 2.0), ('a', 1.0)]
  ```

## References

[1] redis-py@Github, [redis-py](https://github.com/andymccurdy/redis-py/blob/master/redis/client.py)

[2] redis-py@ReadTheDocs, [redis-py’s documentation](https://redis-py.readthedocs.io/en/latest/)

