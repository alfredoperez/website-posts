---
title: "Taming HTTP Requests with an Abstract API Service"
seoDescription: "Abstract API Service streamlines HTTP requests, offering ease, standardization, and versatility for various entities and API types. A universal manage..."
datePublished: Thu Jan 04 2024 06:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cltvkhgnv00070ajy6o9adpsm
slug: taming-http-requests-with-an-abstract-api-service
canonical: https://medium.com/ngconf/taming-http-requests-with-an-abstract-api-service-61e73f39ed75
tags: angularjs, web-development, typescript, repository-pattern, domain-driven-design

---

---

![](https://cdn-images-1.medium.com/max/2400/1*62YHWMCnhrSzYhIN91yYLQ.jpeg align="left")

Some time ago, there was a need to have multiple remote controllers in your living room because you had multiple devices on it, like the TV or stereo system, and so on, making it a hassle to manage all of them and to find the correct one when you needed it. As a solution for this, the universal control was invented with the purpose of having a single control for all your devices, and you could even configure it to handle multiple TVs.

Well, today I’m going to talk about how to create an **abstract API service** that can be used as the main service for all your HTTP requests.

### How is it built?

Let’s start by creating an abstract class that also takes a generic, where the generic is the related entity that you want to handle. In the constructor, we’re going to pass the entity name because it is needed to build a URL where we’re going to be making the HTTP requests.

![](https://cdn-images-1.medium.com/max/1600/0*NlQfWdBWjJbzry3f.png align="left")

Let’s go deep into some of the methods that we need for this abstract API service. The first one is `getList` where we're going to retrieve a list of entities. Note that we're returning a `ListResponse` object, where we can define different properties that we want to provide to all our components or consumers related to the list of entities that we're getting from the HTTP response, but let's put a pin in it since I will show more code about it later on.

![](https://cdn-images-1.medium.com/max/1600/0*r8ZsV-TuqB7T9rd6.png align="left")

Next is the method to get an entity ID. We also have requests for all the CRUD operations, like the create, update and delete methods; but the interesting thing is that for all of these, we will be using the same method (`request`) to handle the HTTP requests.

![](https://cdn-images-1.medium.com/max/1600/0*DMxh-ZJTCsWirW49.png align="left")

Now, if we go inside the `request` method we can see that here is where we can create the URL, which headers and which parameters we need to send for the requests. This is really helpful since we can add all the custom implementation details from our APIs in one place.

![](https://cdn-images-1.medium.com/max/1600/0*z_0T_IaoxPq_fD8l.png align="left")

For instance, in the `getOptions` method we have some logic to detect that we are trying to request a list of items with pagination and add the appropriate parameters that our API expects.

![](https://cdn-images-1.medium.com/max/1600/0*iIP8XrGMSH7kNyUx.png align="left")

Furthermore, we can manipulate how we send the response back to the components, services or any other kind of consumer. Here `mapListResponse` is used to have a standard output where we are reading from the headers to get the total count. Also, we are computing if it has more pages and sending back the pagination data that was sent to retrieve the data. This extra data is useful in components like the paginator component to enable or disable the next button and to make a request for the next page.

![](https://cdn-images-1.medium.com/max/1600/0*YrMQ2Mt-AFuIlv3G.png align="left")

Thanks to this, we have a standardized contract where we have inputs like the request options, search and page object to get a specific page, but at the same time, we have standardized outputs like the response for list requests.

### How do we use the Abstract API Service?

So, you might be asking yourself, How do we use it? And this is really simple. The only thing that we need to do after this, is create a new service that is going to extend this API service, and we need to pass the entity that we’re going to be using it with. Also, in that constructor, we need to pass how the entity is called on the server since it will be used while creating the URL for the requests.

![](https://cdn-images-1.medium.com/max/1600/0*v3BH_lwCY17cIwGm.png align="left")

### Is it possible to have other kinds of requests?

Okay! So it is easy to re-use, but what about the cases where you have related endpoints that have a different endpoint structure than your common CRUD or that return another entity than the one for the service you are working on?

You can still enhance this service with a method for this specific request. The only thing that I recommend is that you use the `request` methods so you can still benefit from not having to setup all the options needed from your API and to specify the type that you're going to return in the request.

The following example demonstrate this by having an API request for Heroes inside the `UsersApiService`

![](https://cdn-images-1.medium.com/max/1600/0*suGuxOZ7pYoeJNQ7.png align="left")

### What about requesting subentities or related entities?

Sometimes you might need to return a specific entity that is related to another entity. For example, let’s say that you want to return the tasks related to the users `user/1/tasks` . In this case, you can opt for either adding the method to the TasksApiService or the UsersApiService. The only caveat is that you need to specify that you're going to be returning tasks instead of users.

![](https://cdn-images-1.medium.com/max/1600/0*FudcFIq-hCJ4reYb.png align="left")

### Benefits

1. **Ease of Use:** Once set up and aligned with API specifications, creating services for various entities becomes effortless.
    
2. **Standardized Behavior:** Centralization allows for the incorporation of loggers, SLIs, or other features related to server requests, ensuring standardized behavior.
    
3. **Flexibility:** The abstract service remains flexible, accommodating different API types and calls based on project requirements.
    

### Is it different from just using the `HttpClient`?

The key distinction lies in tailoring the service specifically for your API. By extending a service, it becomes knowledgeable about constructing URLs, required headers, and specific output objects, offering a more customized approach.

---

So there you have it. One service to rule them all. This is now your universal remote controller for all your HTTP requests.

![](https://cdn-images-1.medium.com/max/1600/1*K8H_xbjCs7ULM6cbE6aCMw.png align="left")