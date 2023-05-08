---
title: "NgRx Workshop: Part 2 - Actions"
datePublished: Thu Apr 02 2020 19:03:29 GMT+0000 (Coordinated Universal Time)
cuid: clh438fs7000009jvggom5b8t
slug: ngrx-workshop-part-2-actions
canonical: https://dev.to/alfredoperez/my-notes-from-ngrx-workshop-from-ngconf-2020-part-2-actions-1ilh
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682899928763/3b0e902f-66cd-48a2-92e7-655382382e48.png
tags: angularjs, web-development, ngrx, state-management

---

---

* Unified interface to describe events
    
* Just data, no functionality
    
* has at least a type property
    
* strongly typed using classes and enums
    

#### Notes

There are a few rules for [writing good actions](https://ngrx.io/guide/store/actions#writing-actions) within your application.

* **Upfront** - write actions before developing features to understand and gain a shared knowledge of the feature being implemented.
    
* **Divide** - categorize actions based on the event source.
    
* **Many** - actions are inexpensive to write, so the more actions you write, the better you express flows in your application.
    
* **Event-Driven** - capture *events* **not** *commands* as you are separating the description of an event and the handling of that event.
    
* **Descriptive** - provide context that are targeted to a unique event with more detailed information you can use to aid in debugging with the developer tools.
    

* Actions can be created with `props` or fat arrows
    

```typescript
// With props
export const updateBook = createAction(
    '[Books Page] Update a book',
    props<{
        book: BookRequiredProps,
        bookId: string
    }>()
);

// With fat arrow
export const getAuthStatusSuccess = createAction(
    "[Auth/API] Get Auth Status Success",
    (user: UserModel | null) => ({user})
);
```

#### Event Storming

You can use sticky notes as a group to identify:

* All of the events in the system
    
* The commands that cause the event to arise
    
* The actor in the system that invokes the command
    
* The data models attached to each event
    

#### Naming Actions

* The **category** of the action is captured within the square brackets. `[]`
    
* It is recommended to use the present or past tense to **describe the event that occurred** and stick with it.
    

***Example***

* When referring to components, you can use the present tense because they are related to events. It is like in HTML where events do not use past tense. Eg. `OnClick` or `click` is not `OnClicked` or `clicked`
    

```typescript
export const createBook = createAction(
    '[Books Page] Create a book',
    props<{book: BookRequiredProps}>()
);

export const selectBook = createAction(
    '[Books Page] Select a book',
    props<{bookId: string}>()
);
```

* When the actions are related to API you can use past tense because they are used to describe an action that happened
    

```typescript
export const bookUpdated = createAction(
    '[Books API] Book Updated Success',
    props<{book: BookModel}>()
);

export const bookDeleted = createAction(
   '[Books API] Book Deleted Success',
   props<{bookId: string}>()
);
```

#### Folders and File structure

It is a good practice to have the actions define close to the feature that uses them.

```typescript
├─ books\
│     actions\
│         books-api.actions.ts
│         books-page.actions.ts
│         index.ts
```

The index file can be used to define the names for the actions exported, but it can be completely avoided

```typescript
import * as BooksPageActions from "./books-page.actions";
import * as BooksApiActions from "./books-api.actions";

export { BooksPageActions, BooksApiActions };
```