---
created: 2023-09-10T00:00:41 (UTC -04:00)
tags: []
source: https://stackoverflow.com/questions/61797060/dependency-injection-with-react-hooks
author: |-
  SteeviePSteevieP
  20522 silver badges55 bronze badges
dg-publish: true
---

> [!tldr] 
> The biggest benefit of dependency injection I have found is being able to replace the implementation of a service throughout my entire app in one line at the composition root.
> 



---
The biggest benefit of dependency injection I have found is being able to replace the implementation of a service throughout my entire app in one line at the composition root.

Is there a way of doing this using React hooks?

It seems that when using a hook, eg useHook(), you're binding tightly to a specific implementation, and it's a manual process to find and switch out all the implementations, which is further complicated by useHooks() appearing at arbitrary points in the component.

My current solution is to use a React Context to make the composition root availiable to everything as required, which seems to be working well, but with many touting Hooks as a DI framework I am wondering if I've missed something.
