- All caches in ElastiCache:
	- Do not support IAM authentication
	- IAM policies on ElastiCache are only used for AWS API-level security
- Redis AUTH
	- You can set a "password/token" when you create a Redis cluster
	- This is an extra level security for your cache (on top of security groups)
	- Support SSL in flight encryption
- Memcached
	- Supports SASL-based authentication (advanced)
	- ![[Screen Shot 2022-04-27 at 11.23.34.png]]

### Patterns for ElastiCache
- **Lazy loading**: all the read data is cached, data can become stale in cache
- **Write Through**: Adds or update data in the cache when written to a DB (no stale data)
- **Session Store**: store temporary session data in a cache (using TTL features)

- Quote: There are only two hard things in Computer Science: cache invalidation and naming things
- ![[Screen Shot 2022-04-27 at 11.27.33.png]]

### Redis Use Case
- Gaming Leaderboards are computationally complex
- **Redis Sorted sets** gurantee both uniqueness and element ordering
- Each time a new element added, it's ranked in real time, then added in correct order
- ![[Screen Shot 2022-04-27 at 11.30.52.png]]