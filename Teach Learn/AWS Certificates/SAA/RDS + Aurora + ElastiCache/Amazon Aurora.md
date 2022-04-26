- Aurora is proprietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurorar DB (that means your driver will work as if Aurora was a Postgres or MySQL database)
- Aurora is "AWS cloud optimized" and claims 5x performance improvement over MySQL on RDS, over 3x performance of Postgres on RDS
- Aurora storage automatically grows in increments of 10GB, up to 128TB
- Aurorra can have 15 replicas while MySQL has 5, and the replication process is faster (ssub 10 ms replica lag)
- Failover in Aurora is instantaneous. It's HA native
- Aurora costs more than RDS (20% more) - but is more efficient
### High Availability and Read Scaling
- 6 copies of your data accross 3 AZ:
	- 4 copies out of 6 needed for writes
	- 3 copies out of 6 need for reads
	- Self healing with peer-to-peer replication
	- Storage is striped across 100s of volumes
- One Aurora Instance takes writes (master)
- Automated failover for master in less than 30 seconds
- Master + up to 15 Aurora Read Replicas serve reads
- Support for Cross Region Replication
- ![[Screen Shot 2022-04-26 at 10.21.52.png]]

- ![[Screen Shot 2022-04-26 at 10.24.45.png]]
### Features
- Automatic fail-over
- Backup and Recovery
- Isolation and security
- Industry compliance
- Push-button scaling
- Automated Patching with Zero Downtime
- Advanced Monitoring
- Routinee Maintenance
- Backtrack: restore data at any point of time without using backups
### Security
- Similar to RDS because uses the same engines
- Encryption at rest using KMS
- Automated backups, snapshots and replicas are also encrypted
- Encryption in flight using SSL (same process as MySQL or Postgres)
- Possiblity to authenticate using IAM token (same method as RDS)
- You are responsible for protecting the instance with security groups
- You can't SSH