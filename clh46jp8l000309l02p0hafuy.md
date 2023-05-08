---
title: "NgRx Workshop: Part 8 -  Folder Structure"
datePublished: Fri Apr 03 2020 01:36:20 GMT+0000 (Coordinated Universal Time)
cuid: clh46jp8l000309l02p0hafuy
slug: ngrx-workshop-part-8-folder-structure
canonical: https://dev.to/alfredoperez/ngrx-workshop-notes-folder-structure-3ame
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682905559828/4fc34b55-dfa4-4dd3-adf0-b12964d73bff.png
tags: angularjs, web-development, ngrx, state-management

---

Following the LIFT principle:

* **L**ocating our code is easy
    
* **I**dentify code at a glance
    
* **F**lat file structure for as long as possible
    
* **T**ry to stay DRY - don’t repeat yourself
    

---

# Key Takeaways

* Put state in a shared place separate from features
    
* Effects, components, and actions belong to features
    
* Some effects can be shared
    
* Reducers reach into modules’ action barrels
    

---

---

# Folder structure followed in the workshop

```plaintext
├─ books\
│     actions\
│         books-api.actions.ts
│         books-page.actions.ts
│         index.ts                  // Includes creating names for the exports
│      books-api.effects.ts
│     
├─ shared\
│       state\
│          {feature}.reducer.ts     // Includes state interface, initial interface, reducers and local selectors
│          index.ts
│
```

* The index file in the *actions* folder was using action barrels like the following:
    

```typescript
import * as BooksPageActions from "./books-page.actions";
import * as BooksApiActions from "./books-api.actions";

export { BooksPageActions, BooksApiActions };
```

* This made easier and more readable at the time of importing it:
    

```plaintext
import { BooksPageActions } from "app/modules/book-collection/actions";
```

---

---

## Folder structure followed in example app from @ngrx

```plaintext
├─ books\
│     actions\
│         books-api.actions.ts
│         books-page.actions.ts
│         index.ts                  // Includes creating names for the exports
│     effects\
|         books.effects.spec.ts 
|         books.effects.ts 
|     models\
|         books.ts 
│     reducers\
|         books.reducer.spec.ts 
|         books.reducer.ts 
|         collection.reducer.ts 
|         index.ts 
│     
├─ reducers\
│        index.ts  /// Defines the root state and reducers
│
```

* The index file under the *reducers* folder is in charge of setting up the reducer and state
    

```typescript
import * as fromSearch from '@example-app/books/reducers/search.reducer';
import * as fromBooks from '@example-app/books/reducers/books.reducer';
import * as fromCollection from '@example-app/books/reducers/collection.reducer';
import * as fromRoot from '@example-app/reducers';

export const booksFeatureKey = 'books';

export interface BooksState {
  [fromSearch.searchFeatureKey]: fromSearch.State;
  [fromBooks.booksFeatureKey]: fromBooks.State;
  [fromCollection.collectionFeatureKey]: fromCollection.State;
}

export interface State extends fromRoot.State {
  [booksFeatureKey]: BooksState;
}

/** Provide reducer in AoT-compilation happy way */
export function reducers(state: BooksState | undefined, action: Action) {
  return combineReducers({
    [fromSearch.searchFeatureKey]: fromSearch.reducer,
    [fromBooks.booksFeatureKey]: fromBooks.reducer,
    [fromCollection.collectionFeatureKey]: fromCollection.reducer,
  })(state, action);
}
```

* The index file under `app/reducers/index.ts` defines the meta-reducers, root state, and reducers
    

```typescript
/**
 * Our state is composed of a map of action reducer functions.
 * These reducer functions are called with each dispatched action
 * and the current or initial state and return a new immutable state.
 */
export const ROOT_REDUCERS = new InjectionToken<
  ActionReducerMap<State, Action>
>('Root reducers token', {
  factory: () => ({
    [fromLayout.layoutFeatureKey]: fromLayout.reducer,
    router: fromRouter.routerReducer,
  }),
});
```

Personally, I like how the `example-app` is organized. One of the things that I will add is to have all the folders related to ngrx in a single folder:

```plaintext
├─ books\
│     store\
│        actions\
│            books-api.actions.ts
│            books-page.actions.ts
│            index.ts                  // Includes creating names for the exports
│        effects\
|            books.effects.spec.ts 
|            books.effects.ts 
|        models\
|            books.ts 
│        reducers\
|            books.reducer.spec.ts 
|            books.reducer.ts 
|            collection.reducer.ts 
|            index.ts 
│     
├─ reducers\
│        index.ts  /// Defines the root state and reducers
│
```