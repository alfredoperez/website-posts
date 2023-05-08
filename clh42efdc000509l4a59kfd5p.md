---
title: "Testing NgRx Effect using observer-spy"
datePublished: Fri Sep 25 2020 17:23:52 GMT+0000 (Coordinated Universal Time)
cuid: clh42efdc000509l4a59kfd5p
slug: testing-ngrx-effect-using-observer-spy
canonical: https://dev.to/alfredoperez/testing-an-effect-using-observer-spy-4anj
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682880476606/a8852188-02bb-48ce-82ab-fba0533546bf.png
tags: angularjs, testing, rxjs, ngrx

---

Have you tried the [observer-spy](https://github.com/hirezio/observer-spy) library by [Shai Reznik](https://twitter.com/shai_reznik)?

%[https://github.com/hirezio/observer-spy] 

It particularly makes testing ngrx effects an easy task and keeps them readable.

To demonstrate this, I refactored the tests from `book.effects.spec.ts` from the ngrx example application, and here are the differences.

## Testing the success path

Using marbles:

```typescript
 it('should return a book.SearchComplete, with the books, on success, after the de-bounce', () => {
    const book1 = { id: '111', volumeInfo: {} } as Book;
    const book2 = { id: '222', volumeInfo: {} } as Book;
    const books = [book1, book2];
    const action = FindBookPageActions.searchBooks({ query: 'query' });
    const completion = BooksApiActions.searchSuccess({ books });

    actions$ = hot('-a---', { a: action });
    const response = cold('-a|', { a: books });
    const expected = cold('-----b', { b: completion });
    googleBooksService.searchBooks = jest.fn(() => response);

    expect(
      effects.search$({
        debounce: 30,
        scheduler: getTestScheduler(),
      })
    ).toBeObservable(expected);
  });
```

Using Observer-Spy makes it easier to arrange what we want to test, as we don't have to create a representation of the observables using characters, as demonstrated in the code below:

```typescript
 it('should return a book.SearchComplete, with the books, on success, after the de-bounce', fakeTime((flush) => {
        const book1 = { id: '111', volumeInfo: {} } as Book;
        const book2 = { id: '222', volumeInfo: {} } as Book;
        const books = [book1, book2];

        actions$ = of(FindBookPageActions.searchBooks({ query: 'query' }));
        googleBooksService.searchBooks = jest.fn(() => of(books));

        const effectSpy = subscribeSpyTo(effects.search$());

        flush();
        expect(effectSpy.getLastValue()).toEqual(
          BooksApiActions.searchSuccess({ books })
        );
      })
    );
```

## Testing when the effect does not do anything

Using marbles:

```typescript
   it(`should not do anything if the query is an empty string`, () => {
      const action = FindBookPageActions.searchBooks({ query: '' });

      actions$ = hot('-a---', { a: action });
      const expected = cold('---');

      expect(
        effects.search$({
          debounce: 30,
          scheduler: getTestScheduler(),
        })
      ).toBeObservable(expected);
    });
```

Using observer-spy, it is more readable because we are "acting" before the expectation, as opposed to when using marbles, where the "acting" part of the test is done inside the expect statement.

```typescript
 it(`should not do anything if the query is an empty string`, fakeTime((flush) => {
        actions$ = of(FindBookPageActions.searchBooks({ query: '' }));

        const effectSpy = subscribeSpyTo(effects.search$());

        flush();
        expect(effectSpy.getLastValue()).toBeUndefined();
 })
```

## Testing the error path of an effect

Using marbles:

```typescript
  it('should return a book.SearchError if the books service throws', () => {
      const action = FindBookPageActions.searchBooks({ query: 'query' });
      const completion = BooksApiActions.searchFailure({
        errorMsg: 'Unexpected Error. Try again later.',
      });
      const error = { message: 'Unexpected Error. Try again later.' };

      actions$ = hot('-a---', { a: action });
      const response = cold('-#|', {}, error);
      const expected = cold('-----b', { b: completion });
      googleBooksService.searchBooks = jest.fn(() => response);

      expect(
        effects.search$({
          debounce: 30,
          scheduler: getTestScheduler(),
        })
      ).toBeObservable(expected);
    });
```

Using observer-spy, it is clear what action should be triggered and expected.

```typescript
 it('should return a book.SearchError if the books service throws', fakeTime((flush) => {
        const error = { message: 'Unexpected Error. Try again later.' };
        actions$ = of(FindBookPageActions.searchBooks({ query: 'query' }));
        googleBooksService.searchBooks = jest.fn(() => throwError(error));

        const effectSpy = subscribeSpyTo(effects.search$());

        flush();
        expect(effectSpy.getLastValue()).toEqual(
          BooksApiActions.searchFailure({
            errorMsg: error.message,
          })
        );
      })
    );
```

You can find the working example here:

%[https://github.com/alfredoperez/ngrx-observer-spy/blob/master/projects/example-app/src/app/books/effects/book.effects.spec.ts]