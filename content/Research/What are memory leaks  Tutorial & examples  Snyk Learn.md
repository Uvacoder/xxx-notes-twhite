---
created: 2023-09-10T00:03:06 (UTC -04:00)
tags: []
source: https://learn.snyk.io/lesson/prototype-pollution/
author: 
dg-publish: true
---

> [!todo] 
> Learn about memory leaks, and how to mitigate and remediate the vulnerability with real-world examples from security experts.
> 



---
### What is a memory leak?

Memory leaks are a common source of performance issues and instability in JavaScript applications. A memory leak occurs when a Node.js program fails to release memory that it no longer needs, causing the program to consume more and more memory over time. This can lead to poor performance, slow response times, and ultimately, cause the application and other applications to crash.

When an application does not need a memory block anymore, it should release it back to the OS. In the case of a memory leak, the garbage collector never collects the block and it stays on the heap.

There are several common causes of memory leaks in Node.js, such as global variables, multiple references, and incorrect use of closures and timers.

### About this lesson

In this lesson, you will learn about vulnerabilities stemming from a memory leak and how to protect your applications against them. We will step into the shoes of a junior pentester, Jacob, who accidentally DoSed (Denial of Service) their client’s website when performing tests for a different type of vulnerability.

FUN FACT

### Garbage collector

Node.js uses a “garbage collector” to automatically manage memory and prevent memory leaks. However, if an application keeps references to objects, those will not be cleared by the garbage collector.

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

Jacob, a junior pentester for a cyber security firm in the south, is doing a pentest for SocialsCorp. SocialsCorp is a new social media platform where users have their own profile pages.

Jacob starts with trying to test the application for a common vulnerability type; IDOR (Insecure Direct Object Reference).

### Finding a memory leak

-   STEP 1
    
-   STEP 2
    
-   STEP 3
    
-   STEP 4
    
-   STEP 5
    

#### Setting the stage

Jacob's testing the \`user/{id}\` endpoint where he tries to access the details of other users by incrementing the ID value. Let's see what happens.

![memory-leaks-start.svg](https://images.ctfassets.net/4un77bcsnjzw/619HTAUCviJf6jyJeGWCtk/1d0694886f3b2efe639577d4ab458ab2/memory-leaks-start.svg)

FUN FACT

### Third-party libraries

Memory leaks do not always exist in the code of the company itself. It can also be caused by third-party libraries, or by incorrect use of third-party libraries.

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

Let’s break down what happened in the story above. Jacob was sending requests to the `user` endpoint with different user ID’s. After a handful of requests, the server got really slow and eventually crashed.

The backend code of the `user` endpoint was not releasing memory after use, causing allocated memory to be never released and overall memory usage to climb. The backend code of the `user` endpoint is the following:

The endpoint fetches the user from the database, then checks if the current logged-in user is allowed to view that user, and if so, returns the user object.

There are a few flaws here:

1.  The requested user is fetched from the database (if the `users` dictionary did not contain the user), regardless of the permissions that the authenticated user has.
2.  The retrieved users are stored in a global variable that will keep growing larger as more users are fetched.

Each time a user is requested, the `users` dictionary will grow. Jacob triggered an out of memory crash (a memory leak) by requesting a lot of different users in a short period of time. Because the user data can also contain profile pictures and other large data properties, the `users` dictionary got to a point where it was consuming all memory.

### What is the impact of a memory leak?

A memory leak in a Node.js application can have serious consequences for its performance and stability. If the process continues to consume more and more memory over time, it can eventually result in an out-of-memory error, causing the process to crash. Just like with the application Jacob was pentesting. Not only can the application that contains the leak crash but other applications might panic too if they are not able to allocate new memory.

Additionally, memory leaks can lead to slower performance as the process spends more time managing and garbage collecting the increasing amount of memory it is using. This leads to decreased responsiveness and a poor user experience.

To migrate a memory leak in a Node.js application, you can follow these steps:

-   Identify the source of the leak using tools such as the Node.js debugger/profiler and heap snapshot analyzers.
-   Analyze the cause of the leak by reviewing the code, analyzing performance data, and reproducing the issue.
-   Implement a fix to address the leak, which may involve refactoring code, optimizing data structures, or using different approaches to allocate and deallocate memory.
-   Test and validate the fix by using tools from step 1, to verify that the leak has been fixed.
-   Monitor and optimize your application's memory usage by performing regular performance testing, implementing best practices for memory management, and using tools like the Node.js debugger and heap snapshot analyzers to identify and fix any issues that may arise.

To migrate the memory leak in the `users` endpoint code:

-   Implement an eviction policy: To prevent the users object from growing indefinitely, you can implement a policy that removes the least recently used users from the object to make room for new ones.
-   Use a cache library: Instead of manually implementing a cache, you could use a library that has built-in eviction policies and expiration times. This can help to automatically manage the `users` object and prevent it from growing too large.
-   Use a memory-efficient data structure: Instead of using an object to store the users, you could consider using a data structure that is more memory-efficient, such as a linked list or a queue. This will allow you to store a large number of users without consuming too much memory.
-   Additionally, the objects stored in memory should only contain minimal information. Profile pictures or other large data should be referenced and not stored directly.
-   Monitor memory usage: Regularly monitoring your application's memory usage can help you identify potential memory leaks and take steps to fix them before they become a problem. You can use tools such as the Node.js debugger and heap snapshot analyzers to monitor memory usage and identify any issues.
-   Had target.com monitored their memory usage, they would have spotted the memory leak themselves instead of stumbling upon it when it was too late.
-   Only store data that you really need. Perform the right checks, such as permission checks, first, and only then fetch the user object. Storing the user object in the `users` dictionary while you don’t even know you if you need it, is a waste of resources and memory space.

A good caching library in Node.js is `cache-manager`. You can find more about this package, including a security scan and health check, at [https://snyk.io/advisor/npm-package/cache-manager](https://snyk.io/advisor/npm-package/cache-manager)

Implementing the `cache-manager` package into the `users` endpoint code, and performing the permissions check first, will look like something this:

FUN FACT

### Built-in profiler

Node.js has a built-in profiler that can be used to detect memory leaks and profile the performance of your application. To use the profiler, you can use the `--prof` flag, which communicates with the V8 profiler to gather information from functions and store it in a file.

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

### Keep learning

-   OWASP has a page dedicated to memory leaks that can be found here: [https://owasp.org/www-community/vulnerabilities/Memory\_leak](https://owasp.org/www-community/vulnerabilities/Memory_leak)
-   You can also read more about this CWE at [https://cwe.mitre.org/data/definitions/401.html](https://cwe.mitre.org/data/definitions/401.html)
-   Check out our Snyk blog post about [memory leaks and Python](https://snyk.io/blog/diagnosing-and-fixing-memory-leaks-in-python?loc=learn)

## Congratulations

Now you know more about memory leaks and how to prevent them! We hope that you will apply this knowledge to make your applications safer. We'd really appreciate it if you could take a minute to rate how valuable this lesson was for you and provide feedback to help us improve! Also, make sure to check out our lessons on other common vulnerabilities.
