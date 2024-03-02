---
tags: Architect, Database
---

CAP stands for Consistency, Availability and Partition Tolerance.

- Consistency (C ): All nodes see the same data at the same time. What you write you is what you get to read.
- Availability (A): A guarantee that every request receives a response about whether it was successful or failed. Whether you want to read or write you will get some response back.
- Partition tolerance (P): The system continues to operate despite arbitrary message loss or failure of part of the system. Irrespective of communication cut down among the nodes, system still works.

> ==Often CAP theorem is misunderstood. It is not any 2 out of 3. Key point here is== ==**P**== ==is not visible to your customer. It is Technology solution to enable C and A. Customer can only experience C and A.==

P is driven by wires, electricity, software and hardware and none of us has any control and often P may not be met. If P is existing, there is no challenge with A and C (except for latency issues). The problem comes when P is not met. Now we have two choices to make.

**AP:** When there is no partition tolerance, the system is available but with inconsistent data.

![](https://miro.medium.com/v2/resize:fit:1400/1*28U3YrPC_11kq2h4PsX-Bg.png)

**CP:** When there is no partition tolerance, system is not fully available. But the data is consistent.

![](https://miro.medium.com/v2/resize:fit:1400/1*6eCxb09MQAQCv7m-n_ns-Q.png)

It would be a crime if I don’t end the explanation of CAP theorem without the famous CAP triangle and some popular databases.

![](https://miro.medium.com/v2/resize:fit:1400/1*TGSdFh0mVfW_7QTsO-yTpQ.png)

More reference: https://www.youtube.com/watch?v=RexrINtVh-M