---
title: "View Transitions API in Angular 17"
seoDescription: "Discover Angular 17's View Transitions API for smooth animations, better UX, and increased app performance. Upgrade web apps with ease"
datePublished: Mon Nov 20 2023 18:35:20 GMT+0000 (Coordinated Universal Time)
cuid: clp78ympg00020al468aj2gt1
slug: view-transitions-api-in-angular-17
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700663936008/add2f2f3-ed0e-4562-ba11-b093f4bff38a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1700505269820/b0b0a29f-7ba2-4637-866b-4a3bdc77e2e7.png
tags: web-design, web-development, angular, web-animation, angular17

---

![](https://miro.medium.com/v2/resize:fit:770/1*le0L2YwsyNDeYgKNrfxyhA.png align="left")

🚀 Version 17 of Angular is just around the corner, and it will include something that is making waves in the web development world: support for the [View Transitions API.](https://developer.chrome.com/docs/web-platform/view-transitions/)

The View Transitions API makes it super easy to add cool animations and smooth transitions when you’re building web apps. You can create eye-catching effects with just a bit of code, which makes your app more engaging for users.

Plus, it’s not just about making things look fancy 🎩. This API helps users understand how different parts of your app connect, and it can even make things seem faster when data is loading. So, it’s not just about making your app look good; it’s about making it work better for your users too. 💪

%[https://developer.chrome.com/docs/web-platform/view-transitions/] 

## Enable View Transitions in the routes

To enable it, you just need to take one step! just one thing. You just need to add `withViewTransitions` to the `` `provideRouter` `` in your `ApplicationConfig`:

```typescript
export const appConfig: ApplicationConfig = {
  providers: [
    provideAnimations(),
    provideRouter(
        routes, 
        withViewTransitions(),     // 👈
        withComponentInputBinding()
   )]
};
```

Then you can setup route animations as you normally do (or follow this [guide](https://angular.io/guide/route-animations#enable-routing-transition-animation)) but if you look closely, you will notice that the pseudo element for View Transitions is added to the DOM when the animation happens:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696251362417/d74fc543-a30f-4f99-99b5-a0b72ebe98e5.png align="center")

Also, if you go to Animations in Chrome Developer Tools, you can now debug the View Transitions API:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696251365312/b1c2cad1-a346-4c94-9887-d695b11ab57c.gif align="center")

## **Using transitions to show the user flow**

Now, let's do something where we guide the user through a transition when navigating from a parent to a child page.

In the following example, you see how the book cover transitions to its new location, letting it know that we are entering the detail page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696252833674/a6bc632e-0990-47e0-b9b8-3f51d404d4d7.gif align="center")

I was inspired to make this example from a video of [Midudev](https://twitter.com/midudev?lang=en) where he shows how Astro 3.0 adds support for the View Transitions API, but honestly, I think using this new API in Angular is way easier since you only have to add it to the router and it knows what to do. You can watch the original video here.

[https://www.youtube.com/watch?v=ODv-VFRFGcY](https://www.youtube.com/watch?v=ODv-VFRFGcY)

To add this functionality in Angular, we only need to add the same name in the `view-transition-name` for each book in the two pages that are using it.

To do this, I mapped the id to a new property:

```typescript
books.map(book books.map (
 {
    ...book,
     viewTransitionName: `view-transition-name: book-${book.id}`
 }
));
```

Then you add it to the image element on both pages:

```xml
<picture class="mb-8 w-full relative">
   <img
      class="aspect-[389/500]rounded"
      [ngSrc]="book.img"
      width="389"
      height="500"
      [style]="book.style"
   />
</picture>
```

## Summary

In this article, we discuss the upcoming support for the View Transitions API in Angular version 17, which allows developers to easily add animations and smooth transitions in web apps.

You can add it in a matter of seconds with a couple of code lines, and immediately you will be able to have an elegant web application.

You can play with the code here: [https://github.com/alfredoperez/angular-17-demos](https://github.com/alfredoperez/angular-17-demos)