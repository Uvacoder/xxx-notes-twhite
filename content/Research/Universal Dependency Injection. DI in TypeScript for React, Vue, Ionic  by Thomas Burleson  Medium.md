---
created: 2023-09-09T23:59:40 (UTC -04:00)
tags: []
source: https://thomasburlesonia.medium.com/https-medium-com-thomasburlesonia-universal-dependency-injection-86a8c0881cbc
author: Thomas Burleson
dg-publish: true
---

> [!summary] 
> A lightweight TypeScript dependency injection solution (DI) useful for React, Ionic, and more.
> 



---
[

![Thomas Burleson](https://miro.medium.com/v2/resize:fill:88:88/1*M5Wlvf-rguXBUbWpOtXY1A.png)



](https://thomasburlesonia.medium.com/?source=post_page-----86a8c0881cbc--------------------------------)

Dependency Injection (**DI**) is a system process found in many software frameworks: [Spring](https://www.journaldev.com/2394/java-dependency-injection-design-pattern-example-tutorial), [Flex](https://www.adobe.com/devnet/flex/articles/dependency_injection.html), [Swiz](https://swizframework.jira.com/wiki/spaces/SWIZ/pages/1998872/Dependency+Injection), [Angular](https://angular.io/guide/dependency-injection).

Dependency injection is a solution in which a system supplies a _target_ 1..n _dependencies_ from external sources; rather than requiring the _target_ to create those dependencies itself. Dependencies are services, objects, functions, or values that a class (or factory, function) needs to perform its function.

Most developers think DI is only useful for testing with mocks, spys, and stubs. The true value of DI, however, is its ability to decouple **_origination_** (configuration, construction, and caching**)** from **_destination_** usages.

## Traditional Scenario

Consider the following coupling of components:

![](https://miro.medium.com/v2/resize:fit:1400/0*iUIQobZyB-NbqLkT.png)

When we say **injected**, we mean **used by**! The best way to provide injected instances is via the constructor (or function arguments). So when we say _injected_, we mean “_passed in as constructor arguments”_.

![](https://miro.medium.com/v2/resize:fit:1400/1*3oqCFIQWzSlnzxDHEXohig.png)

ContactsFacade could be implemented in two ways:

![](https://miro.medium.com/v2/resize:fit:1400/1*PysuvGGYw3Q1h81LdC_Glw.png)

Worst implementation: locked implementation

![](https://miro.medium.com/v2/resize:fit:1400/1*zpwyHB9LBbirFZ-Dv5bjwQ.png)

Best Implementation: Supports DI

Without DI systems, developers interested in using an instance of `ContactsFacade` must 1st prepare instances of `ContactsService` and `ContactsStore`. Those instances are passed as construction arguments to ContactsFacade.

![](https://miro.medium.com/v2/resize:fit:1400/0*uMEG3CgUO1t3X2vZ.png)

After construction, developers interested in _sharing the same instance_ of a service or data model must consider how those instances will be cached, shared, and accessed.

This manual process should be very familiar… and is not required when a proper DI system is available.

## Dependency Injection in React

1.  React provides a Context as a way to pass data through the component tree without having to pass props down manually at every level. This allows deep child components to easily lookup services (using `Context.Consumer` )… services that were previously provided higher in the DOM tree (using `Context.Provider`).

> IMO, such constructs ^ also clutter the HTML markup and the DOM tree.

2.`useContext()` is another approach that provides programmatic lookups to services registered higher **up** the view hierarchy.

Lee Warrick recently wrote an article that dicusses “[Context API’s Dirty Little Secret](https://dev.to/leewarrickjr/the-problem-with-react-s-context-api-3dn0)”. This article emphasis why the Context should not be used.

![](https://miro.medium.com/v2/resize:fit:1400/1*kbJnxR6WRcYlmA0iABPp6A.png)

Context use can trigger many re-renders!

3\. Seasoned developers may even consider Higher-Order Functions (HoF) to encapsulate configuration information (or services) that will be needed for future (deep-tree) use.

Unfortunately, none of these provide a true _dependency injection_ infrastructure nor address the true intent of DI.

1.  These approaches do not address the **goals of automating construction** and **decoupling dependencies**.
2.  These solutions do not address the issues of **how to test with mocks**, etc.

A **true** dependency injection (DI) system easily solves these issues!

## Requirements for Universal Dependency Injection

A robust dependency injection (DI) system allows provides the following features to developers.

![](https://miro.medium.com/v2/resize:fit:1400/1*8jY0gbk6EJiUF_LdmZCBLg.png)

Q**uestion**: “How can we build a framework-agnostic DI system that can be used in React or raw JavaScript/TypeScript applications?”

A more universal solution will require the following features:

![](https://miro.medium.com/v2/resize:fit:1400/1*jzWDTZ5ptJwvyCawLDL6Mw.png)

The DI solution implemented here does **NOT use** reflection, decorators, nor any other more advanced feature. The DependencyInjector is a lightweight, stand-alone solution that can work with any **ES6/TypeScript** framework or application.

## Exploring the DI Flow

Dependency injection uses a lookup process to determine HOW to create an object. The key aspect of the _lookup_ is the **Token**.

![](https://miro.medium.com/v2/resize:fit:1400/1*DuTI2URx5smBhNkBzt9L9A.png)

Using the concept of `**Provider**` featured and [documented in Angular](https://angular.io/guide/architecture-services), we can implement a platform-independent approach:

![](https://miro.medium.com/v2/resize:fit:1400/0*KRWtmqJ4CBP7yODD.png)

The **Token** ( `<token>`) is the key to the DI process: it is simply an _identifier_ object used to request and lookup the cached object instance.

If the cached singleton object is not available, then the `<token>` is used to scan a list of Provider registrations. In such cases, the `<token>` is used as a lookup for the associated **_factory_ methods** that will be used to create that instance.

In most cases, the `<token>` is the actual Class. Sometimes, however, a class reference is not possible. The `<token>` could be either a

-   string
-   Class
-   InjectionToken

> Developers should use an `InjectionToken` whenever the type you are injecting is not reified (does not have a runtime representation); such as when injecting an interface, callable type, array or parameterized type.

Developers should note the `deps:any[]` options that allow each factory to define subsequent dependencies that are needed for proper construction. (_soon we will see how this is used_)

## Custom Configuring DI (for your needs):

If we consider \[again\] the ContactsFacade dependency tree:

![](https://miro.medium.com/v2/resize:fit:1400/0*iUIQobZyB-NbqLkT.png)

This construction process seems trivial… yet it is a real world example and requires a tree-like instantiation process.

So how can one easily configure construction-relationships for programmatic DI? We want a DI system that:

-   can easily be used at any view level
-   supports singleton instances
-   supports override (non-singleton) instances
-   supports multiple DI providers

We can easily build a custom `injector` instance using the `makeInjector()` factory. Below, we registered a set (1..n) of Contact `Provider` configurations to create a custom Contact _injector_.

![](https://miro.medium.com/v2/resize:fit:1400/0*Eh3cUl1ZGH1JNo2J.png)

The best part of the custom injector is that its on-demand (lazy) feature. When a developer uses `injector.get(<token>)`, the cache registry is first checked to see if the instance exists. If not, only then will the required factories be called to create and cache the instance.

## DI with React Components

Using our custom injectors in the React View layer is trivial: we import and then use the `injector.get(<token>)` syntax!

![](https://miro.medium.com/v2/resize:fit:1400/0*8RxFA4TCQT7YnNEq.png)

From the perspective of the ContactsList view component ^, the view is not concerned with the ‘**how**’, ‘**when**’, or ‘**where**’ the ContactsService was constructed or cached.

Instead the view component is only interested in the **USE** of the ContactsService API.

## Using `useInjectorHook()`

Our use of the DI features in the View layer can be simplified even more if we build a custom Injection hook using the `useInjectorHook()` API (_see line 11_):

![](https://miro.medium.com/v2/resize:fit:1400/1*S07nQz971o_9_xgP89p_yg.png)

Now — within the View layer— the DI code is super simple (_see line 14_):

![](https://miro.medium.com/v2/resize:fit:1400/1*eTzfEeMKVB-kU3qLTsjUKg.png)

## Easy Testing with DI + Mocks

Consider an additional requirement to test the view component in isolation. This means testing will need to inject a fake or mock service ContactsService into the ContactsList view component.

With DI, it is super easy to replace a ‘real’ provider with a ‘mock’ provider:

![](https://miro.medium.com/v2/resize:fit:1400/0*gTBVdOYG815cZ7eK.png)

With this replacement on lines 10–13 above, the existing Provider configurations (defined in `./contacts`) will be overridden.

Future requests for an injected ContactsService instance will then deliver only a **mock** ContactsService.

## Considerations

The astute reader will realize that both of the following are required to build custom injectors:

-   `makeInjector()`, and
-   `import {injector} from ‘….’`

These two (2) manual steps are required since a DI system has **not been architected within React** like it is in Angular!

Fortunately, this DI solution that can be used in **any** view component, at any view-depth, or within any (non-view) service! The manual part is a small investment of effort that yields **huge ROI** with true dependency injection.

## Using the Library

Install the DI features into your project using:

`npm install @mindspace-io/utils --save`

To configure your own custom injector, just import the DI features:

`import { makeInjector, DependendecyInjector, InjectionToken } from ‘@mindspace-io/utils';`

> Don’t forget to build a custom hook to make DI lookups super easy!
