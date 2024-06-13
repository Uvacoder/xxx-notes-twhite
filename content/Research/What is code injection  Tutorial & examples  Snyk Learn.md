---
created: 2023-09-10T00:03:46 (UTC -04:00)
tags: 
source: https://learn.snyk.io/lesson/prototype-pollution/
author: null
dg-publish: true
---

> [!todo] 
> Learn how to protect your applications against malicious code injection by exploiting a vulnerable web app as part of this Snyk Learn lesson.
> 



---
### What is code injection?

Code injection is a type of attack that allows an attacker to inject malicious code into an application through a user input field, which is then executed on the fly. Code injection vulnerabilities are rather rare, but when they do pop up, it is often a case where the developer has attempted to generate code dynamically. Preventing code injection attacks usually comes down to reconsidering the need to dynamically execute code, especially where user input is involved.

### About this lesson

In this lesson, you will learn how code injection works and how to protect your applications against it. We will begin by exploiting a code injection vulnerability in a simple application. Then we will analyze the vulnerable code and explore some options for remediation and prevention.

Ready to learn? Buckle your seat belts, put on your hacker's hat, and let's get started!

FUN FACT

### The godfather of vulnerabilities

Code injection has been around for about as long as computers, but its prevalence is waning. In 2008, 5.66% of all vulnerabilities reported were classified as code injection, the highest year on record. By 2015, this had decreased to 0.77%. It is even rarer today, but if your server has one, it will be "sleeping with the fish."

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

### Hacking an API endpoint

Let's set the scene. You're a 9-5 developer working for BigCorp and it's 3:30 p.m. on a Friday. The coffee is running hot but your eyes still feel heavy. One more hour and you'll be git pushing yourself all the way home for some corn chips and late-night Call of Duty.

All you need to do is make a string uppercase and greet the customer in onboarding. You've heard something about `eval()` being bad, but right now you just want to get the job done. You take a deep breath and blow on your piping hot coffee. Let's do this.

Your fingers glide across the keyboard, producing some sub-par code. It's not your best work but it works, and those corn chips are calling you. "That should do the trick", you mumble to yourself. Git add, git commit, git push, git out of here! You turn your phone off for the weekend and go hiking with friends.

Copy and paste the following into the terminal and hit enter:

`curl https://bigcorp.com/customerOnboarding?name=Edgar`

It's 9 o'clock on Saturday night and Dimitri the attacker wakes up at his desk for another night of hacking. It's a cold night, perfect black hoodie weather. Ooooh, BigCorp has made some changes to their website, what's this? A function that converts a name to uppercase? Dimitri enters his hacker alias by navigating to `curl https://bigcorp.com/customerOnboarding?name=Edgar`.

