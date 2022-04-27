- The same way RDS is to get managed Relational Databases ...
- ElastiCache is to get managed Redis or Memcached
- Caches are in-memory databases with really high performance, low latency
- Helps make your application stateless
- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups
- **Using ElastiCache involves heavy application code changes**
### DB Cache
- Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache