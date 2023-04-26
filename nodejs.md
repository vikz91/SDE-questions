# Node.JS


* [Top 10 Concept questions](#top-10-concept-questions)
  + [1. Explain the difference between process.nextTick() and setImmediate(). How do they affect the event loop?](#1-explain-the-difference-between-processnexttick---and-setimmediate---how-do-they-affect-the-event-loop-)
  + [2. How does the Node.js event loop work? Describe its phases and how they relate to the libuv library.](#2-how-does-the-nodejs-event-loop-work--describe-its-phases-and-how-they-relate-to-the-libuv-library)
  + [3. What is a readable stream and a writable stream in Node.js? Explain their main characteristics and provide examples of when to use each.](#3-what-is-a-readable-stream-and-a-writable-stream-in-nodejs--explain-their-main-characteristics-and-provide-examples-of-when-to-use-each)
  + [4. How do you create a secure RESTful API in Node.js? Describe best practices for authentication, authorization, and data validation.](#4-how-do-you-create-a-secure-restful-api-in-nodejs--describe-best-practices-for-authentication--authorization--and-data-validation)
  + [5. Explain the concept of "middleware" in Express.js. How is it used to modify the request-response cycle?](#5-explain-the-concept-of--middleware--in-expressjs-how-is-it-used-to-modify-the-request-response-cycle-)
  + [6. Describe the role of cluster module in Node.js. How can it be used to scale a Node.js application and optimize performance?](#6-describe-the-role-of-cluster-module-in-nodejs-how-can-it-be-used-to-scale-a-nodejs-application-and-optimize-performance-)
  + [7. What is the difference between using callbacks, Promises, and async/await in Node.js? Discuss the advantages and disadvantages of each approach.](#7-what-is-the-difference-between-using-callbacks--promises--and-async-await-in-nodejs--discuss-the-advantages-and-disadvantages-of-each-approach)
  + [8. Explain the concept of Event Emitters in Node.js. Provide examples of how they can be used to create custom events and manage asynchronous operations.](#8-explain-the-concept-of-event-emitters-in-nodejs-provide-examples-of-how-they-can-be-used-to-create-custom-events-and-manage-asynchronous-operations)
  + [9. How do you handle error handling and exception handling in a Node.js application? Discuss best practices for catching and logging errors.](#9-how-do-you-handle-error-handling-and-exception-handling-in-a-nodejs-application--discuss-best-practices-for-catching-and-logging-errors)
  + [10. What is the role of the package.json file in a Node.js project? Describe its main elements and how it's used for dependency management and project configuration.](#10-what-is-the-role-of-the-packagejson-file-in-a-nodejs-project--describe-its-main-elements-and-how-it-s-used-for-dependency-management-and-project-configuration)
* [5 More Advanced Questions](#5-more-advanced-questions)
  + [11. How does garbage collection work in Node.js, and how can you optimize memory management?](#11-how-does-garbage-collection-work-in-nodejs--and-how-can-you-optimize-memory-management-)
  + [12. Explain the concept of "Worker Threads" in Node.js and how they can be used to handle CPU-intensive tasks.](#12-explain-the-concept-of--worker-threads--in-nodejs-and-how-they-can-be-used-to-handle-cpu-intensive-tasks)
  + [13. How can you implement caching in a Node.js application to improve performance?](#13-how-can-you-implement-caching-in-a-nodejs-application-to-improve-performance-)
  + [14. Explain the difference between the "crypto" and "bcrypt" libraries in Node.js and when to use each.](#14-explain-the-difference-between-the--crypto--and--bcrypt--libraries-in-nodejs-and-when-to-use-each)
  + [15. What are the best practices for logging and monitoring a Node.js application in a production environment?](#15-what-are-the-best-practices-for-logging-and-monitoring-a-nodejs-application-in-a-production-environment-)

## Top 10 Concept questions

### 1. Explain the difference between process.nextTick() and setImmediate(). How do they affect the event loop?
`process.nextTick()` schedules a callback function to be executed on the next iteration of the event loop, before any I/O events or timers are executed.  
This is useful when you want to prioritize a task to be completed as soon as possible, without blocking the event loop.  

`setImmediate()` schedules a callback function to be executed after the current event loop iteration has completed all I/O events and timers.  
This is useful when you want to execute a task after all current tasks have been completed, without starving I/O operations.

```JS
console.log('Start');

setImmediate(() => {
  console.log('Set immediate');
});

process.nextTick(() => {
  console.log('Next tick');
});

console.log('End');

/* OUTPUT

Start
End
Next tick
Set immediate

*/
```

### 2. How does the Node.js event loop work? Describe its phases and how they relate to the libuv library.
The event loop is the central mechanism in Node.js that handles asynchronous operations. 
It works in conjunction with the libuv library and consists of several phases:

1. Timers: Executes callbacks scheduled by setTimeout() and setInterval().
2. Pending Callbacks: Executes I/O callbacks deferred from the previous loop iteration.
3. Poll: Retrieves new I/O events and executes corresponding callbacks.
4. Check: Executes setImmediate() callbacks.
5. Close: Executes 'close' event callbacks.

```JS
setTimeout(() => {
  console.log('setTimeout');
}, 0);

setImmediate(() => {
  console.log('setImmediate');
});


/* OUTPUT

setTimeout
setImmediate

*/

```

### 3. What is a readable stream and a writable stream in Node.js? Explain their main characteristics and provide examples of when to use each.
_Readable streams_ represent data sources from which data can be read. They can be used to read data from a file, a network connection, or any other data source. _Writable streams_ represent destinations to which data can be written, like a file or a network connection.  

Readable streams have a `data` event that is emitted when data is available to read. Writable streams have a 'write' method to write data and a `end` method to indicate that no more data will be written.


```JS
// Reading from a file using a readable stream
const fs = require('fs');
const readStream = fs.createReadStream('input.txt');

readStream.on('data', (chunk) => {
  console.log(chunk.toString());
});

// Writing to a file using a writable stream
const writeStream = fs.createWriteStream('output.txt');
writeStream.write('Hello, ');
writeStream.write('World!');
writeStream.end();

```


### 4. How do you create a secure RESTful API in Node.js? Describe best practices for authentication, authorization, and data validation.
To create a secure RESTful API in Node.js, consider the following best practices:

- Use HTTPS to encrypt data in transit.
- Implement authentication using secure mechanisms like JWT or OAuth.
- Implement authorization to restrict access to resources based on user roles.
- Validate and sanitize user input to prevent attacks like SQL Injection, XSS or RCE.
  - Validation: Check if the user input meets certain criteria (e.g., data type, length, format) before processing it with libraries like  `joi`.
  - Sanitization: Remove or replace any potentially harmful characters or content from user input with libraries like `validator` or `sanitize-html`.
- Additional Mesaures:
  - Use security headers like 'Content-Security-Policy', 'X-Content-Type-Options', 'X-Frame-Options', and 'X-XSS-Protection' to protect against various attacks.
  - Implement rate limiting to prevent abuse of your API by limiting the number of requests a user can make within a specific time frame.
  - Keep your dependencies up to date to reduce the risk of security vulnerabilities in third-party packages.
  - Use CORS (Cross-Origin Resource Sharing) to restrict which domains can access your API and prevent unauthorized access from different origins. 


```JS
// Generate a token when the user logs in
const jwt = require('jsonwebtoken');
const token = jwt.sign({ userID: 1, role: 'admin' }, 'secret_key', { expiresIn: '1h' });

// Verify the token in subsequent requests
const express = require('express');
const app = express();

app.use((req, res, next) => {
  const token = req.headers.authorization;
  try {
    const decoded = jwt.verify(token, 'secret_key');
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ message: 'Invalid token' });
  }
});

```


### 5. Explain the concept of "middleware" in Express.js. How is it used to modify the request-response cycle?
Middleware are functions that have access to the request object (req), response object (res), and the next middleware in the application's request-response cycle. They can be used to modify the request or response, perform logging, or execute any code that needs to run during the processing of a request.

```JS
const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log(`Request method: ${req.method}, URL: ${req.url}`);
  next();
});

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### 6. Describe the role of cluster module in Node.js. How can it be used to scale a Node.js application and optimize performance?
The cluster module in Node.js allows you to create multiple instances of your application to take advantage of multi-core systems. By distributing incoming connections and workload among different worker processes, you can improve the performance and scalability of your application.

```JS
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('Hello, World!');
  }).listen(3000);
}
```

### 7. What is the difference between using callbacks, Promises, and async/await in Node.js? Discuss the advantages and disadvantages of each approach.
- Callbacks: Functions passed as arguments to asynchronous functions to handle the results. They can lead to "callback hell" when dealing with multiple nested callbacks.
- Promises: Objects representing the eventual completion (or failure) of an asynchronous operation. They help with chaining asynchronous operations and error handling.
- async/await: A syntax that simplifies working with Promises, making asynchronous code look and behave like synchronous code.


```JS
// Using Callbacks
function getData(callback) {
  setTimeout(() => {
    callback(null, 'Data received');
  }, 1000);
}

getData((error, data) => {
  if (error) {
    console.log(error);
  } else {
    console.log(data);
  }
});

// Using Promises
function getDataPromise() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data received');
    }, 1000);
  });
}

getDataPromise()
  .then(data => console.log(data))
  .catch(error => console.log(error));

// Using async/await
async function fetchData() {
  try {
    const data = await getDataPromise();
    console.log(data);
  } catch (error) {
    console.log(error);
  }
}

fetchData();
```

### 8. Explain the concept of Event Emitters in Node.js. Provide examples of how they can be used to create custom events and manage asynchronous operations.
Event Emitters are objects in Node.js that emit named events and allow other objects to listen for those events. They provide a way to create custom events and manage asynchronous operations. To create an event emitter, you can use the 'events' module and instantiate an EventEmitter object.


```JS
const EventEmitter = require('events');

const myEmitter = new EventEmitter();

myEmitter.on('event', () => {
  console.log('An event occurred!');
});

myEmitter.emit('event');
```


### 9. How do you handle error handling and exception handling in a Node.js application? Discuss best practices for catching and logging errors.
In Node.js, we can use try-catch blocks for synchronous operations, and error-first callbacks or Promise rejections for asynchronous operations. Best practices include:

- Centralizing error handling.
- Logging errors for monitoring and debugging.
- Gracefully shutting down the application in case of uncaught exceptions.

```JS
// Synchronous error handling
try {
  const result = someFunctionThatMightThrow();
  console.log(result);
} catch (error) {
  console.error('Error:', error.message);
}

// Asynchronous error handling
function asyncFunction(callback) {
  setTimeout(() => {
    try {
      const result = someFunctionThatMightThrow();
      callback(null, result);
    } catch (error) {
      callback(error);
    }
  }, 1000);
}

asyncFunction((error, result) => {
  if (error) {
    console.error('Error:', error.message);
  } else {
    console.log(result);
  }
});

```
### 10. What is the role of the package.json file in a Node.js project? Describe its main elements and how it's used for dependency management and project configuration.

The `package.json` file is a crucial part of any Node.js project. It serves as a manifest file that contains metadata about the project and is used for managing dependencies, configuring scripts, and storing other project-related information. Here's a brief overview of the primary elements of a package.json file:
- name: The name of the project.
- version: The current version of the project, following semantic versioning.
- description: A brief description of the project.
- main: The entry point of the project, typically the main JavaScript file.
- scripts: A collection of scripts that can be run using npm run script-name. These scripts are used for automating tasks such as starting the application, running tests, or building the project.
- dependencies: A list of packages required for the application to run in production. These packages are installed when running npm install without any arguments.
- devDependencies: A list of packages required for development purposes, such as testing or building the application. These packages are not installed when deploying the application to production.
- author: The name of the project's author or maintainer.
- license: The type of license used for the project, such as MIT or GPL.

```JSON
{
  "name": "my-nodejs-app",
  "version": "1.0.0",
  "description": "A simple Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  },
  "author": "John Doe",
  "license": "MIT"
}
```

## 5 More Advanced Questions
### 11. How does garbage collection work in Node.js, and how can you optimize memory management?
Garbage collection in Node.js is managed by the V8 JavaScript engine. It uses a generational garbage collection strategy, which involves a combination of the Scavenge (for young/small objects) and Mark-Sweep (for old/large objects) algorithms. To optimize memory management, you can use weak references, object pools, or manually trigger garbage collection (not recommended for production).  

Launch the node process with the `--expose-gc` flag, you can then call `global.gc()` to force node to run garbage collection

### 12. Explain the concept of "Worker Threads" in Node.js and how they can be used to handle CPU-intensive tasks.
Worker Threads can be used to handle CPU-intensive tasks without blocking the main event loop. They are useful for offloading work to separate threads and improving application performance.
```JS
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
  // Main thread
  const worker = new Worker(__filename);
  worker.on('message', (result) => {
    console.log(`Result: ${result}`);
  });
  worker.postMessage(5);
} else {
  // Worker thread
  parentPort.on('message',; (data) => {
    const result = factorial(data);
    parentPort.postMessage(result);
  });

  function factorial(n) {
    if (n === 0 || n === 1) {
      return 1;
    }
    return n * factorial(n - 1);
  }
}


```

### 13. How can you implement caching in a Node.js application to improve performance?
Caching can be implemented in Node.js applications using in-memory caches, key-value stores like Redis, or reverse proxies like Varnish or Nginx.

```JS
const express = require('express');
const redis = require('redis');
const axios = require('axios');
const app = express();

const client = redis.createClient();

app.get('/data/:id', async (req, res) => {
  const id = req.params.id;

  client.get(id, async (err, data) => {
    if (data) {
      res.send(data);
    } else {
      const response = await axios.get(`https://api.example.com/data/${id}`);
      client.set(id, JSON.stringify(response.data));
      res.send(response.data);
    }
  });
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});

```

### 14. Explain the difference between the "crypto" and "bcrypt" libraries in Node.js and when to use each.
The crypto library is a built-in module in Node.js for working with cryptography, while bcrypt is an external library for hashing passwords. Use crypto for general-purpose cryptography tasks and bcrypt specifically for password hashing and verification.
```JS
const bcrypt = require('bcrypt');

async function hashAndVerifyPassword() {
  const plainPassword = 'myPassword';
  const saltRounds = 10;

  // Hash the password
  const hashedPassword = await bcrypt.hash(plainPassword, saltRounds);
  console.log(`Hashed password: ${hashedPassword}`);

  // Verify the password
  const isValid = await bcrypt.compare(plainPassword, hashedPassword);
  console.log(`Is valid: ${isValid}`);
}

hashAndVerifyPassword();

```


### 15. What are the best practices for logging and monitoring a Node.js application in a production environment?
Best practices for logging and monitoring a Node.js application in production include using a logging library like Winston or Bunyan, configuring log levels, log rotation, and log aggregation, and setting up monitoring tools like Prometheus, Grafana, or ELK Stack.

```JS
const express = require('express');
const winston = require('winston');
const app = express();

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.printf(({ timestamp, level, message }) => {
      return `[${timestamp}] ${level}: ${message}`;
    })
  ),
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

app.get('/', (req, res) => {
  logger.info('GET request received at /');
  res.send('Hello, World!');
});

app.listen(3000, () => {
  logger.info('Server running on port 3000');
});

```
