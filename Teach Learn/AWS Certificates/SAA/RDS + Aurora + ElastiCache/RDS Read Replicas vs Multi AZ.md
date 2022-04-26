### Read Replicas
- Up to 5 Read Replicas
- Within AZ, Cross AZ or Cross Region
- Replication is ASYNC, so reads are eventaully consistent
- Replicas can be promoted to their. own DB
- Applications must update the connection string to leverage read replicas
- ![[Screen Shot 2022-04-23 at 16.00.28.png]]
- Use Cases:
	- You have a production database that is taking on normal load
	- You want to run a reporting application to run some analytics
	- You create a Read Replica to run the new workload there
	- The production application is unaffected
	- Read replicas are used for SELECT(=read) only kind of statements (not INSERT, UPDATE, DELETE)
	- ![[Screen Shot 2022-04-23 at 16.29.35.png]]
- Network cost:
	- In AWS there's a network cost when data goes from one AZ to another
	- **For RDS Read Replicas within the same region, you don't pay that fee**![[Screen Shot 2022-04-23 at 16.32.46.png]]
### Multi AZ (Diaster Recovery)
- SYNC replication
- One DNS name - automatic app failover to standby
- Increase availability
- Failover in case of loss of AZ, loss of network, instnace or storage failure
- No manual intervation in aps
- Not used for scaling
- Note: The Read Replicas be setup as Multi AZ for Disaster Recovery (DR)
![[Screen Shot 2022-04-23 at 16.55.41.png]]
### From Single-AZ to Multi-AZ
- Zero down time operation (no need to stop the DB)
- Just click on "modify" for the database
- ![[Screen Shot 2022-04-23 at 16.59.17.png]]
- The following happen internally:
	- A snapshot is taken
	- A new DB is restored from the snapshot in a new AZ
	- Synchronization is established between the two databases