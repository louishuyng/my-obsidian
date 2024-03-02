---
tags:
  - Architect
  - Fundamental
---

## 4 Principals Design
Here are the four principles of design:
1. Human rule. All design is social in nature.
2. Ambiguity rule. Preserve ambiguity.
3. Redesign rule. All design is redesign.
4. Tangibility rule. Make ideas tangible to facilitate communication.

We’ll use the acronym HART to help remember these principles. 

## Design Mindset
![images/design-mindsets.png](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781680502923/files/images/design-mindsets.png)

### Understand the Problem

In the understand mindset we actively seek information from stakeholders and work to describe the problem. The understand mindset is as much about requirements as it is empathy. To understand the problem, we must learn about the people who will be touched by our system and what they need.

To understand the problem, we’ll need to investigate business goals and quality attributes that are important to our stakeholders. We’ll also have to learn how our team operates and get a deeper sense of the priorities and trade-offs among design decisions.

### Explore Ideas

In the popular ethos, design thinking is all about brainstorming and sticky notes. Brainstorming is powerful, but it is only one practice in the explore mindset. When we explore, we create multiple design concepts and identify engineering approaches for solving some aspect of a problem.

Exploring software architecture means we try combinations of structures until we find a combination that best promotes desired quality attributes. To find the best mix of structures, we’ll need to survey a broad range of patterns, technologies, and development practices. When we’re planning the architecture, we’ll spend a lot of time in the exploration mindset, but this mindset is also useful when working with stakeholders.

### Make It Real

As you learned in [​_Make the Architecture Tangible_​](https://learning.oreilly.com/library/view/design-it/9781680502923/f_0020.xhtml#sec.tangibility.rule), ideas are great but if you can’t transfer them from your brain into someone else’s brain, then your ideas are useless. Making ideas real gives us a way to share them but also provides an opportunity for testing an idea. In the make mindset we turn our design concepts into real-world artifacts.

The most common ways we make architecture real is by creating models. Making goes way beyond box and line diagrams. You can make the architecture real by building prototypes, writing documents, crunching numbers, and a variety of other approaches.

The make mindset is useful for communicating our plans. We’ll also make the architecture real as we build the system—for example, by organizing our code so that it’s possible to see module structures in the architecture. Making is also an excellent way to push your team out of analysis paralysis.

### Evaluate Fit

How do you know if a design decision will solve the problem? When we embrace the evaluate mindset, we determine the fitness of our design decisions relative to our current understanding.

Evaluation is not an all or none proposition. We can evaluate all or part of the architecture, even only a single model, concept, or idea. The most common approach is to walk through a piece of the architecture with different scenarios, but we can also test design decisions directly by running experiments or examining the risks surrounding a decision.

The evaluate mindset comes in handy when we want to verify the planned or built architecture, but this is only the beginning. This mindset will help us inspect anything we make and decide whether that artifact is serving our need.

Using design mindsets requires a process with a tight feedback loop so we can quickly move from one mindset to the next. In the next section, we’ll learn how to use a simple, iterative approach to help us choose and use design mindsets.

## Think, Do and Check
![images/think-make-check.png](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781680502923/files/images/think-make-check.png)

### Iterate to Learn

An iteration can be as brief as a few minutes or as long as a few days. We prefer shorter cycles over longer ones, but sometimes more time is required for in-depth research. Every iteration follows the same steps, though the execution will vary depending on the design mindset we adopt.

Think

What do we hope to learn? What questions do we need answered? What are our top risks? Thinking involves creating a plan to learn what we need to answer specific questions or reduce risks.

Do

Execute the plan. Create something tangible that quickly and cheaply uncovers information needed to check our thinking and share our ideas.

Check

Critically examine what we accomplished during the do step so we can decide our next move. The insights coming out of the check step tell us what to do next. Repeat at the think step.