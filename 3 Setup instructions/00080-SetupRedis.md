### Setup Redis for Oskari (Optional)

Redis is used for caching data for example from statistical datasources to provide a cleaner user experience for statistical map functionalities (optional part of `oskari-map` webapp). It is also used in clustered environment (when you have multiple instances running an Oskari-based server) for session management (servers see sessions initiated by the other nodes) and cluster messaging like flushing caches from all nodes when something is updated on one node.

1\) Get Redis

Install binaries from http://redis.io/ or from your platforms package repository.

2\) Install

And startup `redis-server`. The default port Redis listens to is `6379`.

3\) Configure Oskari (Optional)

Oskari expects Redis to be found in the default port (6379) on the same server as Oskari ("localhost"). If you have it running on another host/port you need to change the `oskari-ext.properties`:

```properties
redis.hostname = localhost
redis.port = 6379

# # Credentials
# redis.user = (optional)
# redis.password = (optional)

# # Timeout configuration is milliseconds
# redis.timeout.connect = 2000
# redis.ssl = false
# redis.pool.size = 30

# # BlockWhenExhausted setting
# redis.blockExhausted = false
```
The commented ones are optional and depend on your environment. The values are the defaults (user/pass defaults are not defined).
