- Can move between classes manually or using S3 lifecycle configurations
![[Screen Shot 2022-05-22 at 15.11.07.png]]
![[Screen Shot 2022-05-22 at 15.10.49.png]]
### S3 Durability and Availability
- Durability
	- High durability (99.9999999999%, 11 9s) of objects across multiple AZ
	- If your store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
	- Same for all storage classes
- Availability:
	- Measures how readily available a service is
	- Varies depending on storage class
	- Example: S3 standard has 99.99% availability = not available 53 minutes a year

### S3 Standard - General  Purpose
- 99.99% Availability
- Used for frequency accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use Cases: Big Data analytics, mobile & gaming applications, content distribution ...

### S3 Storage Classes - Infrequent Access 
- For data that is less frequently accessed, but requires rapid access when needed
- Lower cost than S3 Standard
#### Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
	- 99.9% Availability
	- Use cases: Disaster Recovery, backups
#### Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)
	- High durability in a single AZ; data lost when AZ is destroyed
	- 99.5% Availability
	- Use Cases: Storing secondary backup copies of on-premise data, or data you can recreate

### S3 Galacier Storage Classess
- Low-cost object storage meant for archiving/ backup
- Pricing: price for storage + object retrieval cost
#### Amazon Glacier Galcier Instant Retrieval
- Milisecond retrieval, great for data accessed once a quarter
- Minimum storage duration of 90 days
#### Amazon Glacier Galcier Flexible Retrieval (formerly Amazon S3 Galcier)
- Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) - free
- Minium storage duration of 90 days
#### Amazon S3 Glacier Deep Archive - for long term storage:
- Standard (12 hours), Bulk (48 hours)
- Minium storage duration of 180 days

### S3 Intelligent - Tiering
- Small monthly monitoring and auto-tiering fee
- Moves objects automatically between Access Tiers based on usage
- There are no retrievak charges in S3 Intelligent-Tiering

- Frequent Access tier (automatic): default tier
- Infrequent Access tier (automatic): object not accessed for 30 days
- Archive Instant Access tier (automatic): object not accessed for 90 days
- Archive Access tier (automatic): configurable from 90 days to 700+ days
- Deep Archive Access tier (optional): configurable from 180 days to 700+ days