---
created: 2023-09-10T00:02:32 (UTC -04:00)
tags: []
source: https://learn.snyk.io/lesson/prototype-pollution/
author: 
dg-publish: true
---

> [!todo] 
> Learn how NoSQL Injection attacks work, and compare them to the similar SQL injection attacks with examples and remediation information
> 



---
### What is NoSQL injection?

NoSQL injection is a type of vulnerability where an attacker is able to inject arbitrary text into NoSQL queries. NoSQL injections are very similar to the traditional [SQL injection attack](https://learn.snyk.io/lessons/sql-injection/), except that the attack is against a NoSQL database. NoSQL is a general term for any database that does not use SQL, a common database management system (DBMS) that utilizes NoSQL is MongoDB.

### About this lesson

During this lesson, we will learn how NoSQL injections work, why they work, and how to prevent them. We will start by performing a NoSQL injection in an application using MongoDB, a popular document-structured database. We’ll then dive deep into the vulnerable code that allowed us to perform the attack, and finish by fixing up the code.

FUN FACT

### Meow attack! 🙀

In 2020, MongoDB was one of over 1000 databases hit by the “Meow” attack. The attack originated from a bot created to scan the internet for unsecured databases. Affected databases were permanently destroyed and overwritten with simply the word “Meow”. The attackers made no demands, indicating that they just did it to prove it was malicious. 🙀

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

The Best Botanist Awards show is coming up soon and Gianna the Gardner has been toiling over her garden for months now. Her hands are calloused from the manual labor, but it’s been all worth it, as her garden is in tip-top shape for the show. With some time to spare, Gianna decides to check out her competitors.

Scrolling through social media, she finds one of her newest competitors, Philippe. His feed shows an array of the most immaculate gardens, not a brown leaf or sagging flower in sight. Soon enough, Gianna finds out exactly how newcomer Philippe has maintained such a luxurious garden. His secret weapon is an automated IoT plant management application called PerfectPlant. The rules of the competition were quite clear, all automated IoT plant management was strictly forbidden!

The plants look amazing! How will they look after the attack?

### NoSQL attack

-   STEP 1
    
-   STEP 2
    
-   STEP 3
    
-   STEP 4
    
-   STEP 5
    
-   STEP 6
    

#### Setting the stage

Let's take down Philippe’s garden by hacking PerfectPlant. Now let's get to work and get Gianna her well-deserved trophy!

![nosql-start.svg](https://images.ctfassets.net/4un77bcsnjzw/40BvRte081fiffnI47Hfx6/4744e7ca461a139e92da0b5863874e96/nosql-start.svg)

We performed a Query Selector Injection in the login form by injecting the “not equal” MongoDB Query Selectors, `$ne`. By doing this we altered the logic of the login query so that instead of searching for:

“where the username is equal to philippesgarden and password is equal to 123”

This would have equated to `false`. We instead searched for:

“Where the username is equal to philippesgarden and password is not equal to 123”

The resulting Mongo query can be seen here: `{"username":{"$ne":null},"password":{"$ne":null}}`

Which equated to `true`! This allowed us to bypass authentication and access Philippe’s garden monitoring system. The login code is as follows:

After this, we injected some simple JavaScript which created a denial of service condition, created by an infinite while loop. NoSQL databases usually allow the execution of queries through a procedural language such as JavaScript.

We could execute these attacks because there was no sanitization performed on our query input. Let’s dive deeper and examine the code that handled the search query:

As is often the case with injection vulnerabilities, we must sanitize the user-supplied input to fix the NoSQL injection found in the example above. It’s very important to strictly validate any user-supplied input before it makes its way to any query string, NoSQL databases are not an exception to this rule!

### Login page

To fix this vulnerability, all we need to do is cast the user input to a string. Now, if an object such as `$ne` is provided, it will result in the following invalid Mongo query:

`{"username":"[object Object]","password":"[object Object]"}`

### Search bar

## Validator

To stop JavasScript from executing, simply set `javascriptEnabled` to `false` in the `mongod.conf` file. Additionally, it’s recommended that you try to avoid dangerous operators such as `where`, `mapreduce` and `group` with user input. For our solution, we’re going to alter our query to use the equals operator and cast our query to a string.

### Key Takeaways

-   NoSQL databases are vulnerable to injection vulnerabilities
-   NoSQL injections can potentially be more dangerous than SQL injections, as you may be able to execute code in the context of the application, not just the database
-   Remediate NoSQL injections by sanitizing your user-supplied input. You can do this with the popular helper library “Mongo-Sanitize” or using the “validator” npm library to validate any user-supplied input

### Test your knowledge!

Which of the following techniques is recommended for mitigating the risk of NoSQL injection when using MongoDB?

[Sign up to take the quiz](https://api.snyk.io/v1/learn/auth?cta=signup&page=learn-lesson&loc=body&learn_redirect_path=%2Flesson%2Fnosql-injection-attack%2F&learn_redirect_fragment=%23q-a6b7fcc5-89f3-4922-f1e8-d51d03b12330-e37c413b-ea22-573e-c870-671b3f46db4d)

## Congratulations

You’ve learned what a NoSQL injection is and how to protect your systems from it. Take this knowledge and go make your code safer!

Feel free to rate how valuable this lesson was for you and provide feedback to make it even better! Also, make sure to check out our lessons on other common vulnerabilities.