Out of habit, Dimitri injects a double quote (") to see if it triggers an error, and sure enough, it does.

Copy and paste the following into the terminal and hit enter:

`curl https://bigcorp.com/customerOnboarding?name=Edgar"`

### Injecting code into the request

Dimitri strokes his beard and raises one eyebrow, why would a double quote cause the application to throw an exception? Could it be because this is a string that is inside an `eval()` statement? To test it out, he tests a code injection payload, which he hopes will return the Node.js version being used.

Copy and paste the following into the terminal and hit enter:

`curl https://bigcorp.com/customerOnboarding?name=";process.version//`

Yes, it looks like you have achieved code injection, Dimitri, and you've also discovered that `Node.js v16.9.1` is being used on the server. Now that we have code injection, have a think about what other nefarious things we could do. Read arbitrary files, including configs? Read environment variables containing credentials? Execute commands? Delete every file on the server? The possibilities are endless.

### How does code injection work?

Let's jump into the nitty-gritty details of how this code injection works:

As an attacker, we were able to determine that our input was being injected directly into a Node.js `eval()` function.

The eval function takes a string of JavaScript code and executes it.

We escaped out of the existing string by adding `";` at the start of our payload.

Any text after the initial `";` will be interpreted as server-side JavaScript code, as an example we accessed the `process.version` variable using this payload: `";process.version//`.

The `//` at the end of the line is simply to avoid syntax errors from trailing characters.

And now we will have a look at this example in more detail by going through the server-side code.

A code injection illustration which shows a hacker sending a malicious code to an api

The name req parameter is directly stored without validation.

### Impacts of code injection

If a code injection vulnerability exists in an application, the security impact is that an attacker is able to execute arbitrary server-side code.

The ability to execute server-side code can result in a total loss of integrity, availability, and confidentiality within the application. An attacker may also abuse a code injection vulnerability to execute terminal commands on that server and pivot to adjacent systems.

### Is code injection common?

Today, code injection is not a very common vulnerability. There is rarely a situation where it would make sense to inject user input into an `eval()` function. The use of `eval()` is typically an indication of bad software design, and there is usually a better alternative.

In general, the proliferation of server-side web frameworks and libraries over the last decade has also decreased the need to custom code many tasks, reducing the likelihood that vulnerabilities will be introduced, and increasing the ease of writing sound, secure code.

Having said this, occasionally code injections can still make their way into popular libraries. One pertinent example is in the LinkedIn DustJS templating engine in 2016. If you are interested, [check out the full details of the vulnerability](https://security.snyk.io/vuln/npm:dustjs-linkedin:20160819/?loc=learn).

Despite the rarity of code injection vulnerabilities, they are included in common checks because of their critical severity.

FUN FACT

### DNA code injection

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)

### Avoid the use of dangerous functions

In JavaScript, avoid the use of `eval()`, `setTimeout()`, `setInterval()` and the `Function constructor`, especially when dealing with user input.

For example, instead of the vulnerable example from the interactive example above, which looks like this:

`uppercaseName = eval('"' + name + '"' + '.toUpperCase()')`

We could remove the `eval()` function, and simplify our code like this:

`uppercaseName = name.toUpperCase()`

Admittedly this is quite a simple example, but the point is that the vulnerability arises because dynamic data is being passed into the `eval()` function.

A code injection illustration which shows how to mitigate injection of malicious code on the server

### Reconsider the need for dynamic code execution

Perhaps the most straightforward of all prevention techniques is to reconsider the need to evaluate any dynamically generated server-side code. Usually, this dynamic code execution is the result of poor software design rather than necessity, so it's always best to consider other ways to achieve the task.

### Lock down the interpreter

If the interpreter you are using allows, lock it down by limiting the functionality to the minimum amount required by your application in the server configuration. If you are using PHP, one method is to use the `disable_functions` directive within the `php.ini` file. This will instruct the PHP server to ignore any instances of the defined functions. Of course, this will also mean that those functions can not be used at all on that server.

### Utilize a static analysis tool

Adding a static application security testing ([SAST](https://snyk.io/learn/application-security/static-application-security-testing/?loc=learn)) tool to your devops pipeline as an additional line of defence is an excellent way to catch vulnerabilities before they make it to production. There are many, but [Snyk Code](https://snyk.io/product/snyk-code/?loc=learn) our personal favorite, as it scans in real-time, provides actionable remediation advice, and is available from your favorite IDE.

### Use a security linter

Use a security linter. If you are a JavaScript developer, there's a good chance that you're already using linters to enforce the uniformity of your code, why not use one to improve security, too?

That's where [eslint-plugin-security-node](https://www.npmjs.com/package/eslint-plugin-security-node) comes in. In order to install it, run the following command:

`npm install --save-dev eslint-plugin-security-node`

Then add the following configuration to your `.eslintrc` file:

The plugin contains rules that will test your code for vulnerabilities when you run npm test. It's important to note that linters like this will not catch everything. But they are a good sanity check, and the more layers of checks we have the less likely you are to introduce vulnerabilities.

### Test your knowledge!

### Keep learning

To learn more about code injection, check out some other great content:

-   Have a look at the [OWASP code injection page](https://owasp.org/www-community/attacks/Code_Injection) that goes over some other examples of code injection.
    
-   We all love cheatsheets, to check this one [here](https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet.html), where you can read about other types of injection and how to prevent them.
    
-   If you loved the lesson, don't forget to check out our article about [command injection.](https://snyk.io/blog/command-injection/?loc=learn)
    

## Congratulations

You have taken your first step into learning what code injection is, how it works, what the impacts are, and how to protect your own applications. We hope that you will apply this knowledge to make your applications safer. We'd really appreciate it if you could take a minute to rate how valuable this lesson was for you and provide feedback to help us improve! Also, make sure to check out our lessons on other common vulnerabilities.
