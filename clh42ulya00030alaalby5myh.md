---
title: "Where to initialize components selectors in Angular?"
datePublished: Wed Sep 16 2020 13:51:39 GMT+0000 (Coordinated Universal Time)
cuid: clh42ulya00030alaalby5myh
slug: where-to-initialize-components-selectors-in-angular
canonical: https://dev.to/alfredoperez/where-to-initialize-components-selectors-in-angular-a0g
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682899192740/9c1ab5c9-9125-46c1-b5e9-a9258ac735ce.png
tags: angularjs, web-development, typescript, ngrx

---

## TL;DR

When using the selector in the component, it is recommended not to initialize them in the declaration but instead initialize them in the constructor.

```typescript
export class FindBookPageComponent {
  searchQuery$: Observable<string>;
  books$: Observable<Book[]>;
  loading$: Observable<boolean>;
  error$: Observable<string>;

  constructor(private store: Store<fromBooks.State>) {
    this.searchQuery$ = store.pipe(
      select(fromBooks.selectSearchQuery),
      take(1)
    );
    this.books$ = store.pipe(select(fromBooks.selectSearchResults));
    this.loading$ = store.pipe(select(fromBooks.selectSearchLoading));
    this.error$ = store.pipe(select(fromBooks.selectSearchError));
  }

  search(query: string) {
    this.store.dispatch(FindBookPageActions.searchBooks({ query }));
  }
}
```

## Angular Type Safety

Initializing in the constructor helps because when using the **strict mode in TypeScript**, the compiler will not be able to know that the selectors were initialized on `ngOnInit`

The strictPropertyInitialization was added by default in the `--strict` mode in Angular 9.

The following checks where also added:

```json
{
  "//": "tsconfig.json",
  "compilerOptions": {
    "noImplicitAny": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noFallthroughCasesInSwitch": true,
    "strictNullChecks": true
  }
}
```

> The strict-mode allows us to write much safe and robust application. Honestly, I think all Angular application should be written in strict-mode, but it is a little hard to understand every error messages caused by the compiler.

---

## References

%[https://medium.com/lacolaco-blog/guide-for-type-safe-angular-6e9562499d93] 

%[https://mariusschulz.com/articles/strict-property-initialization-in-typescript]