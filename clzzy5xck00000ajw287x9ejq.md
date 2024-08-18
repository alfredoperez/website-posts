---
title: "Improve loading times with the @defer block in Angular"
seoTitle: "Enhance Angular loading speed using @defer block"
seoDescription: "Optimize Angular app with `@defer` for deferred content, enhancing LCP and seamless data loading via idle, viewport, and interaction triggers"
datePublished: Thu Dec 07 2023 06:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzzy5xck00000ajw287x9ejq
slug: improve-loading-times-with-the-defer-block-in-angular
canonical: https://medium.com/ngconf/improve-loading-times-with-the-defer-block-in-angular-0bfc52823a54
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702053689140/44453672-cff8-4f7f-a5c7-99820616a0aa.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1702050293603/cd6fde09-59e0-4ccd-9002-da86a08904a7.jpeg
tags: web-development, angular, webdev, typescript, ui, frontend-development

---

The `defer` block is a feature in Angular's deferrable views that allows you to defer the loading of certain content or large components until they are needed. It serves as a container for the deferred content and provides various triggers to control when and how the content is loaded.

With the `defer` block, you can optimize the initial loading time of your application by deferring the loading of non-critical or less frequently accessed components, modules, or dependencies. This can help improve the performance and speed up the initial rendering of your page, and even get better Core Web Vitals results, specifically improving metrics such as Largest Contentful Paint (LCP) and Time to First Contentful Paint by deferring when things are loaded.

## Blocks

There are three available blocks that are part of the `@defer` block and you can use these to manage the different stages of the loading process:

`@placeholder`: This block is rendered while the deferred content is being loaded. It serves as a temporary placeholder for the deferred content.

⭐ *Use this block to* ***display loaders or placeholders*** *to indicate that the content is loading* ⭐

`@loading`: This block is rendered when the deferred content is being loaded.

⭐ *Use this  to* ***show customized loading indicators*** *or messages* ⭐

`@error`: This block is rendered if there is an error loading the deferred content.

⭐ Use this to handle and **display error messages** or fallback content ⭐

---

## Triggers

On the other hand, you will be able to use the following triggers within the defer blocks:

`on idle`: This trigger condition is met when the browser state becomes idle. It is commonly used to defer loading until the browser is not busy with other tasks.

`on viewport`: This trigger condition is met when the deferred content comes into the viewport, meaning it becomes visible to the user. It is useful for lazy loading content that is below the fold or not immediately visible.

`on interaction`: This trigger condition is met when the user interacts with the placeholder or the surrounding elements. It is useful when you want to load the deferred content upon user interaction, such as clicking or hovering.

`on hover`: This trigger condition is met when the user hovers over the placeholder or the surrounding elements. It is useful for lazy loading content that should be loaded when the user hovers over it.

`on immediate`: This trigger condition is met immediately, without any delay. It is useful for situations where you want to load the deferred content immediately without any waiting.

`on timer`: This trigger condition is met after a specified delay. It allows you to set a timer and load the deferred content after the specified time.

---

Now let's move on to the good part and see an example of how we can use blocks and triggers together to improve app performance.

## Improving the initial load

%[https://snappify.com/view/23ccb8d2-9a20-47e8-8bc8-8c5a4c1240d9] 

This first example shows how to improve the initial load time of an infinite-loading application (such as Facebook or Instagram) by rendering just the components that are visible in the viewport.

In order to avoid loading all of the images and data from each subcomponent, we are defining the defer block to load only if the component is within the viewport. Additionally, we are setting it to prefetch anytime it is idle.

Since we are delaying component loading and only loading what the user needs, these two configurations will help us enhance the Largest Content Paint (LCP) results from the Core Web Vitals. 

Additionally, we are utilizing the `@loading` and `@placeholder` subblocks to ensure that the data loading process is as seamless as possible. You can utilize the`@placeholder` sub-block to prevent screen flickering when loading the actual components by providing a component of similar dimensions to the one that will be rendered at the end. The`@loading` sub-block can be used to add another component, like a skeleton or a shimmering box, to represent the loading status.

Here in the GIF below, you can see how the`OnInit` lifecycle for the post component gets called after scrolling down, and the component gets into the viewport.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700654715754/907c1b2e-aa1d-4a0e-8389-e74513d24a08.gif align="center")

## Improving the load of sub-components

%[https://snappify.com/view/88b2948e-a9c8-48d3-855f-7b77044d5df8] 

In your applications, you might encounter cases where a component contains child components that require additional data or images. In the example above, you can see how using the `@defer` block will make this more efficient.

The first thing that we have is the`@defer` block that is set up to only load whenever the section with the name of `#commentsSection` is on the viewport and also when we have comments in that post. these will make sure that we don't start loading data for components that are not actually visible or that we don't actually need to be loading data.

The sub-blocks that we use here are similar to the first example, but in this case, the `@loading` sub-block has some configuration to only show whenever loading the component is taking longer than 100 ms and we're gonna show it for a minimum of 500 ms. This will help to avoid not flickering or showing components too fast that can confuse the user.

the last sub-block that we're using here is the `@error` and this can be shown in cases that you wanna present a specific component when there was an error retrieving data.

In the GIF below you can see the the `Comments` Component gets render also when scrolling down, but it is also only applying to the posts that have comments.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700655046509/3872df45-b88f-43c7-9db8-a29af321a777.gif align="center")

## TL;DR;

In this article, we discuss Angular's `@defer` block feature, which allows you to optimize your application's initial loading time by deferring the loading of certain content or components until needed. We explore the three available blocks - `@placeholder`, `@loading`, and `@error` - and the various triggers, such as on idle, on viewport, and on interaction.

In the examples or use cases you can see how defer blocks can be used to improve app performance, specifically by enhancing Largest Contentful Paint (LCP) results from Core Web Vitals and providing a seamless data loading process.