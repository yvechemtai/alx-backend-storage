# README.md for alx-backend-storage/0x02-Redis_Basic

## Redis Basic

This project focuses on utilizing Redis for basic operations and explores various functionalities offered by Redis. The tasks involve creating a Cache class, writing and reading strings to Redis, incrementing values, storing lists, and implementing an expiring web cache and tracker.

## Project Details

### 0x02. Redis Basic

- **Back-end:** Redis
- **Author:** Emmanuel Turlay (Staff Software Engineer at Cruise)
- **Weight:** 1
- **Project Duration:** Dec 20, 2023, 6:00 AM, to Dec 21, 2023, 6:00 AM
- **Checker Release:** Dec 20, 2023, 12:00 PM
- **Auto Review:** An auto review will be launched at the deadline.

### Resources

- [Redis Commands](#)
- [Redis Python Client](#)
- [How to Use Redis With Python](#)
- [Redis Crash Course Tutorial](#)

### Learning Objectives

Upon completing this project, you are expected to:

1. Learn how to use Redis for basic operations.
2. Understand Redis as a simple cache.
3. Familiarize yourself with Redis commands and the Redis Python client.
4. Implement methods to write and read strings to/from Redis.
5. Use Redis to store and retrieve lists.
6. Explore advanced concepts like decorators in Python.
7. Implement an expiring web cache and tracker using Redis.

## Requirements

### Redis Installation

To run the project, you need to install Redis on Ubuntu 18.04. Use the following commands:

```bash
$ sudo apt-get -y install redis-server
$ pip3 install redis
$ sed -i "s/bind .*/bind 127.0.0.1/g" /etc/redis/redis.conf
```

### Tasks

#### 0. Writing strings to Redis

Create a Cache class with methods to store data in Redis.

```bash
$ cat main.py
#!/usr/bin/env python3
"""
Main file
"""
import redis

Cache = __import__('exercise').Cache

cache = Cache()

data = b"hello"
key = cache.store(data)
print(key)

local_redis = redis.Redis()
print(local_redis.get(key))
```

#### 1. Reading from Redis and recovering the original type

Implement a `get` method to retrieve data from Redis with optional conversion functions.

```python
cache = Cache()

TEST_CASES = {
    b"foo": None,
    123: int,
    "bar": lambda d: d.decode("utf-8")
}

for value, fn in TEST_CASES.items():
    key = cache.store(value)
    assert cache.get(key, fn=fn) == value
```

#### 2. Incrementing values

Implement a `count_calls` decorator to count how many times methods of the Cache class are called.

```bash
$ cat main.py
#!/usr/bin/env python3
""" Main file """

Cache = __import__('exercise').Cache

cache = Cache()

cache.store(b"first")
print(cache.get(cache.store.__qualname__))

cache.store(b"second")
cache.store(b"third")
print(cache.get(cache.store.__qualname__))
```

#### 3. Storing lists

Implement a `call_history` decorator to store the history of inputs and outputs for a particular function.

```bash
$ cat main.py
#!/usr/bin/env python3
""" Main file """

Cache = __import__('exercise').Cache

cache = Cache()

s1 = cache.store("first")
print(s1)
s2 = cache.store("secont")
print(s2)
s3 = cache.store("third")
print(s3)

inputs = cache._redis.lrange("{}:inputs".format(cache.store.__qualname__), 0, -1)
outputs = cache._redis.lrange("{}:outputs".format(cache.store.__qualname__), 0, -1)

print("inputs: {}".format(inputs))
print("outputs: {}".format(outputs))
```

#### 4. Retrieving lists

Implement a `replay` function to display the history of calls of a particular function.

```python
cache = Cache()
cache.store("foo")
cache.store("bar")
cache.store(42)
replay(cache.store)
```

#### 5. Implementing an expiring web cache and tracker (Advanced)

Implement a `get_page` function that tracks how many times a particular URL was accessed and caches the result with an expiration time of 10 seconds.

```bash
$ cat web.py
#!/usr/bin/env python3
""" Web Cache file """

from exercise import Cache

def get_page(url: str) -> str:
    """
    Get HTML content of a URL and cache the result with expiration time.
    """
    # Your implementation here

# Additional Bonus: Implement this use case with decorators.
```

## Example

```bash
$ cat main.py
#!/usr/bin/env python3
"""
Main file
"""

Cache = __import__('exercise').Cache

cache = Cache()

data = b"hello"
key = cache.store(data)
print(key)

local_redis = redis.Redis()
print(local_redis.get(key))
```

## How to Use Redis Aggregations

- Redis offers various commands and functionalities for aggregations. While the tasks in this project focus on fundamental Redis usage, you might want to explore advanced Redis features for more complex scenarios. To incorporate aggregations into this project, consider adding optional tasks or sections exploring aggregations in the context of Redis data.

## Advanced Usage

- This project covers basic interactions with Redis using Python. For more advanced use cases, consider exploring features such as Redis pub/sub, transactions, and Lua scripting. These features can enhance the functionality and performance of your Redis-based applications.

## Additional Resources

- [Redis Documentation](https://redis.io/documentation)
- [PyRedis Documentation](https://redis-py.readthedocs.io/en/stable/)
- [Requests Documentation](https://docs.python-requests.org/en/latest/)
- [Redis Commands Reference](https://redis.io/commands)
- [Python Decorators](https://realpython.com/primer-on-python-decorators/)
- [Understanding Redis as a Cache](https://www.digitalocean.com/community/tutorial_series/understanding-redis-as-a-cache)
- [Redis Labs - Redis University](https://university.redislabs.com/)
