---
title: "NgRx Workshop: Part 4 -Selectors"
datePublished: Thu Apr 02 2020 19:10:48 GMT+0000 (Coordinated Universal Time)
cuid: clh45xruk000009jpbc554x3g
slug: ngrx-workshop-part-4-selectors
canonical: https://dev.to/alfredoperez/ngrx-workshop-notes-selectors-3n3k
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682904546430/94d94669-375c-45c3-b471-ee1dd0f83a56.png
tags: angularjs, typescript, redux, ngrx, state-management

---


- Allow us to query our store for data
- Recompute when their inputs change
- Fully leverage memoization for performance
- Selectors are fully composable

#### Notes

* They are in charge of **transforming the data** to how the UI uses it. State in the store should be clean and easy to serialize and since selector is easy to test this is the best place to transform. Is also makes it easier to hydrate 
* They can transform to complete "View Models", there is nothing wrong about naming the model specific to the UI that they are using
* "Getter" selectors are simple selector that get data from a property on the state

```typescript
export const selectAll = (state: State) => state.collection;
export const selectActiveBookId = (state: State) => state.activeBookId;
```

* Complex selectors combine selectors and this should be created using `createSelector`. The function only gets called if any of the inputs selectors are modified

```typescript
export const selectActiveBook = createSelector(
    selectAll,
    selectActiveBookId,
    (books, activeBookId) =>
        books.find(book => book.id === activeBookId)
);
```

* Selectors can go next to the component where they are used or in the reducer file located in the `state` folder. Local selectors make it easier to test

* Global selectors are added in the `state/index`.

```typescript
export const selectBooksState = (state:State)=> state.books;
export const selectActiveBook = createSelector(
    selectBooksState,
    fromBooks.selectActiveBook
)
```

* When using the selector in the component is recommended to initialize in the constructor. If using the strict mode in TypeScript, the compiler will not be able to know that the selectors where initialized on `ngOnInit`
