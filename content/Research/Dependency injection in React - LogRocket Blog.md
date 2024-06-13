---
created: 2023-09-09T23:59:10 (UTC -04:00)
tags: 
source: https://blog.logrocket.com/dependency-injection-react/
author: Simohamed Marhraoui
dg-publish: true
---



> [!summary] 
> Discover the primary reason to use dependency injection in React and follow along with this library-free guide to using it. 


---
Dependency injection (DI) is a pattern where components necessary for your code to run are hot-swappable. This means that your dependencies are not hard-coded in your implementation and can change as your environment changes.

[Enabled by inheritance, DI is a well-used pattern in Object-Oriented Programming](https://blog.logrocket.com/roll-your-own-dependency-injection/) (OOP) intended to make the code reusable across different objects and classes. The primary reason to use dependency injection in React, however, would be to mock and test React components easily. Unlike in [Angular](https://blog.logrocket.com/how-dependency-injection-works-in-angular/), DI is not a requirement while working with React, but rather a handy tool to use when you want to clean things up.

## Dependency injection in JavaScript

To illustrate the principles of DI, imagine an npm module that exposes the following `ping` function:

```jsx
export const ping = (url) => {
  return new Promise((res) => {
    fetch(url)
      .then(() => res(true))
      .catch(() => res(false))
  })
}
```

Using the `ping` function in a modern browser would work just fine.

```jsx
import { ping } from "./ping"

ping("https://logrocket.com").then((status) => {
  console.log(status ? "site is up" : "site is down")
})
```

But running this code inside Node.js would throw an error because `fetch` is not implemented in Node.js. However, there are many `fetch` implementations and polyfills for Node.js that we can use.

DI allows us to turn `fetch` into an injectable dependency of `ping`, like so:

```jsx
export const ping = (url, fetch = window.fetch) => {
  return new Promise((res) => {
    fetch(url)
      .then(() => res(true))
      .catch(() => res(false))
  })
}
```

We are not required to give `fetch` a default value of `window.fetch`, but not requiring us to include it every time we use `ping` makes for a better development experience.

Now, in a Node environment, we can use `[node-fetch](https://www.npmjs.com/package/node-fetch)` in conjunction with our `ping` function, like so:

```jsx
import fetch from "node-fetch"
import { ping } from "./ping"

ping("https://logrocket.com", fetch).then((status) => {
  console.log(status ? "site is up" : "site is down")
})
```

## Working with multiple dependencies

If we have multiple dependencies, it wouldn’t be feasible to keep adding them as parameters: `func(param, dep1, dep2, dep3,…)`. Instead, a better option is to have an object for dependencies:

```jsx
const ping = (url, deps) => {
  const { fetch, log } = { fetch: window.fetch, log: console.log, ...deps }

  log("ping")

  return new Promise((res) => {
    fetch(url)
      .then(() => res(true))
      .catch(() => res(false))
  })
}

ping("https://logrocket.com", {
  log(str) {
    console.log("logging: " + str)
  }
})
```

Our parameter `deps` will be spread into an implementation object and will override the functions that it provides. By destructuring from this modified object, the surviving properties will be used as dependencies.

Using this pattern, we can choose to override one dependency but not the others.

## Dependency injection in React

While working with React, we make heavy use of custom hooks to fetch data, track user behavior, and perform complex calculations. Needless to say that we do not wish to (nor can we) run these hooks on all environments.

Tracking a page visit during testing will corrupt our analytics data, and fetching data from a real backend would translate to slow-running tests.

Testing is not the only such environment. Platforms like [Storybook](https://storybook.js.org/) streamline documentation and can do without using many of our hooks and business logic.

### Dependency injection via props

Take the following component, for example:

```jsx
import { useTrack } from '~/hooks'

function Save() {
  const { track } = useTrack()

  const handleClick = () => {
    console.log("saving...")
    track("saved")
  }

  return <button onClick={handleClick}>Save</button>
}
```

As mentioned before, running `useTrack` (and by extension, `track`) is something to avoid. Therefore, we will convert `useTrack` into a dependency of the `Save` component via props:

```jsx
import { useTracker as _useTrack } from '~/hooks'

function Save({ useTrack = _useTrack }) {
  const { track } = useTrack()

  
}
```

By aliasing our `useTracker` to avoid name collision and using it as a default value of a prop, we preserve the hook in our app and have the ability to override it whenever the need arises.

The name `_useTracker` is one naming convention out of many: `useTrackImpl`, `useTrackImplementation`, and `useTrackDI` are all widely used conventions when trying to avoid collision.

Inside Storybook, we can override the hook as such, using a mocked implementation.

```jsx
import Save from "./Save"

export default {
  component: Save,
  title: "Save"
}

const Template = (args) => <Save {...args} />
export const Default = Template.bind({})

Default.args = {
  useTrack() {
    return { track() {} }
  }
}
```

#### Using TypeScript

When working with TypeScript, it is useful to let other developers know that a dependency injection prop is just that, and use the exact `typeof` implementation to retain type safety:

```
function App({ useTrack = _useTrack }: Props) {
  
}

interface Props {
  
  useTrack?: typeof _useTrack
}
```

### Dependency injection via the Context API

Working with the Context API makes dependency injection feel like a first-class citizen of React. Having the ability to redefine the context in which our hooks are run at any level of the component comes in handy when switching environments.

Many well-known libraries provide mocked implementations of their providers for testing purposes. React Router v5 has `[MemoryRouter](https://v5.reactrouter.com/web/api/MemoryRouter)`, while Apollo Client provides a `[MockedProvider](https://www.apollographql.com/docs/react/development-testing/testing/#the-mockedprovider-component)`. But, if we employ a DI-powered approach, such mocked providers are not necessary.

[React Query](https://react-query.tanstack.com/) is a prime example of this. We can use the same provider in both development and testing and feed it to different clients within each environment.

In development, we can use a bare `queryClient` with all the default options intact.

```jsx
import { QueryClient, QueryClientProvider } from "react-query"
import { useUserQuery } from "~/api"

const queryClient = new QueryClient()

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <User />
    </QueryClientProvider>
  )
}

function User() {
  const { data } = useUserQuery()
  return <p>{JSON.stringify(data)}</p>
}
```

But when testing our code, features like retries, re-fetch on window focus, and cache time can all be adjusted accordingly.

```jsx

import { QueryClient, QueryClientProvider } from "react-query"

const queryClient = new QueryClient({
  queries: {
    retry: false,
    cacheTime: Number.POSITIVE_INFINITY
  }
})


export const decorators = [
  (Story) => {
    return (
      <QueryClientProvider client={queryClient}>
        <Story />
      </QueryClientProvider>
    )
  },
]
```

Dependency injection in React is not exclusive to hooks, but also JSX, JSON, and anything that we wish to abstract away or change under different circumstances.

### Alternatives to dependency injection

Depending on the context, dependency injection might not be the right tool for the job. Data-fetching hooks, for instance, are better mocked using an interceptor (like [MSW](https://mswjs.io/)) instead of injecting hooks all over your test code, and outright [mocking functions](https://jestjs.io/docs/mock-functions) remains an advanced and cumbersome tool for bigger problems.

## Why should you use dependency injection?

**Reasons to use DI:**

-   No overhead in development, testing, or production
-   Extremely easy to implement
-   Does not require a mocking/stubbing library because it’s native to JavaScript
-   Works for all your stubbing needs, such as components, classes, and regular functions

**Reasons to not use DI:**

-   Clutters your imports and components’ props/API
-   Might be confusing to other developers

## Conclusion

In this article, we took a look at a library-free guide to dependency injection in JavaScript and make the case for its use in React for testing and documentation. We used Storybook to illustrate our use of DI, and finally, reflected on reasons why you should and shouldn’t use DI in your code.

## Get set up with LogRocket's modern React error tracking in minutes:

1.  Visit [https://logrocket.com/signup/](https://lp.logrocket.com/blg/react-signup-general) to get an app ID.
2.  Install LogRocket via NPM or script tag. `LogRocket.init()` must be called client-side, not server-side.

-   [NPM](https://blog.logrocket.com/dependency-injection-react/#tab1)
-   [Script Tag](https://blog.logrocket.com/dependency-injection-react/#tab2)

```sh
$ npm i --save logrocket import LogRocket from 'logrocket'; LogRocket.init('app/id');
```

Add to your HTML:
```jsx
<script src="https://cdn.lr-ingest.com/LogRocket.min.js"></script><script>window.LogRocket && window.LogRocket.init('app/id');</script>
```

4.  (Optional) Install plugins for deeper integrations with your stack:
    -   Redux middleware
    -   ngrx middleware
    -   Vuex plugin

[Get started now](https://lp.logrocket.com/blg/react-signup-general)
