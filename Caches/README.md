# Caches

## Memcached

* Installation

  ```shell
  # Mac OS X
  brew install memcached libmemcached  # Server Package, Development Package

  # Ubuntu
  sudo apt-get install memcached libmemcached-dev  # Server Package, Development Package
  ```

## Redis

* Installation

  ```shell
  # Mac OS X
  brew install redis

  # Ubuntu
  # Latest Version
  sudo apt-get update
  sudo apt-get install -y python-software-properties
  sudo add-apt-repository -y ppa:rwky/redis
  sudo apt-get update
  sudo apt-get install -y redis-server
  ```