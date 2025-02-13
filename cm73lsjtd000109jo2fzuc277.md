---
title: "Skip Angular Resource"
seoTitle: "Use TanStack Query instead of Angular Resource"
seoDescription: "TanStack Query excels in Angular apps with powerful server state management and advanced features compared to Angular Resource"
datePublished: Thu Feb 13 2025 17:18:08 GMT+0000 (Coordinated Universal Time)
cuid: cm73lsjtd000109jo2fzuc277
slug: skip-angular-resource
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738528695297/01839d2b-3a53-4957-8a93-9c3921fd87ba.jpeg
tags: angularjs, web-development, typescript, state-management, tanstack-query

---

I understand. You're building an Angular app and have heard about the new [Resource API](https://angular.dev/guide/signals/resource). Maybe you're considering using it, or you already are. Here's the thing: while Angular Resource and TanStack Query weren't designed to do exactly the same things, the Resource API offers a new way to make server requests. I confidently recommend starting with TanStack Query because it includes the features of the Resource API plus server state management, which you often implement with NgRx entities. TanStack Query provides more features by default than both, with an easier API.

Let's talk about Angular Resource for a second. It's Angular's experimental take on handling async data, and sure, it's got some neat features. You can load data through a clean API that plays nice with Angular's router, and it gives you basic tools to handle loading states. Nothing wrong with that, and here you can see an example of how you will use it:

But then there's [TanStack Query](https://tanstack.com/query/v5/docs/framework/angular/installation). Born in the React world as React Query and battle-tested across thousands of applications, it's now available for Angular as @tanstack/angular-query. Think of it as Resource API's older, more experienced sibling who started in React, mastered server state management, and brought all those hard-earned lessons to Angular. It doesn't just fetch data - it's got your back with smart caching, updating “out of date” data in the background, deduping multiple requests for the same data, and a ton of features that you didn't even know you needed until you're knee-deep in production issues. Here is an example of how it looks

