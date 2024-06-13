---
created: 2023-09-10T00:02:09 (UTC -04:00)
tags: 
source: https://learn.snyk.io/lesson/prototype-pollution/
author: null
dg-publish: true
---

> [!todo] 
> Learn what JavaScript prototype pollution is and how to prevent it.
> 


---
### What is prototype pollution?

Prototype pollution is an injection attack that targets JavaScript runtimes. With prototype pollution, an attacker might control the default values of an object's properties. This allows the attacker to tamper with the logic of the application and can also lead to denial of service or, in extreme cases, remote code execution.

After reading the above definition, there are probably at least a dozen questions that spring to mind. What does it actually mean to “override object attributes at runtime”? How can it affect the [security of my application](https://snyk.io/learn/application-security/?loc=learn)? And, most importantly, how do I protect my code against this attack?

### About this lesson

Prototype pollution can be complex, so we will walk through it in three steps.

1.  You will use prototype pollution to compromise a vulnerable API
2.  You will learn more about JavaScript prototypes and how prototype pollution works
3.  And, you will learn how to fix and prevent prototype pollution in your applications

FUN FACT

### Not an isolated case

Prototype pollution vulnerabilities have been found and fixed in many popular JavaScript libraries, including [jQuery](https://snyk.io/vuln/SNYK-JS-JQUERY-174006/?loc=learn), [lodash](https://snyk.io/vuln/SNYK-JS-LODASH-450202), [express](https://snyk.io/vuln/SNYK-JS-EXPRESSFILEUPLOAD-595969), [minimist](https://snyk.io/vuln/SNYK-JS-MINIMIST-559764), [hoek](https://snyk.io/vuln/npm:hoek:20180212)… and the list goes on. When a prototype pollution vulnerability was discovered in jQuery, jQuery was--at that time--being used in 74% of all websites. Talk about scary!

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

Let’s demonstrate how a prototype pollution attack may play out in the real world. A company called startup.io, which we might recognize from the other lessons, finally acquired users for its products. Obliged by .io in its name, startup.io decided to release an API that allows users to manage data the company holds via the app.

Unfortunately, stressed by looming deadlines and chased by ever-demanding stakeholders, startup.io engineers did a bad job of securing their API. They never had time to schedule a meeting with their AppSec team to help with the design and they later ignored all the issues reported by the security scanners. As a result, their API contains many bugs and vulnerabilities--one of which is prototype pollution.

This is obviously bad for startup.io--but good news for us because with it we can compromise their API. Let’s focus on two API endpoints that startup.io exposes:

-   An HTTP POST on `https://api.startup.io/users/:userId` that allows updating data for a user with `userId`
-   An HTTP GET on `https://api.startup.io/users/:userId/role` that allows retrieving the security role that a given user is currently assigned to (either `admin` or `user`)

### Vulnerable API

Let’s try to escalate our privileges to adminhood by tampering with the application logic. Then, let’s try to bring down the whole API with a denial of service attack.

All examples assume we are already authorized, and any authorization headers are omitted for readability. To interact with the API, we will be using an embedded terminal window like the one below.

Let’s examine how the HTTP POST endpoint works by sending a valid request. We read in the docs that the endpoint allows us to change the text in the “about” section that's displayed on our user’s profile page. We are good at sanitizing database code, so we want our “about” section to say “Database sanitization expert”.

Copy and paste the following into the terminal and hit enter:

`curl -H "Content-Type: application/json" -X POST -d '{"about": "Database sanitization expert"}' https://api.startup.io/users/1337`

We should get a JSON response with the data stored about the user, with the “about” text updated:

{ name: "Robert", surname: "Tables", about: "Database sanitization expert" }

Next, let’s see how the HTTP GET endpoint works by sending another valid request.

Copy and paste the following into the terminal and hit enter:

`curl -X GET https://api.startup.io/users/1337/role`

We should get back JSON with the default role our user is assigned:

`{ role: "user" }`

### Hacking #1: naive, failed attempt

Now that we know how the API works, let’s see if we can modify our role and set it to `admin`. Try setting the `role` attribute to `admin` directly via a POST request.

Copy and paste the following into the terminal and hit enter:

`curl -H "Content-Type: application/json" -X POST -d '{"role": "admin"}' https://api.startup.io/users/1337 && curl -X GET https://api.startup.io/users/1337/role`

We should get the following output:

`{ role: "user" }`

Alas, this simple approach did not work. The role attribute stubbornly remains set to user. However, our quest to transcend to adminhood is not over yet!

### Hacking #2: privilege escalation with prototype pollution

Earlier, we discovered that prototype pollution might allow us to override any attribute defined on any object in the application. Perhaps the vulnerability could allow us to change the `role` attribute? Let’s try again--but this time let’s add a magical (for the time being) `__proto__` prefix to the attribute we are setting.

Copy and paste the following into the terminal and hit enter:

`curl -H "Content-Type: application/json" -X POST -d '{"about": {"__proto__": {"role": "admin"}}}' https://api.startup.io/users/1337 && curl -X GET https://api.startup.io/users/1337/role`

And we should get:

`{ role: "admin" }`

BOOM! We’ve successfully managed to elevate ourselves to adminhood by sending a mysterious payload `{"about": {"__proto__": {"role": "admin"}}}` to the backend.

But wait, what is this magical `__proto__` prefix and why did it work? Don’t worry, we’ll discuss that more in the next section of this lesson. But before we do that, let’s bring this whole buggy API down.

### Hacking #3: bringing down the whole API

In our previous attack, we managed to change the role attribute to whatever we wanted. But wait, aren’t JavaScript functions also stored as attributes on their respective objects? Could we possibly use the same tampering technique to override a function?

Let’s try to override one! What function is likely called by any program written in JavaScript? Well, one of the best candidates is the `toString` function! Let’s try to override the function with something meaningless, maybe a programmer dad joke?

Copy and paste the following into the terminal and hit enter:

`curl -H "Content-Type: application/json" -X POST -d '{"about": {"__proto__": {"toString": "Two bytes meet. The first byte asks: Are you ill? The second byte replies: No, just feeling a bit off."}}}' https://api.startup.io/users/1337`

And we should get as an output:

`500 Internal Server Error`

The API is down. Turns out that we can override a function!

What happened here? We will dive into the code a bit later. For now, we can deduce that we were able to override the `toString` method, just like we did before with the `role` attribute. Whenever the JavaScript runtime invokes `toString()` it expects it to be a method. But, it is no longer a method after our change (it is a dad joke now, i.e. a string literal), so the whole web server crashes, resulting in 500 errors.

### What is a prototype in JavaScript?

To understand why our attacks worked, we need to take a slight detour and explain what JavaScript prototypes are.

When we create an empty object in JavaScript (for example, `const obj = {}`), the created object already has many attributes and methods defined for it, for instance, the `toString` method. Have you ever wondered where all these attributes and methods come from? The answer is the prototype.

Many object-oriented languages, for example Java, use classes as blueprints for object creation. Each object belongs to a class, and classes are organized in parent-child hierarchies. When we call the `toString` method on an object, the language runtime will look for the `toString` method defined on the class a given object belongs to. If it cannot find such a definition, it will look for it in the parent class, then its parent class, until it hits the top of the class hierarchy.

JavaScript, instead, is a prototype-based object-oriented programming language. Each object is linked to a “prototype”. When we invoke the `toString` method on an object, JavaScript will first check to see if we explicitly defined the method for the given object. If we haven’t, it will look for its definition on the object’s prototype.

## Just a normal JavaScript object Run code

## Missing attributes come from the prototype Run code

## The shared default prototype Run code

## Setting attributes on a shared prototype Run code

### Prototype pollution explained

The bottom line is--if we modify a prototype shared by two or more objects, all objects will reflect this modification! They don’t even have to be in the same scope or otherwise related. And remember, most objects by default share the same prototype--so if we change the prototype of just one of these objects, we can change the behaviour of all of them!

What if a malicious person can change (or “pollute”) a prototype shared by multiple objects? In fact, this is what we did when we compromised startup.io’s API in the previous section. Remember, the payloads we sent to the server were:

`{"about": {"__proto__":"{"role": "admin"}}}`

`{"about": {"__proto__": {"toString": "Two bytes meet. The first byte asks: Are you ill? The second byte replies: No, just feeling a bit off."}}}`

We polluted the `role` and `toString` attributes of the default shared prototype object by sending a specially crafted HTTP POST request. To see how that attack worked, consider the code of the GET and POST HTTP request handlers:

A prototype pollution attack where a hacker sends a malicious payload to the backend server, and an unsafe merge function recursively merges that payload with a backend object

The `updateUser` method handles the HTTP POST request. The input `requestBody` is the payload we sent to the server.

### Solution: Use safe open source libraries when recursively setting object's properties

The `merge` function that startup.io wrote aimed to update one object with all attributes of another object. As we saw when we toured the code in the last section, the merge function is recursive and merrily merges all properties from its second input--even when it contains untrusted data with dubious keys such as `__proto__`.

Merging two objects is not the only functionality that can expose the code to a prototype pollution attack—any function which recursively sets nested properties can create an attack vector. Other common examples in the JavaScript ecosystem include: deep cloning (e.g. [lodash cloneDeep](https://lodash.com/docs#cloneDeep)), setting nested properties (e.g. [lodash set](https://lodash.com/docs#set)), or creating objects by recursively "zipping" properties with values (e.g. [lodash zipObjectDeep](https://lodash.com/docs#zipObjectDeep)).

Always be sure to sanitize untrusted input when recursively setting nested properties. Don’t do this yourself! Even the best developers can easily get this wrong. Instead, use a library such as [lodash](https://lodash.com/), which is extremely popular and has excellent community support and a track record of promptly fixing security issues.

A prototype pollution mitigation, where a hacker tries to send a malicious input, but a safe merge function is used, preventing the malicious input from affecting the prototype

To decide which libraries to trust, use [Snyk Advisor](https://snyk.io/advisor/?loc=learn)! Snyk Advisor provides information on a given package's popularity, community support, and security. Also, check your open source libraries with vulnerability scanners such as [Snyk](https://snyk.io/?loc=learn), which will notify you about all new vulnerabilities discovered in any libraries you are using, and will help you mitigate them easily.

FUN FACT

### It is hard to get right

Mitigating prototype pollution attacks is hard. While implementing a recursive merge function, [lodash](https://lodash.com/) developers made sure a key with the value `__proto__` would not be copied from one object to another. Unfortunately, it later turned out that prototype pollution is also possible through other properties, e.g. `constructor.prototype` ([check this fix commit](https://github.com/lodash/lodash/pull/4336/files) to learn how lodash developers dealt with that issue).

The lesson to learn here is that proper sanitization of user input is extremely hard. Whenever you can, use a battle-tested library to do the work for you.

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

### Solution: Create objects without prototypes: Object.create(null)

Another way to avoid prototype pollution is to consider using the `Object.create()` method instead of the object literal `{}` or the object constructor `new Object()` when creating new objects. This way, we can set the prototype of the created object directly via the first argument passed to `Object.create()`. If we pass `null`, the created object will not have a prototype and therefore cannot be polluted.

### Solution: Prevent any changes to the prototype: use Object.freeze()

JavaScript comes with an `Object.freeze()` method, which we can use to prevent any changes to the attributes of an object. Since the prototype is just an object, we can freeze it, too. We can freeze the default prototype by invoking `Object.freeze(Object.prototype)`, which prevents the default prototype from getting polluted.

Alternatevly you can simply install [nopp](https://www.npmjs.com/package/nopp) npm package which freezes all common object prototypes automatically.

### How do you mitigate prototype pollution?

To mitigate prototype pollution vulnerabilities in your codebase use popular open-source libraries when you need to recursively set nested properties on an object. Check which libraries to use with [Snyk Advisor](https://snyk.io/advisor/?loc=learn), and always make sure that the library you choose is free of vulnerabilities with scanners such as [Snyk](https://snyk.io/?loc=learn). To harden your code further, use `Object.create(null)` to avoid using prototypes altogether, or use `Object.freeze(Object.prototype)` to prevent any changes to the shared prototype.

### Test your knowledge!

Which of the following methods is an effective way to create an object without a prototype in JavaScript, thereby preventing prototype pollution?

[Sign up to take the quiz](https://api.snyk.io/v1/learn/auth?cta=signup&page=learn-lesson&loc=body&learn_redirect_path=%2Flesson%2Fprototype-pollution%2F&learn_redirect_fragment=%23q-bd4cdea7-d999-4a72-8aa7-6dc016b0a91d-22ba9ab1-cb40-4118-296e-9138f021abfd)

### Keep learning

To learn more about prototype pollution, check out our blog posts:

-   Read about how our security research team discovered prototype pollution in [lodash](https://snyk.io/blog/snyk-research-team-discovers-severe-prototype-pollution-security-vulnerabilities-affecting-all-versions-of-lodash/?loc=learn) and [minimist](https://snyk.io/blog/prototype-pollution-minimist/?loc=learn)
-   Check out our coverage on prototype pollution findings in [jQuery](https://snyk.io/blog/after-three-years-of-silence-a-new-jquery-prototype-pollution-vulnerability-emerges-once-again/?loc=learn) and [express](https://snyk.io/blog/prototype-pollution-in-express-fileupload/?loc=learn)

Finally, if you would like to dive deeper into prototype pollution, be sure to read [this detailed report](https://github.com/HoLyVieR/prototype-pollution-nsec18/blob/master/paper/JavaScript_prototype_pollution_attack_in_NodeJS.pdf) on prototype pollution written by Security Researcher, [Olivier Arteau](https://hackerone.com/holyvier?type=user). He has found and responsibly disclosed many prototype pollution vulnerabilities in the most common JavaScript libraries.

## Congratulations

You’ve learned what prototype pollution is and how to protect your applications from it. We hope you will apply your new knowledge wisely and make your code safer. Please rate how valuable this lesson was and provide feedback to make it better. Also, make sure to check out our lessons on other common vulnerabilities.
