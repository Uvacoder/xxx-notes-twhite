---
dg-publish: true
---

For the longest time I was stuck on how XSS actually worked; so you write some JavaScript in my own browser and it somehow ends up going to other people? I am writing scripts on *my* machine, not theirs so how does that work?

Today during a lab from [TCM Security](https://academy.tcm-sec.com/p/the-all-access-pass) we wrote a little query param and sent that data as a `POST` request and it clicked: I am sending a POST request to server-side because that is literally what a `POST` request does. If that vulnerability isn't fixed, it could do some wild stuff to other folks visiting that website.

