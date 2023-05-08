---
title: "NgRx Workshop: Part 7 - Meta-Reducers"
datePublished: Fri Apr 03 2020 01:12:22 GMT+0000 (Coordinated Universal Time)
cuid: clh46gznn000309jpboyadc3q
slug: ngrx-workshop-part-7-meta-reducers
canonical: https://dev.to/alfredoperez/ngrx-workshop-notes-meta-reducers-4b36
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682905436736/48afa0be-c909-4fd8-8599-09507abaad83.png
tags: angularjs, web-development, ngrx, state-management

---


* Intercept actions before they are reduced
* Intercept state before it  is emitted
* Can change the control flow of the Store

![Alt Text](https://cdn.hashnode.com/res/hashnode/image/upload/v1682899621621/1ac1f27a-eaf6-4dea-ac99-b03f8b288273.gif)

## Most common use cases

* Reset state when a signout action occurs
* for debugging creating logger
* to rehydrate when the application starts up

-It is like a plugin system for the store, they behave similarly to the **interceptors**

## Example 
An example of this can be to use it in a logger

```typescript
const logger = (reducer: ActionReducer<any, any>) => (state: any, action: Action) => {
    console.log('Previous State', state);
    console.log('Action', action);

    const nextState = reducer(state, action);

    console.log('Next State', nextState);
    return nextState;
};

export const metaReducers: MetaReducer<State>[] = [logger];
```