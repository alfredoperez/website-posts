---
title: "TanStack Query in Angular"
seoTitle: "TanStack Query Integration Guide for Angular"
seoDescription: "Explore how TanStack Query enhances Angular apps by efficiently managing server state with minimal code, reducing boilerplate significantly"
datePublished: Mon May 06 2024 13:50:55 GMT+0000 (Coordinated Universal Time)
cuid: clvv0rz6x000309l4dn5whmct
slug: tanstack-query-in-angular
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715002363680/4e0dd37e-c507-4222-8737-acc27c0ddfec.jpeg
tags: javascript, angularjs, web-development, state-management, tanstack-query

---

[TanStack Query](https://tanstack.com/query/latest) [is a powerful](https://tanstack.com/query/latest) data synchronization library that enables efficient handling of server state in web applications. It’s widely used in React applications, but lately it has been making noise in Angular since it is a library that can provide a lot of functionality out of the box to handle all the server state of our applications with a few lines of code and, with that, reduce the amount of boilerplate code.

## **Server State vs Client State**

To fully appreciate TanStack's benefits, it is useful to have a basic understanding of the two types of states—server state and client state—that we must control while developing the UI.

### Client State

This state is related to the state that is used in order to know what the user has been interacting with. For this state, I see that there are three levels: component, feature, and global state.

* **Component State**: This is the most basic of the states, and it is used within one or multiple components that work together to achieve a specific goal. It is agnostic of the domain or context. This state helps to communicate with child or sibling components, and one example of this is the state of a data table where the pagination component and search input need to keep their states to communicate what is currently being displayed.
    
* **Feature State:** This is the state that is shared among a group of container or presentational components in the same feature and helps to share state-related information that belongs to a specific domain or feature. An example of this is a multi-step form for an entity where we need to keep track of the current step the user is in, the selected item, or even the data needed for the form fields.
    
* **Global State:** This is a state that is shared in all the applications and can be things like current user information and which features are enabled for them.
    

[![Image showing different types of client state. Original image from Dribbble](https://miro.medium.com/v2/resize:fit:700/1*h5-kL-9dVHcCNcSAwlOUIA.png align="left")](https://dribbble.com/shots/19908225-Task-Management-Dashboard)

### Server State

This is all the async data that comes from the API requests, GraphQL, or any kind of server. This state is usually kept in order to have a better UI since we have a single place where we can keep track of all the data that we get from the server.

From all these states that we need to keep track of, the server state tends to be the biggest one we need to keep track of since it might be needed for each entity, and for the component and feature states, you can use different strategies like services or container components before having to need a full-fledged state system like a component or signal store.

---

> [*TanStack Query*](https://tanstack.com/query/latest/docs/framework/angular/guides/does-this-replace-client-state) [***replaces the bo***](https://tanstack.com/query/latest/docs/framework/angular/guides/does-this-replace-client-state)***ilerplate code*** *and related wiring used to manage cache data in your client-state and replaces it with just a few lines of code.*

In the followin[g image, you ca](https://tanstack.com/query/latest/docs/framework/angular/guides/does-this-replace-client-state)n see how the request is done using TanStack Query and that will return us a signal that has the state of the request, which we will explore in more detail further down in the article.

![](https://miro.medium.com/v2/resize:fit:700/1*dsjoBZsWzgduRmk4greTQA.png align="left")

TanStack Query provides tools for fetching, caching, synchronizing, and updating asynchronous data without having to manage the complexities of updating UIs, caching data, and managing errors or loading states manually.

Here is a list of all the features that TanStack Query offers from the official website:

[![List of all the features by TanStack as described in the official website](https://miro.medium.com/v2/resize:fit:700/1*4Q9U41MMRu7Hh8Vqn9b-Tg.png align="left")](https://tanstack.com/query/v4)

but ENOUGH!… Let's see it in action 👀

In the following section, I will show you how you can install and use it while highlighting some of the features it offers that barely scratch the surface of the whole set of things you can do with it.

# TanStack Query in Angular

There are currently two libraries for Angular that have implemented TansStack Query, one by [ngneat](https://github.com/ngneat/query) that can be used with observables and signals, and the other by [TanStack](https://tanstack.com/query/latest/docs/framework/angular/overview) that is in the experimental phase and only works with signals. Both are good, but I will show you how to use the one by the TanStack team.

%[https://github.com/ngneat/query] 

%[https://github.com/TanStack/query/tree/main/packages/angular-query-experimental?source=post_page-----5520dab44882--------------------------------] 

# **Getting started**

To install the library and the developer tools, we will use the following commands:

![Commands used to install TanStack Query and its Dev Tools](https://miro.medium.com/v2/resize:fit:700/1*SuTC4IFz5lg1gcGTSKCleg.png align="left")

Once you have it installed, you will need to add the provider to the application and developer tools to the main component

![Code snippet showing how to add the Query Client to the application](https://miro.medium.com/v2/resize:fit:700/1*YgXZhuO28a-iwoGJxmdUpQ.png align="left")

The developer tools are added to the main component, and they will show up as a floating button that enables and disables the developer tools.

![Code snippet showing how to add the Developer tools](https://miro.medium.com/v2/resize:fit:700/1*aOEEsoyOPi-FzWbL-p6StQ.png align="left")

![](https://miro.medium.com/v2/resize:fit:700/1*w1FzXwLef3bwOepzHPWYxw.png align="left")

To make a request, you have to construct a query that requires a `queryKey` that should be serializable and unique to the query’s data since it is the main thing that TanStack Query uses to cache the data. Also, it requires a query function to be used to make the API request and the returned data will be linked to the query key.

![](https://miro.medium.com/v2/resize:fit:700/1*CqvduUEnaLS3XeXd2R0SRg.png align="left")

![](https://miro.medium.com/v2/resize:fit:700/1*skDSavounq9u0yFayfX2gA.png align="left")

Once we have this, the `usersQuery` will have the state of the request as a signal, like `usersQuery.isPending()` or,`usersQuery.isError()` and the data will also be able to be accessed at the [`usersQuery.data`](http://usersQuery.data)`()` signal.

![](https://miro.medium.com/v2/resize:fit:700/1*PPvulG2YhzuZ1lS--0fIuQ.png align="left")

The developer tools are composed of three sections. In the left menu section, it displays all the queries and mutations that have been executed, and you also have some tooling to filter, sort and order them. At the top, you can see the state that they are in, and there are also some controls to remove all the data or even simulate offline mode.

![](https://miro.medium.com/v2/resize:fit:700/1*ITLw3pB40RvuHzbA0Ey4HQ.png align="left")

![](https://miro.medium.com/v2/resize:fit:700/1*9JNNmPFOqqwPbB5ajUIksA.png align="left")

Once you select a query, you will see in the top right section query details that go from the query that was used to its current state and how many observers are linked to this query. This is especially useful since every observer will be notified of any changes if the query data changes.

Also, you will see a set of controls to troubleshoot or see how the UI behaves by triggering different states for the query.

![](https://miro.medium.com/v2/resize:fit:700/1*jGjCQC8N7a3TzYvRhURAWA.png align="left")

![](https://miro.medium.com/v2/resize:fit:700/1*G0ZpNFWICYMliFkb1hRsMw.png align="left")

Finally, at the bottom part, you will find an explorer for the data and a query that will help a lot while debugging what data was used or is currently cached.

![](https://miro.medium.com/v2/resize:fit:700/1*fgMj2Az7ejhcLIQ8Yy7lKw.png align="left")

## Mutations

Mutations are the way to update the data and can be used when updating, deleting, or creating new items. It requires a mutation key and function as well, but it can also take which query keys to invalidate, meaning that once the mutation is done, any observer that has a query with the same key will automatically re-fetch data.

![](https://miro.medium.com/v2/resize:fit:700/1*LAVeTPSKdGLa3_zWqflsjA.png align="left")

![](https://miro.medium.com/v2/resize:fit:700/1*v0JCmqwv5XnqPPOYkClDSg.png align="left")

In the following GIF, you can see how the data grid in the background is automatically updated after performing the mutation, and this is because it was specified which query key to invalidate, and the data grid has an observer for that key.

![](https://miro.medium.com/v2/resize:fit:700/0*E7OJYZLhmSIgugep.gif align="left")

But this is not all! TanStack Query offers offline support, which means that all the mutations will be executed once it comes back online and it will also refresh all the observers for the related queries.

![](https://miro.medium.com/v2/resize:fit:700/1*1xYJtfSrIXIsJW_GGTpqlQ.png align="left")

In the following GIF, you can see a demo of this, where, by using the developer tools offered by TanStack query, we can simulate offline mode and try to add a new user once we go back online. On the right side, you can see that the requests were executed, and now the data grid shows the updated list of users.

![](https://miro.medium.com/v2/resize:fit:700/0*XHcC33rHCCC9HKLb.gif align="left")

---

# Conclusion

TanStack Query is a library that can be used to handle all the server-state and thanks to it, you will get a lot of benefits out-of-the-box without having to implement custom solutions or write a lot of boilerplate code.

Once you start using it, you will realize that there is not a lot of state to handle, and for the left-over, you might only need a light-weight and extensible solution like a [Service with Signals](https://medium.com/ngconf/keeping-state-with-a-service-using-signals-bee652158ecf) or the [Signal Store](https://ngrx.io/guide/signals/signal-store/entity-management).

![](https://miro.medium.com/v2/resize:fit:700/1*Dw775vIVpMS52FMTWacjSg.png align="left")

Stay tuned, since I am working on follow-up articles exploring how to integrate it with the [API service pattern](https://medium.com/ngconf/taming-http-requests-with-an-abstract-api-service-61e73f39ed75) [I previously talked](https://medium.com/ngconf/taming-http-requests-with-an-abstract-api-service-61e73f39ed75) about and other more advanced features like using local storage to create a bullet-proof offline solution.

---

For more articles and tips for better DevX and CX with Angular, Cypress, and Web Development, you can find me at:

[![https://twitter.com/alfrodo_perez](https://miro.medium.com/v2/resize:fit:428/1*l88tq9KAmIjL5UGaUGeoJA.png align="left")](https://twitter.com/alfrodo_perez)

[![https://www.linkedin.com/in/alfredo-perez/](https://miro.medium.com/v2/resize:fit:457/1*GfFMCNpLGiCPm2CTzjZWvQ.png align="left")](https://www.linkedin.com/in/alfredo-perez/)