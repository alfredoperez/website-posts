---
title: "NgRx Workshop: Part 1- Introduction Introduction"
seoTitle: "NGRX Workshop: Introductory Guide & Notes"
seoDescription: "Optimize NGRX Workshop: Apply SHARI principle, prevent over-engineering, and discover alternative state management solutions"
datePublished: Thu Apr 02 2020 19:00:54 GMT+0000 (Coordinated Universal Time)
cuid: clgrf5js9000a09l7a0j05l8p
slug: ngrx-workshop-part-1-introduction
canonical: https://dev.to/alfredoperez/my-notes-from-ngrx-workshop-from-ngconf-2020-part-1-introduction-h8l
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682898049987/e785acc8-17c5-4463-9c6b-300f2754e2dd.png
tags: angularjs, web-development, ngrx, state-management

---

These are the comprehensive notes I diligently compiled while participating in the insightful NgRx workshop, which took place during the renowned ng-conf event. This workshop was specifically designed to provide an in-depth understanding of the NgRx framework, its best practices, and the latest updates in the Angular community.

---

# When to use it?

This question was asked several times, and basically, you can use the **SHARI** principle to find out if NgRx or Redux is needed for your application or even a specific component inside your app.

%[https://twitter.com/MikeRyanDev/status/988074549983023104] 

From the [docs](https://ngrx.io/docs#when-should-i-use-ngrx-for-state-management) we get the following:

> `@ngrx/store` (or Redux in general) provides us with a lot of great features and can be used in a lot of use cases. But sometimes this pattern can be an overkill. Implementing it means we get the downside of using Redux (a lot of extra code and complexity) without benefiting of the upsides (predictable state container and unidirectional data flow).

> The NgRx core team has come up with a principle called **SHARI**, that can be used as a rule of thumb on data that needs to be added to the store.

> * **Shared**: State that is shared between many components and services
>     
> * **Hydrated**: State that needs to be persisted and hydrated across page reloads
>     
> * **Available**: State that needs to be available when re-entering routes
>     
> * **Retrieved**: State that needs to be retrieved with a side effect, e.g. an HTTP request
>     
> * **Impacted**: State that is impacted by other components
>     

> Try not to over-engineer your state management layer. Data is often fetched via XHR requests or is being sent over a WebSocket, and therefore is handled on the server side. Always ask yourself **when** and **why** to put some data in a client side store and keep alternatives in mind. For example, use routes to reflect applied filters on a list or use a `BehaviorSubject` in a service if you need to store some simple data, such as settings. Mike Ryan gave a very good talk on this topic: [You might not need NgRx](https://youtu.be/omnwu_etHTY)

Also, the following question was asked:

> Once you add ngrx to an application, is it a bad practice to use a service with BehaviorSubject to communicate between components? Example. Having a modal with a form that uses a stepper. Should the modal use NGRX because the whole app is using ngrx.

The response from Mike was:

> **No, you do not have to use ngrx for all your states.**

> We have this term called SHARI principle and essentially the only state that goes into the ngrx store is a truly global state, state that is shared between your components, state that needs to be rehydrated when the application boots up or is impacted by actions of other features.

> If the state doesn't match that rubric, then **it is totally fine to use a simple state storage mechanism** for it as a service with a Behavior Subject.

Another related comment by Alex:

> One of the things I remember myself when I started using ngrx, I tried to **push anything** ngrx mostly because I wanted the dev tools to the time travel and I think some of the features that are exciting and helpful can misguide you. The moment of realization for me is not to push everything possible into it, at that time we were learning, as the pattern was evolving, now is kind of known what are the best practices.

> Some of the things I ***don't put in the store*** are **Form Controls** (since it is already a reactive state), **the local state within components** (even if it interacts with the network), some of the lifecycle state for a component. Basically, **the first thing I asked myself is how shareable is the state, how much does the application needs to know about this.**

Here is a link to a re-usable service that uses BehaviorSubject and can be used for simpler communication cases

{% link https://dev.to/alfredoperez/angular-service-to-handle-state-using-behaviorsubject-4818 %}

Here is a library by Dan Wahlin that can help for simple state management:

{% github DanWahlin/Observable-Store%}

## Resources

* [Reducing the Boilerplate with NgRx](https://www.youtube.com/watch?v=t3jx0EC-Y3c) by Mike Ryan and Brandon Roberts
    
* [Do we really need @ngrx/store](https://blog.strongbrew.io/do-we-really-need-redux/) by Brecht Billiet
    
* [Simple State Management with RxJS’s scan operator](https://juristr.com/blog/2018/10/simple-state-management-with-scan/) by Juri Strumpflohner
    
* [You might not need NgRx](https://www.youtube.com/watch?v=omnwu_etHTY) by Myke Ryan
    
* [Mastering the Subject: Communication Options in RxJS](https://www.youtube.com/watch?v=_q-HL9YX_pk) by Dan Wahlin