![Example of TanStack Query](https://cdn.hashnode.com/res/hashnode/image/upload/v1738513973792/652c6529-1eee-44a8-a158-e1da1e144142.jpeg align="center")

As you might notice from the code examples above, the way you use Resource vs. Query is really similar—both need a key/id, both handle the fetching of data, and both give you access to the loaded value. That's exactly why I'm making the case that you should skip using Resource and go directly to TanStack. It provides a maintainable and scalable solution for server state management without the boilerplate code that you'd need if you wanted to implement all the features it offers.

And you might be asking what other features it provides. Let's focus on the similarities we just saw in the code.

## What is similar between TanStack Query and Angular Resource?

Let's break down the main areas where these tools overlap. Hopefully after seeing these similarities, you will understand why I'm advocating for TanStack Query from the start if you are already thinking of refactoring your code and want to get server state management in the process

### Request States

An outstanding new feature of Angular Resource over the existing HTTP Client is that now it provides a request status. This is really useful in the template code because it can be used to present other components depending on the state.

Both tools help you handle those loading states, but they do it a bit differently. With Resource, you get something like this:

![Example of handling requests](https://cdn.hashnode.com/res/hashnode/image/upload/v1738514450636/1a33c45a-82bb-43b0-80b0-24a9bc67e098.jpeg align="center")

Pretty similar, right? But here's where Query steps it up—it gives you an extra `fetchStatus` property that tells you if you're fetching in the background while showing stale data. Super handy for those smooth UI updates!

### Aborting Requests

The next big thing about Angular Resource is that you can [abort requests](https://angular.dev/guide/signals/resource#aborting-requests) by just providing a signal to it. This query cancellation feature is also available in [TanStack Query](https://tanstack.com/query/latest/docs/framework/angular/guides/query-cancellation) and it works in a similar way; take a look at how it can be implemented using both:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738517445947/646ab59a-9792-4283-8a50-7a8d64361aca.jpeg align="center")

But TanStack Query goes the extra mile - you can also ***manually cancel queries whenever you want***. Pretty neat when you need that extra control!

![Cancelling all requests with TanStack Query](https://cdn.hashnode.com/res/hashnode/image/upload/v1738517403123/7f024839-c464-4727-b05d-aeeb6f7019f5.jpeg align="center")

### Deduping Requests

Here's something cool - both tools make sure you're not accidentally fetching the same data multiple times. Resource does it by ignoring results from old requests if a new one comes in.

Query takes it a step further—it sees that you're asking for the same data (thanks to the `queryKey` )and just ***reuses the existing request*** and this works because, as mentioned previously, it keeps the state of the request so if another component is trying to get the same data, instead of doing multiple requests, it will provide what was stored into the state. One request, many components happy!

### Reloading Data

Need fresh data? Both tools got you covered; with Angular Resource, you can call `reload` like `userResource.reload();` and with TanStack Query you can use `refetch` like in `userQuery.refetch();`

But here's where Query really shines - it can automatically refetch data when:

* The user comes back to your app
    
* The window regains focus
    
* You tell it the data is stale
    
* And more!
    

Yes, you read right! TanStack query keeps track of the components (observers) that have requested data and whenever the user comes back to the window, it will automatically refresh all the data. You don't have to write any of that logic yourself. It just works.

# Advanced Features from TanStack Query

Let's explore some of the powerful features that make TanStack Query stand out. These capabilities come built-in, whereas with Resource you would need to implement them manually.

## Pagination

Pagination is a common requirement in modern applications, particularly when dealing with tables or lists. TanStack Query provides built-in features that enhance user experience by prefetching, using placeholder data and stale time.

Let's begin by talking about `staleTime`. Stale time is a property that you can set when using a query, and it indicates how long you consider your data to be fresh. This is really helpful in cases where you want to avoid making API requests and reuse the data already in the state. For example, if the stale time is greater than 5 seconds and the user is constantly moving from page to page, the query will reuse the data it has in the state instead of making a new request.

Here's how you can implement pagination with these features:

![Pagination with TanStack Query](https://cdn.hashnode.com/res/hashnode/image/upload/v1738522844667/d51c1080-8f92-4fdd-b630-ef247ff26332.jpeg align="center")

This implementation leverages three key TanStack Query features:

1. **Stale Time**: By setting `staleTime: 5000`, we tell TanStack Query to consider the data fresh for 5 seconds, reducing unnecessary API calls when users navigate between pages quickly.
    
2. **Placeholder Data**: Using `placeholderData: (previousData) => previousData`, we show the previous page's data while loading the new page, providing a smoother user experience instead of showing loading indicators.
    
3. **Prefetching**: The `prefetchNextPage` function checks if we're currently showing placeholder data and if there are more pages available. If both conditions are met, it automatically prefetches the next page, ensuring the data is ready when the user navigates forward.
    

## Offline Support

TanStack Query provides robust offline support through two key features: network modes and optimistic updates.

One of TanStack Query's standout features is its built-in network handling capabilities. When initializing your QueryClient, you can specify how your application should behave when offline:

Network modes determine how queries behave when the connection is unstable:

* `'offlineFirst'`: Attempts to fetch data but falls back to cache if offline
    
* `'online'`: Only fetches when the network is available
    
* `'always'`: Attempts to fetch regardless of network status
    

Beyond network handling, TanStack Query excels at optimistic updates - showing changes immediately while handling server synchronization in the background. Here's a practical example:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738524771741/eb11724c-8ec1-4925-97e3-761222a3ad96.jpeg align="center")

This combination of network modes and optimistic updates enables applications to:

* Continue functioning when offline
    
* Provide immediate feedback to users
    
* Queue mutations for when connectivity returns
    
* Automatically sync with the server when back online
    
* Handle errors gracefully with automatic rollbacks
    

## Devtools

And here's the cherry on top - TanStack Query comes with amazing dev tools. The devtools help you debug and inspect your queries and mutations. You can enable the devtools by adding `withDevtools` to `provideTanStackQuery` and you will get the dev panel that shows you:

* All your queries and their states
    
* What data is cached
    
* When background refreshes happen
    
* Network requests in real-time
    
* Number of observers per query
    
* Action buttons to trigger states and queries
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738524259480/bd0afe92-469d-4b21-919a-41d8689460d3.png align="center")

## The Bottom Line

When evaluating server state management solutions for Angular applications, the comparison between Angular's Resource API and TanStack Query reveals a clear distinction in capabilities and scope. While Resource API offers a basic approach to handling asynchronous resources through Promises and simple state management, TanStack Query delivers a comprehensive solution specifically designed for modern application needs.

TanStack Query effectively combines the simplicity of Resource API with the power of more complex state management libraries like NgRx Entity. It accomplishes this while reducing boilerplate code and providing features that would otherwise require significant custom implementation:

* Built-in state management with sophisticated caching
    
* Automatic background refetching mechanisms
    
* Robust offline support and optimistic updates
    
* Data prefetching and persistence capabilities
    
* Comprehensive developer tools for debugging and optimization
    

If your application already uses Resource and meets all your requirements, migrating might not be immediately necessary. However, if you're starting a new project, planning a refactor, or finding yourself implementing custom solutions for caching, offline support, or state management, TanStack Query presents a compelling alternative that can simplify your architecture while providing a more robust solution.

The key advantage of adopting TanStack Query lies in its ability to handle complex server state management scenarios without the overhead of traditional state management libraries. This leads to cleaner code, better performance, and a more maintainable application structure in the long run.