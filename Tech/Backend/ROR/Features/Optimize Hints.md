---
tags: ROR, Backend
---
Optimize your code by using `optimizer_hints`

Ruby on Rails' optimizer_hints method allows you to provide hints to the compiler regarding code optimization. This will lead to improved performance, especially for frequently executed code.

Here are some tips for using the optimizer_hints method effectively:

Use the `hot` hint for code executed more than 100 times per second.

Use the `cold` hint for code executed less than 10 times per second.

Use the `size` hint to tell the compiler how much memory the code uses.

Use the c`omplexity` hint to tell the compiler how complex the code is.

I want to let you know that the optimizer_hints method should only be used when it is necessary to improve performance. The optimizer_hints method can make the code more difficult to read and understand So use it wisely.

It is also important to note that the compiler may be unable to optimize the code according to the hints provided. The hints may be conflicting, contradictory, incorrect, outdated, and many more.

![[Pasted image 20230529153128.png]]