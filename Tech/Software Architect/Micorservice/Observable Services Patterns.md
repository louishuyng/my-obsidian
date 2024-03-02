---
tags:
  - Microservice
  - Architect
---
![[Pasted image 20231220194050.png]]

- **_Health check API_**—Expose an endpoint that returns the health of the service.
- **_Log aggregation_**—Log service activity and write logs into a centralized logging server, which provides searching and alerting.
- **_Distributed tracing_**—Assign each external request a unique ID and trace requests as they flow between services.
- **_Exception tracking_**—Report exceptions to an exception tracking service, which de-duplicates exceptions, alerts developers, and tracks the resolution of each exception.
- **_Application metrics_**—Services maintain metrics, such as counters and gauges, and expose them to a metrics server.
- **_Audit logging_**—Log user actions.

## Log Aggregate 
![[Pasted image 20231220235458.png]]
## Tracing Implementation
![[Pasted image 20231220235425.png]]

##  Metrics Pattern
![[Pasted image 20231221000421.png]]