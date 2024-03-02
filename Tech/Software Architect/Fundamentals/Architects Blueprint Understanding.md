---
tags: Architect, Backend
---

[Source](https://medium.com/bytebytego-system-design-alliance/the-architects-blueprint-understanding-software-styles-and-patterns-with-cheatsheet-5c1f5fd55bbd)

## Architectural Styles vs. Architectural Patterns

Before we delve into the specifics, it’s important to distinguish between architectural styles and patterns, as these terms are often used interchangeably but have distinct meanings.

**Architectural Styles** are high-level strategies that provide an abstract framework for a family of systems. An architectural style improves partitioning and promotes design reuse by frequently solving recurring problems. You can think of it as the theme or aesthetic that guides the design of buildings or homes. Examples include Layered, Event-Driven, and Microservices.

On the other hand, **Architectural Patterns** are more concrete and are specific to a particular problem or module within the system. They provide a structured solution to architectural issues, detailing how components and interactions should be structured for specific functionality. They are similar to software design patterns but work at a higher level of abstraction. Examples include Model-View-Controller (MVC), Publish-Subscribe, and Serverless.

While architectural styles provide a broad framework and can be seen as a general philosophy of a system’s design, architectural patterns are concrete solutions to specific design problems within this framework. In other words, architectural styles describe the overarching structure of the system, while architectural patterns address specific design problems that might arise within this structure.

In the following sections, we will explore ten key architectural styles, each with its respective patterns, principles, strengths, weaknesses, and applications. These styles include Layered, Component-Based, Service-Oriented, Distributed System, Domain-Driven, Event-Driven, Separation of Concern, Interpreter, Concurrency, and Data-Centric. By understanding these styles and patterns, you can better navigate the architectural landscape and design systems that are robust, scalable, and maintainable. Let’s dive in!

> “In the grand tapestry of software design, styles are the broad strokes of the brush, while patterns are the intricate details that bring the masterpiece to life.”

# The Ultimate Cheatsheet

To help you navigate the vast landscape of architectural styles and patterns, I’ve created a cheat sheet that encapsulates all the key points discussed in this blog. This cheat sheet is a handy reference guide that you can use to quickly recall the main characteristics of each architectural style and pattern.

![](https://miro.medium.com/v2/resize:fit:2000/1*46JX-o6xxMaMmOmTLwyLYQ.png)

Software Architecture Styles Cheatsheet (Download High-Resolution: [https://bit.ly/architect-cheatsheet](https://bit.ly/architect-cheatsheet))

# 1. Layered Architecture Style

Layered architecture is one of the most common architectural patterns. It’s often used for traditional web applications and enterprise applications.

- **Principles**: This architectural style separates concerns into distinct layers. A typical example is the 3-tier architecture: presentation, business logic, and data storage layers.
- **Strengths**: Easy to understand, test, and maintain; each layer can be developed and updated independently.
- **Weaknesses**: This can lead to performance overhead; changes that affect multiple layers can be challenging to implement.
- **Applications**: Web applications, enterprise applications.
- **Antipatterns**: Circular dependencies, skipping layers.

## Layered (n-tier) pattern

An n-tier architecture divides the system into n-layers, each with a specific responsibility. The most common division is into three layers: presentation, business logic, and data storage.

![](https://miro.medium.com/v2/resize:fit:532/1*xyUEhFCRW5dzhqxmMPCgtw.png)

Layered (n-tier) Architecture Pattern

## Clean / Onion Pattern

Clean Architecture, also known as Onion Architecture, is a software design philosophy emphasizing the separation of concerns within the system. It organizes the software into concentric layers, with the domain model at the core, surrounded by application-specific layers. The outer layers depend on the inner layers but not vice versa, promoting high decoupling and isolation. This allows changes in infrastructure, UI, or external agencies to have minimal impact on the business logic. It’s ideal for systems requiring high maintainability, testability, and independence from UI, database, or external frameworks.

![](https://miro.medium.com/v2/resize:fit:1400/1*CSjLVbFvdT6UWfpikdJVMg.png)

Clean / Onion Architecture Pattern

# 2. Component-Based Architecture Style

This style emphasizes the separation of concerns regarding the wide-ranging functionality available throughout a software system.

- **Principles**: This architecture style organizes a system as loosely-coupled, reusable components.
- **Strengths**: High level of reusability, flexibility, and maintainability.
- **Weaknesses**: The complexity of managing components and their interactions.
- **Applications**: Web applications, desktop applications, distributed systems.
- **Antipatterns**: Overly large components, redundant components.

## Object-oriented Pattern

This pattern is a paradigm based on “objects,” which can contain data and code: data in the form of fields (often known as attributes), and code, in the form of procedures (often known as methods). It promotes encapsulation, inheritance, and polymorphism, making designing, implementing, and maintaining complex systems easier.

## Microkernel Pattern

This pattern separates a minimal functional core (microkernel) from extended functionality and customer-specific parts. The microkernel contains the core functionality, while the other features are implemented as plugins to the microkernel. This allows the system to be easily extended without modifying the core.

![](https://miro.medium.com/v2/resize:fit:1184/1*8QRNElHUceu9S29QjePgng.png)

Microkernel Architecture Pattern

## Plug-in Pattern

This pattern allows adding new functionality to an application by adding new modules or plugins. The new modules are integrated into the application through a standard interface, which enables the application to be extended and customized. This pattern is commonly used in web browsers, media players, and content management systems.

# 3. Service-Oriented Architecture Style

This style designs software as a collection of services communicating with each other. Each service is self-contained and represents a specific business activity with a determined outcome.

- **Principles**: SOA designs applications as a collection of services that communicate over a network.
- **Strengths**: Flexibility, scalability, reusability, and loose coupling.
- **Weaknesses**: Increased complexity, network reliance, and potential performance issues.
- **Applications**: Enterprise systems, web services, microservices.
- **Antipatterns**: Ignoring business needs, using SOA where it’s not needed.

## Service-Oriented Architecture Pattern (SOA)

This pattern designs software as a collection of discrete services used within multiple systems. Each service in the SOA model is built to perform a specific business function, such as checking a customer’s credit score, calculating a payment, or processing a mortgage. These services communicate with each other over a network to achieve a specific activity, such as processing a mortgage application. SOA promotes reusability, as multiple applications and flexibility can use services, as services can be modified or replaced without affecting other services.

![](https://miro.medium.com/v2/resize:fit:1064/1*djFyxgmIXVL7kfAg17Z37w.png)

Service-Oriented Architecture Architecture Pattern (SOA)

## Broker Pattern

In the Broker architectural pattern, components of a system are communicated through a broker entity. The broker coordinates communication, such as forwarding requests and transmitting results and exceptions. This pattern structures distributed software systems with decoupled components that interact by remote service invocations.

## Microservice Pattern

This pattern designs a software application as a suite of small services, each running in its process and communicating with lightweight mechanisms, often HTTP. These services are built around business capabilities and are independently deployable by fully automated deployment machinery. This pattern allows for rapid, frequent, and reliable delivery of complex applications.

![](https://miro.medium.com/v2/resize:fit:932/1*ZP-1pmz94MibS6wHgaFZzg.png)

Microservice Architecture Pattern

## Serverless (Function as a Service or FaaS) Pattern

In this pattern, applications are built and run in cloud environments without considering servers. The cloud provider dynamically manages the allocation of machine resources, and developers can focus solely on individual functions in their application code. This pattern is ideal for scalable, event-driven applications.

# 4. Distributed System Architecture Style

This style refers to a system where components located on networked computers communicate and coordinate their actions by passing messages. The components interact with each other to achieve a common goal.

- **Principles**: This architecture involves multiple systems working together over a network to appear as a single system to the end user.
- **Strengths**: Scalability, fault tolerance, and resource sharing.
- **Weaknesses**: Increased complexity, network dependence, and issues related to data consistency.
- **Applications**: Distributed databases, cloud computing, telecommunication networks.
- **Antipatterns**: Not considering network failures, ignoring data consistency challenges.

## Space-Based Pattern

This pattern, also known as Tuple Space or Cloud Architecture, is designed to avoid any single point of failure or performance bottleneck by evenly distributing services and resources across multiple servers. It’s ideal for high-volume, mission-critical applications that require 100% uptime and horizontal scalability, such as financial trading systems or online gaming platforms.

![](https://miro.medium.com/v2/resize:fit:1400/1*Bo0t9CjiBOPDZHNi34RMlg.png)

Space-Based Architecture Pattern

## Peer-to-Peer (P2P) Pattern

In this pattern, each participant (peer) in the network acts as both a client and a server, and the network nodes communicate directly without needing a central server. This pattern is used in applications requiring distributed computing or resource sharing, such as file-sharing networks or blockchain technologies.

![](https://miro.medium.com/v2/resize:fit:824/1*zb0fGv_-8nyGhufB2oRKpQ.png)

Peer-to-Peer (P2P) Architecture Pattern

# 5. Domain-Driven Architecture Style

This style focuses on the core domain and domain logic, basing designs on the business domain model. It emphasizes collaboration between technical and domain experts to iteratively refine a model that is accurate and effective for solving business problems.

- **Principles**: Focuses on the core domain and domain logic and bases designs on a model of the domain.
- **Strengths**: Improving understanding of complex business domains and promoting communication between technical and business teams.
- **Weaknesses**: It can be overkill for simple domains and requires a deep understanding.
- **Applications**: Complex business systems, enterprise software.
- **Antipatterns**: Ignoring ubiquitous language, not involving domain experts.

## Hexagonal (Ports & Adapters) Pattern

This pattern allows an application to equally be driven by users, programs, automated test or batch scripts, and to be developed and tested in isolation from its eventual run-time devices and databases. It separates the software’s business logic from outside concerns, driven by ports and adapters. This pattern is ideal for applications that need to be decoupled from their software environment, allowing them to be adaptable to new environments.

![](https://miro.medium.com/v2/resize:fit:1400/1*ZienG21Tx0wnnlDY3RwOyA.png)

Hexagonal (Ports & Adapters) Architecture Pattern

## Domain-Driven Design Pattern

This pattern is an approach to software development for complex needs by connecting the implementation to an evolving model. It involves a close relationship between implementation and the current model, with a constant iteration cycle over both. DDD is ideal for complex systems with rich, intricate business rules or systems where the domain is in constant flux.

![](https://miro.medium.com/v2/resize:fit:1400/1*AgCIiL_2hqXIrZQzYtqSWg.png)

Domain-Driven Design Architecture Pattern

# 6. Event-Driven Architecture Style

Event-driven architecture is a software architecture and model for application design. With an event-driven system, the capture, communication, processing, and persistence of events are the core structure of the solution.

- **Principles**: This architecture style is driven by events like user actions, sensor outputs, or messages from other programs.
- **Strengths**: Highly scalable, loose coupling, promotes real-time or near-real-time information flow.
- **Weaknesses**: Increased complexity due to asynchronous programming can be hard to maintain and debug.
- **Applications**: GUI applications, real-time analytics, complex event processing.
- **Antipatterns**: Ignoring event order, lack of event durability.

## Event-Driven Pattern

Event-Driven architecture is a popular distributed asynchronous architecture pattern used to produce highly scalable applications. It’s also highly adaptable and can be used for small applications and large, complex systems.

![](https://miro.medium.com/v2/resize:fit:1400/1*sqtl0t8dOxYKuhNditAShw.png)

Event-Driven Architecture Pattern

## Pub-Sub Pattern

This is a messaging pattern where the senders of messages, known as publishers, do not program the messages to be sent directly to specific receivers, known as subscribers. Instead, published messages are characterized into topics without knowledge of which subscribers, if any, there may be. Similarly, subscribers express interest in one or more topics and only receive messages that are of interest without knowledge of which publishers, if any, there are. This pattern is widely used in asynchronous systems to decouple processes that produce events from processes that consume them, allowing for greater scalability and control.

# 7. Separation of Concern Architecture Style

Separation of concerns is a design principle for dividing a computer program into distinct sections, each addressing a separate concern.

- **Principles**: Different areas of functionality are handled by separate, independent sections of a system.
- **Strengths**: Improves understandability, reduces complexity, promotes modularity and parallel development.
- **Weaknesses**: This can increase complexity due to interface management and require more communication between modules.
- **Applications**: Almost all types of software systems.
- **Antipatterns**: Mixing concerns, lack of clear module boundaries.

## MVVC Pattern

This pattern facilitates separating the graphical user interface development from the business or back-end logic development. The view controller of MVVC is responsible for exposing the data objects from the model so that those objects are easily managed and presented. This pattern is widely used in extensive user interaction fields like desktop and mobile applications.

## MVP Pattern

This is a derivation of the model-view-controller (MVC) architectural pattern. It is used mainly for building user interfaces. In MVP, the presenter assumes the functionality of the “middle man.” The model and view are entirely separated and communicate with each other only through the presenter. MVP is an excellent architecture for modern web applications as it allows for easier automated unit testing and provides a clean structure for the project.

![](https://miro.medium.com/v2/resize:fit:992/1*YHp4M-18cRcP2enF8qFUWw.png)

Model-View-Presenter Architecture Pattern

# 8. Interpreter Architecture Style

An interpreter pattern is a design pattern that specifies how to evaluate sentences in a language. The basic idea is to have a class for each symbol (terminal or nonterminal) in a specialized computer language.

- **Principles**: Program instructions are executed directly without previously being converted into machine language.
- **Strengths**: Easier to debug and test, more flexible.
- **Weaknesses**: Slower than compiled languages, requires more resources.
- **Applications**: Scripting languages, some high-level programming languages.
- **Antipatterns**: Using interpreters where performance is critical.

![](https://miro.medium.com/v2/resize:fit:1024/1*o55QedM7HSmnq9GuAgtGUQ.png)

Interpreter Architecture Pattern

## Interpreter Pattern

This design pattern specifies how to evaluate sentences in a language. The basic idea is to have a class for each symbol (terminal or nonterminal) in a specialized computer language. The syntax tree of a sentence in the language is an instance of the composite pattern and is used to evaluate (interpret) the sentence for a client. This pattern is used in compilers and interpreters for programming languages, in regular expression engines, and processing and analyzing structured text data.

# 9. Concurrency Architecture Style

Concurrency is a property of systems in which several independent tasks are executed at the same time.

- **Principles**: Different parts of a program execute independently, potentially simultaneously.
- **Strengths**: Can significantly improve performance, especially on multi-core systems.
- **Weaknesses**: Designing and debugging problems with race conditions and deadlocks can be complex.
- **Applications**: Real-time systems, high-performance computing, web servers.
- **Antipatterns**: Ignoring potential concurrency issues and not correctly synchronizing shared resources.

## Orchestration Pattern

In this pattern, a controller (the ‘orchestrator’) controls the interaction between services. It dictates the business logic’s control flow and ensures everything happens on cue. This pattern is commonly used in complex business processes where you must coordinate multiple services and want centralized control.

![](https://miro.medium.com/v2/resize:fit:992/1*ejKZ0Hl16cKTDdL11aFNQA.png)

Orchestration Architecture Pattern

## Choreography Pattern

This pattern implements a system of services where each service works independently and interacts with other services through events. There is no central orchestrator. Instead, each service knows what to do next. This pattern is used when you want to create a decentralized, highly decoupled system that allows for more flexibility and scalability.

![](https://miro.medium.com/v2/resize:fit:992/1*AKs0QDaZW2Zk81ggduEWxw.png)

Choreography Architecture Pattern

## Primary-Secondary Pattern

This pattern consists of two types of components: Primary and Secondary. The primary component distributes the work among identical secondary components and computes a final result from the results these secondary machines return. This pattern is used in parallel computing to perform computations faster by dividing an enormous computation task among several processors.

## Pipeline / Pipe-Filter Pattern

This pattern involves a chain of processing elements (processes, threads, coroutines, etc.) arranged so that the output of one element is the input of the next. The idea is to break down a task that performs complex processing into separate elements that can be reused. This pattern is used in Unix and Unix-like operating systems for pipelining commands.

# 10. Data-Centric Architecture

This style focuses on how data is organized and transformed. It’s often used in systems that process large volumes of data, perform complex calculations, or need to be highly scalable.

- **Principles**: The database is at the center of the architecture, and all interactions occur through the database.
- **Strengths**: Can provide consistency, integrity, and reliability of data.
- **Weaknesses**: Can create data bottlenecks and potential scalability issues.
- **Applications**: Many enterprise applications, CRM systems, and ERP systems.
- **Antipatterns**: Ignoring the potential for data bottlenecks, not considering data scalability.

## Command Query Responsibility Segregation (CQRS) Pattern

This pattern separates read and write operations for a data store. It enables independent scaling of read and writes workloads and optimizes them separately. This pattern is ideal for applications with a high disparity between the read and write loads.

![](https://miro.medium.com/v2/resize:fit:1400/1*ZELFS-p3S3waIfguTNVOew.png)

Command Query Responsibility Segregation (CQRS) Architecture Pattern

## Event-Sourcing Pattern

This programming technique models the state changes applications make as an immutable sequence or “log” of events. Instead of modifying the state of the application in place, event sourcing involves storing the event that triggers the state change. This pattern is used in systems where it’s necessary to know the events that led to the current state, such as auditing systems or real-time collaborative applications.

## Kappa Pattern

This is a software architecture pattern. Rather than using a relational DB like SQL or a key-value store like Cassandra, the canonical data stored in a Kappa Architecture system is an append-only immutable log. This pattern is used in real-time data processing systems.

## Lambda Pattern

This data-processing architecture is designed to handle massive quantities of data by taking advantage of batch and stream-processing methods. It provides a robust, fault-tolerant system against hardware failures and human mistakes. This pattern is used in big data processing tasks.