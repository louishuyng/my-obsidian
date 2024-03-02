---
tags:
  - Architect
  - FP
---

## Functional programmers distinguish inert data from code that does work

We can draw another line between _calculations_ and _data_. Neither calculations nor data depend on when or how many times they are used. The difference is that calculations can be executed, while data cannot. Data is inert and transparent. Calculations are opaque, meaning that you don’t really know what a calculation will do until you run it.

![Image](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617296208/files/OEBPS/Images/f0008-01.jpg)

The distinction between actions, calculations, and data is fundamental to FP. Any functional programmer would agree that this is an important skill. Most other concepts and skills in FP are built on top of it.

## The three categories of code in FP

Let’s go over the characteristics of the three categories:

### 1. Actions

Anything that depends on when it is run, or how many times it is run, or both, is an _action_. If I send an urgent email today, it’s much different from sending it next week. And of course, sending the same email 10 times is different from sending it 0 times or 1 time.

Actions**

- Tools for safely changing state over time
- Ways to guarantee ordering
- Tools to ensure actions happen exactly once

** FP has tools for using each category

### 2. Calculations

_Calculations_ are computations from input to output. They always give the same output when you give them the same input. You can call them anytime, anywhere, and it won’t affect anything outside of them. That makes them really easy to test and safe to use without worrying about how many times or when they are called.

Calculations

- Static analysis to aid correctness
- Mathematical tools that work well for software
- Testing strategies

### 3. Data

_Data_ is recorded facts about events. We distinguish data because it is not as complex as executable code. It has well-understood properties. Data is interesting because it is meaningful without being run. It can be interpreted in multiple ways. Take a restaurant receipt as an example: It can be used by the restaurant manager to determine which food items are popular. And it can be used by the customer to track their dining-out budget.

Data

- Ways to organize data for efficient access
- Disciplines to keep records long term
- Principles for capturing what is important using data