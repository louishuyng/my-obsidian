---
tags: Nodejs, Backend
---
## Parallel processing in Node.js using worker threads

Node.js is one of the most preferred technologies out there to build real-time web applications due to its non-blocking, event-driven and, single-threaded nature. Node.js was built with an intention to develop applications that can efficiently handle I/O intensive operations.

### Why do we need parallel processing?

One of the most important components of Node's architecture is [**libuv**](https://libuv.org/). libuv is a C++ based library that helps in asynchronously executing I/O based operations in a non-blocking manner. It also provides an interface called the Event Loop which takes the code to be executed from the call stack and executes it. Because of the single-threaded nature, the Event Loop executes only one thing at a time.

When the Event Loop encounters code that must be executed asynchronously (e.g., accessing the file system, making network calls, and other I/O operations), instead of executing the code on its own, it delegates it to a pool of threads and moves on to executing the remainder of the program. The threads then parallelly execute the asynchronous code and return an event to the Event Loop once done.

![Node.js architecture](https://uploads-ssl.webflow.com/63f08f94fb9f06134f8c54eb/643f95071970a4e30797966e_arch.png)

This non-blocking asynchronous behavior of Node.js for I/O operations helps maximize the throughput and makes it an ideal choice for building I/O intensive applications.

However, there is no rose without a thorn. The I/O focused architecture and Event Loop's single-threaded nature also mean that Node.js is not very performant with CPU intensive operations.

Unlike the I/O operations, CPU-based operations are executed by the Event Loop itself and not delegated to the thread pool for asynchronous execution. Hence, for time consuming CPU operations, the Event Loop remains occupied, and the program is blocked from further execution.

So, does this mean Node.js is a big NO when it comes to developing applications that have heavy CPU operations? Not really!

Though Node.js is primarily designed for I/O intensive applications, it provides us with additional abilities to efficiently handle CPU demanding tasks. And one of the ways to do so is by using **Worker Threads**.

### Worker threads to the rescue

Worker Threads help us offload CPU intensive tasks away from the Event Loop to be executed parallelly in a non-blocking manner. A worker thread runs a piece of code as instructed by the parent thread in isolation from the parent and other worker threads. Each worker thread has its own isolated V8 environment, event loop, event queue, etc. A worker and parent can communicate with each other through a messaging channel. Worker threads provide us with a way to run multiple threads under a single process.

Apart from keeping the Event Loop free of time consuming CPU operations, we can also use a pool of worker threads to divide and execute heavy CPU operations in parallel to increase the performance of our Node.js application.

To better understand the benefits of Node.js worker threads, let us take a simple CPU intensive task as an example and compare the task's execution time with various scenarios.

### The task at hand

Let's say we're given a directory with 1000 JSON files in it, and we're tasked with converting each JSON file to an XML file.

Each JSON file has an array of 1000 objects, with each object having 6 key-value pairs as shown in the example below.

‍

```perl
 
{
  "id": 1,
  "first_name": "abc",
  "last_name": "xyz",
  "email": "abcxyz@360.cn",
  "gender": "Male",
  "ip_address": "163.213.59.129"
}
```

Our program will read each JSON file, parse it and convert it to an XML output.

Reading/writing files are I/O operations, and Node.js will handle them asynchronously.

However, parsing the JSON and converting it to XML will be a CPU demanding operation since we have a huge number of large JSON objects.

### Execution scenarios

_Note: In all the scenarios, the time taken to read the contents from the JSON file is not added to the execution time. We have used the_ [**_process.hrtime.bigint()_**](https://nodejs.org/api/process.html#processhrtimebigint) _function to calculate the execution time. To balance errors, We will run each scenario 5 times and take the average time taken as the execution time._

#### Scenario 1 - Synchronous execution without worker threads

Here, we parse and convert the JSON content in a synchronous way without using worker threads.

‍

```javascript
 
/* index.js */

// Read all JSON file contents into an array
const contents = getContents()

/* Execution time start */
const start = hrtime.bigint()

// Convert each JSON file content to XML format
const result = contents.map((content) => {
  content = JSON.parse(content)
  return js2xmlparser.parse('user', content)
})

/* Execution time end */
const end = hrtime.bigint()
console.info(`Execution time: ${(end - start) / BigInt(10 ** 6)}ms`)
```

⏰ **Average execution time:** 1036 ms

#### Scenario 2 - Asynchronous execution with one worker thread

Here, we parse and convert the JSON content in an asynchronous way using one worker thread.

We will use the in-built worker_threads module from Node.js to manage the Worker Threads

‍

```javascript
 
/* index.js */

const { Worker } = require('worker_threads')

// Read all JSON file contents into an array
const contents = getContents()

/* Execution time start */

// Create a new worker
const worker = new Worker('./worker.js')

// Send the contents to the worker
worker.postMessage(contents)

// Get result from the worker
worker.on('message', (result) => {
  /* Execution time end */
  // `result` has the converted XML
})
```

```javascript
 
/* worker.js */

const { parentPort } = require("worker_threads");
const js2xmlparser = require("js2xmlparser");

// Receive message from the parent
parentPort.on("message", (contents) => {
        // Send result back to parent
    parentPort.postMessage(
        contents.map(content => {
            content = JSON.parse(content);
            return js2xmlparser.parse("user", content);
        });
    );
});
```

⏰ **Average execution time:** 1146 ms

  

In this scenario, we see a marginal increase in the execution time. Though the CPU demanding operation is run on a separate thread now, the worker will also take a similar amount of time to execute the operation. Creating a new worker thread in itself is an expensive operation, and hence we see an increase in the execution time.

Asynchronously executing a CPU operation on a single thread can be beneficial if you have some other part of the program that can execute in parallel in the main thread. Since, in our use case, we do not have any instructions to be executed after the conversion operation, the main thread will wait till the child thread finishes its execution.

However, we can divide our CPU intensive (JSON to XML conversion) operation into multiple parts and assign each part to a separate thread. We can assign each part to separate worker threads, or we could assign one part to a worker thread and the other part we can let the main thread process by itself. Either way, we will have more than one thread parallelly working on the operation. For now, let's assume we want to keep the main thread free for some other task and go with the approach of executing in two worker threads.

Though we can achieve the above use case by creating an array of worker threads, it would be preferable to use a worker thread pooling library that will simplify and abstract the management of worker threads for us.

#### Scenario 3 - Parallel execution with two worker threads

Here, we parse and convert the JSON into two parts. Half the content to be converted (50 JSON files), we will assign to a worker thread, and the remaining half of the content we will assign to another worker thread. To manage the worker threads, we will use a pooling library called [**piscina**](https://www.npmjs.com/package/piscina).

```javascript
 
/* index.js */

const Piscina = require('piscina')

// Read all JSON file contents into an array
const contents = getContents()

/* Execution time start */

// Divide the content into two chunks
const chunks = splitToChunks(contents, 2)

// Create a new thread pool
const pool = new Piscina()
const options = { filename: resolve(__dirname, 'worker-pool') }

// Run operation on the chunks parallely
const result = await Promise.all([pool.run(chunks[0], options), pool.run(chunks[1], options)])

/* Execution time end */
```

```javascript
 
/* worker-pool.js */

const js2xmlparser = require('js2xmlparser')

module.exports = async (contents) => {
  return contents.map((content) => {
    content = JSON.parse(content)
    return js2xmlparser.parse('user', content)
  })
}
```

‍  

⏰ **Average execution time:** 687 ms

Using two worker threads, we can see a significant reduction in the execution time compared to the previous scenarios. Having more than one thread parallelly processing parts of the CPU intensive operation increases our program's performance considerably.

Next, let us try dividing the above operation even further so that we can employ more number of threads and see the impact on performance.

Below you can find the execution timings that I recorded with different numbers of worker threads using piscina library.

**No. of Worker Threads2345678**Execution Time (in ms)687577476502527558609

![Performance chart](https://uploads-ssl.webflow.com/63f08f94fb9f06134f8c54eb/643f950746d6dd69056a25b1_chart.png)

From the above data, we can see that the execution time kept reducing till 4 worker threads, and after that, it started to increase gradually. More threads also mean more time for creating threads, communication between threads, and context switching.

### Final notes

Employing worker threads to process CPU intensive operations parallelly does provide considerable performance improvements. However, this does not mean that more threads directly correspond to better performance.

We can also use [**ArrayBuffer**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) and [**SharedArrayBuffer**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) to implement memory transfer or share between threads to make the execution more efficient.

Considering all of the above, we need to decide on the most optimum number of threads to give the best performance for the use case.