---
tag: SoftSkill
---
Let’s solve it as a programmer.

After staying in the same company for years as senior or lead, we always find ourselves overwhelmed by the increasing requests. — From everyone.

Because: you are the one, who is familiar with everything, does things fast, and knows everything.

# So, you must have been here

![](https://miro.medium.com/v2/resize:fit:1400/1*kBTeCSflZip6bH1EDGkpgg.png)

Look at this. Is it you? Can you spot the problem? Yes, you should at least see:

- “You” always take things on yourself the first time (eager loading, wrong! You should be lazy loading)
- “You” are doing everything, almost! (superclass)
- Everyone is looking for “you”! (minimal knowledge is principle is broken!)
- There is “dev1” under you, waiting to be mentored and help you. however, he is idle (multi-threaded model is never used!)
- You are flooded with new stories and more and more bugs (crash!)
- Bottleneck started (blocking I/O)
- “You” may not even have time to onboard anyone in the end
- “You” will burn out.

Does this problem look familiar in the programming world? yes.

- Superclass
- Blocking I/O
- Synchronise
- Single-Threaded

Let’s fix it as a developer.

# Async

![](https://miro.medium.com/v2/resize:fit:1400/1*FH1KaAPMHI2QdmzN238o6g.png)

Delegate. always. once you delegated, a new thread started. you are ready to listen to new “requests”, even if you are single-threaded (but you do have a multi-threading choice, shown below), but you still can be fast, like Redis! as long as your I/O is not blocked.

So when you get a task from the project manager. you should try to avoid doing it yourself, and try to assign it to a junior dev, as fresh as possible. use greedy thinking.

but what if you don’t have anyone to delegate? mentor, keep doing it. it’s your job.

# Pooling

We all know this. when starting a new database connection, ORM picks up a new thread from the pool. what if we apply this idea to management?

Your team is your “thread pool”. Make sure you are taking care of them. when I say taking care I mean:

- they are having fun
- Doing something challenging
- They like what they are doing
- Getting what they need

Patient.

Answer questions and mentor them. empower them will improve the development throughput as a team.

Balance, always.

So you like “dev1” because he is good and want him to do more? be careful! do not overload anyone and ignored others' growth.

Make sure there is someone ready to be “scheduled”. the larger pool you can build, the higher throughput your team can take. which means more productivity.

# Task.join()

on top of async and pooling. sometimes when you got a “epic task” then break down and “started” multiple tasks, you need to “aggregate” the results. so after distributing tasks make sure you sent “let’s have a sync up at 4 pm” to “join” the threads.

# Fast path and slow path

![](https://miro.medium.com/v2/resize:fit:1400/1*pU6mXG6XJcF0hED-QpqgHw.png)

In the above picture, I want to address 2 issues.

- Use a Queue to store the “not urgent” tasks and batch them later
- For urgent issues, reply immediately, and delegate a developer “link” to the requester

If you have ever worked on any big data project. you should be familiar with data ingestion flow. so the idea is applicable here. think about the “requirement” as “data”.

Is this requirement urgent? only if yes go the fast path, which could mean getting an answer from doc and replying to an email, or “do something now” (pick up someone from your mentor pool and “start()”)

If not. go slow path, which means ticket it and assigned it to yourself or someone else.

Try always to go the slow path if possible.

Again, greedy algorithm. (very very important, see details below)

# Batch

As you saw above. batch process the “fragments” stuff.

This idea is not new. meaning fix a time range and then do something every day. e.g. check and reply to emails and messages at 10 am, 12 am, and 4 pm; or review all pull requests at 1 pm and 5 pm.

For everything is a “fragment”, Batch it. does not limit to personal tasks, could be meeting as well. instead of ad-hoc or asking for status every hour, having a stand-up meeting every day is an idea of batch “to collect ideas or status”.

# Greedy

![](https://miro.medium.com/v2/resize:fit:1400/1*fA1AH4gxCo4GWxWAfD8cKA.png)

First, protect yourself! try not to solve anything by yourself. Doing it fast and making PM happy is never your goal.

Remember that, you are the single point of failure to your team. which means you are the only master node, and there is no zookeeper to help you!

Second. when you assigned a developer to some task, try to put it as junior as possible.

Third, estimate as long as possible. when estimating a task, when someone asks you “How long does your team need to finish this?”. you need to first ask the person who is going to do the work, then multiply the number by 3–5 times.

To PM reader: do not get me wrong, we are not sitting there doing anything.

We have bugs to fix, technical debts, got people to train, documents to write, and tests to add! our team needs more time, for quality!

# Keep Prioritizing

Priority keeps changing. we all know that. something suddenly becomes the top 1 priority. and the load comes to you. do not panic. put down the task and pick up the new one.

Just to prepare your mind for this to happen.

Be ready, this can happen anytime.

# Callback VS Poll

![](https://miro.medium.com/v2/resize:fit:704/1*saarZP33lQftG_A5bvHokQ.png)

For high performers, once assigned a task, then trust them and wait for a callback; for low performers, you may need to poll status.

Be careful when doing this.

Do not be mad at this “dev2”. be patient.

The goal here is the “turn” everyone into “trustable”, no need for polling at all. how to do that?

First, It could mean a wrong task assigned to him/her. change to another ticket and see how it goes; Secondly try to assign some easier tasks, if still not working then try to let him/her pick a ticket next time and see how it goes.

# To all devs: Save()/Initialize()

In the morning, you want to call something like “initialize” to quickly warm up.

The trick is very simple. “save” your day before leaving. write down the top 3–5 things that you need to do tomorrow morning in your notepad. try to write something as details as possible. instead of writing “Add some tests to this logout module”, you can write “Add JWT token expiry test to logout module, document link can be found at xx”.

The idea is, to dump whatever details are in your mind right now, and make sure you can start immediately after loading all these “logs”.

The second day morning open the laptop, read “logs”, and get into a flow state.

Within seconds.