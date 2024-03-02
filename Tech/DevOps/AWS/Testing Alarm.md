---
tag: AWS, Devops
---
### Set State to alarm using CLI
```bash
aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
```