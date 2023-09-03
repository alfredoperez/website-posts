---
title: "TIL: nuggets from ng-conf 2023"
seoTitle: "TIL: ng-conf 2023 Key Nuggets"
datePublished: Sat Jul 22 2023 13:22:41 GMT+0000 (Coordinated Universal Time)
cuid: clke1ii0n000b09mn1w7e8cgb
slug: til-nuggets-from-ng-conf-2023
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693752684123/ba6b485f-d712-4926-a5ec-1f8fbf38954f.jpeg
tags: ng-conf, angular, ngrx

---

Here are some of the best shiny new things and innovations from ng-conf 2023, where Angular enthusiasts came together to share their know-how and discuss the framework's future. In this article, I cover key highlights like deferred loading, built-in control flow, binding route information to component inputs, required inputs, directive composition, and more. Join me as we delve into these exciting advancements and learn how they can boost your Angular projects.

## RFC: Deferred Loading

> The modern web emphasizes delivering a strong user experience during the loading of an application. Metrics like Core Web Vitals quantify this experience, highlighting the performance of the initial load.
> 
> One standard and effective technique for optimizing this loading experience is to defer lower-priority parts of the UI and focus resources on loading the most critical parts of the page. For example, a page that shows a main video and a list of comments may be optimized to load the video first and defer loading the code to render the comments until the video is fully buffered and ready to play.
> 
> Angular supports the lazy loading of parts of the application via the Router (lazy routes). Lazy loading of individual components is achievable via dynamic `import()` and `ngComponentOutlet`, but this approach can be complex and error-prone.
> 
> As a result, we see a lot of potential in introducing an ergonomic, holistic approach to deferred loading in the core framework, that works across both client- and server-side rendering. This RFC proposes a new framework primitive: `{#defer}`.

```xml
{#defer on viewport}
  <calendar/>
{:loading}
   <loading/>
{:error}
   Error while loading
{/#defer}
```

Links:

* [https://github.com/angular/angular/discussions/50716](https://github.com/angular/angular/discussions/50716)
    

---

# RFC: Built-In Control Flow

> This RFC proposes a new control flow syntax for Angular, and represents a significant change to how we approach control flow in the framework.

Many templating languages in the open source space have control flow primitives, and we’re fortunate to be able to draw on those ideas and designs from the larger web community. The proposed new control flow syntax is heavily inspired by Svelte’s control flow as well as the `mustache` templating language.

The new control flow syntax is introduced in the form of *blocks*, a new syntactic structure in templates.

**Conditional** control flow uses blocks with the keywords `if` and `else`:

```typescript
{#if cond.expr}
  Main case was true!
{:else if other.expr}
  Extra case was true!
{:else}
  False case!
{/if}
```

**Switch** control flow uses blocks with the keywords `switch`, `case`, and `default`"."

```typescript
{#switch cond.kind}
  {:case x}
    X case
  {:case y}
    Y case
  {:case z}
    Z case
  {:default}
    No case matched
{/switch}
```

Angular’s `switch` does not have fallthrough (there is no `break` operation required).

**Loop** control flow uses blocks with the keywords `for` and `empty`:

```typescript
{#for item of items; track item.id}
  {{ item }}
{:empty}
  There were no items in the list.
{/for}
```

Source:

* [https://github.com/angular/angular/issues/47737](https://github.com/angular/angular/issues/47737)
    

---

# **Bind Route Info to Component Inputs**

> In Angular v16 we will get a new feature that will simplify the process of retrieving route information in the component and make it way easier.
> 
> We will be able to pass the route information to the component inputs, so we don’t need to inject the **ActivatedRoute** service anymore.

```typescript
const routes: Routes = [
  {
    path: "search/:id",
    component: SearchComponent,
    data: { title: "Search" },
    resolve: { searchData: SearchDataResolver }
  },
];


@Component({})
export class SearchComponent implements OnInit {
    // 👇 this will come from the query params
    @Input() query?: string; 

    // 👇 this will come from the path params
    @Input() id?: string; 
    
    // 👇 this will come from the data
    @Input() title?: string; 

    // 👇 this will come from the resolved data
    @Input() searchData?: any; 
   
     ngOnInit() {
        // do something with the query
        // do something with the id
        // do something with the title
        // do something with the searchData
    }
}
```

Source:

* [https://itnext.io/bind-route-info-to-component-inputs-new-router-feature-1d747e559dc4](https://itnext.io/bind-route-info-to-component-inputs-new-router-feature-1d747e559dc4)
    

---

## Required Inputs

> By using this new feature, we can ensure that all necessary data is present before the component or directive logic is executed, resulting in better code quality, fewer errors, and an overall more efficient development process. To use it we can set the new `required` option in our `input`:

```typescript
@Component({
  selector: 'app-foo',
  standalone: true,
  templateUrl: './foo.component.html',
})
export class FooComponent {
           👇
  @Input({ required: true }) elementId: string;
}
```

[https://netbasal.com/from-good-to-great-required-inputs-in-angular-47374c8a43cf](https://netbasal.com/from-good-to-great-required-inputs-in-angular-47374c8a43cf)

---

## Directive Composition

In version 15, Angular introduced a new directive composition API, enabling developers to combine existing directives into more complex ones or components. This facilitates the encapsulation of behaviors into smaller directives and their subsequent reuse throughout the application.

```typescript
@Directive({
  selector: "button[sfeirButton]"
  hostDirectives: [
    { directive: MatTooltip,
      inputs: [
        'matTooltip: tooltipText',
        'matTooltipPosition': tooltipPosition
      ],
      outputs: []
    }
  ]
})
export class SfeirButton
```

Also, you can use it within components:  
Adding host directives to a component

> Similarly to composing a directive out of other directives, we can apply the same approach to adding behavior to components using `hostDirectives` API. This way, we could, for example, create a more specialized component or just apply the behavior of the directive to a whole host element:

```ts
@Component({
  selector: 'app-highlight-and-border',
  standalone: true,
  template: `
    <p>My first component with border and highlight</p>
  `,
  styles: [
    `
    :host {
      cursor: pointer;
      display: block;
      padding: 16px;
      width: 500px;
    }
  `,
  ],
  hostDirectives: [HighlightDirective, BorderDirective],
})
export class HighlightAndBorderComponent {}
```

**Source**:

* [https://www.telerik.com/blogs/use-powerful-directive-composition-api-angular-15-kendo-ui](https://www.telerik.com/blogs/use-powerful-directive-composition-api-angular-15-kendo-ui)
    
* [https://www.thisdot.co/blog/introduction-to-directives-composition-api-in-angular/](https://www.thisdot.co/blog/introduction-to-directives-composition-api-in-angular/)
    
* [https://angular.io/guide/directive-composition-api](https://angular.io/guide/directive-composition-api)
    

---

## Angular Language Service: Automatic Import Improvements

This will bring a lot of functionality to Visual Studio Code, but most of this has been available in WebStorm for a while now.

* ✅ *(15.0.0)* **Automatic Quickfix Imports**: Suggest imports as a quick fix for unknown standalone components.
    
* ✅ *(15.0.0)* **NgModule support**: Support importing to and from NgModules, in addition to standalone components.
    
* ✅ *(15.1.0)* **Multi-suggestions**: If more than one directive has this selector, give the user a choice.
    
* ✅ *(15.2.0)* **Pipes**: Support pipes as well as directives.
    
* 🚧 *(15.2.0)* **Declaration Files**: Suggest directives from .d.ts files, including imports with `@` specifiers.
    

Source:

* [https://github.com/angular/angular/issues/47737](https://github.com/angular/angular/issues/47737)
    

---

## Ag-grid new features

### **Cell Data Types and Automatic Type Inference**

> AG Grid 30 adds built-in [cell data types](https://www.ag-grid.com/javascript-data-grid/cell-data-types/?ref=blog.ag-grid.com#top) with automatic type inference based on the data in the column. When a column data type is inferred or set manually, this automatically sets the appropriate column configuration for this type for rendering, editing, filtering, row grouping and Import & Export. The pre-defined cell data types can be [overridden](https://www.ag-grid.com/javascript-data-grid/cell-data-types/?ref=blog.ag-grid.com#overriding-the-pre-defined-cell-data-type-definitions) and fully [custom data types](https://www.ag-grid.com/javascript-data-grid/cell-data-types/?ref=blog.ag-grid.com#providing-custom-cell-data-types) can be provided for complex objects.
> 
> This allows a lot of the configuration on column definitions that previously had to be manually set to now be automatically provided by the grid based on the data type. This allows numbers, dates and boolean values to be correctly displayed and parsed when edited without any extra configuration on the column definition.

![](https://blog.ag-grid.com/content/images/2023/06/cell_types_infer.gif align="left")

### **Built-in Cell Editors**

> AG Grid 30 adds built-in cell editors for [number](https://www.ag-grid.com/javascript-data-grid/provided-cell-editors/?ref=blog.ag-grid.com#number-cell-editor), [date](https://www.ag-grid.com/javascript-data-grid/provided-cell-editors/?ref=blog.ag-grid.com#date-cell-editor) and a [boolean](https://www.ag-grid.com/javascript-data-grid/provided-cell-editors/?ref=blog.ag-grid.com#checkbox-cell-editor) cell data types. These built-in cell editors can be customized and provide a good user experience without the use of third-party editor components. See the new editors are documented and [demonstrated here](https://www.ag-grid.com/javascript-data-grid/provided-cell-editors/?ref=blog.ag-grid.com#top).

![](https://blog.ag-grid.com/content/images/2023/06/built_in_editors.gif align="left")

Sources:

* [https://blog.ag-grid.com/whats-new-in-ag-grid-30/](https://blog.ag-grid.com/whats-new-in-ag-grid-30/)
    

---

## RxJs

---

## Lightweight Architectures with Sheriff and Angular's Latest Features

### Folder per domain

Each area doesn't need to know about other areas, and because of that we need to restrict - e.g. we don't want the `luggage` with `checking` - Nx can restrict access with domains - **Sheriff library**

### Standalone APIs

TBD

```typescript
bootstrapApplication(AppComponent, {
    providers: [
        provideHttpClient(
            withInterceptors ([authInterceptor]),
        ),
        provideRouter (APP_ROUTES,
            withPreloading(PreloadAl1Modules),
            withDebugTracing(),
        )]
});
```

### Custom Providers

```typescript
export function provideLogger(
  config: Partial<LoggerConfig>,
  ...features: LoggerFeature[]
): EnvironmentProviders {
  const merged = { ...defaultConfig, ...config };
  
  // Implementation...

 return makeEnvironmentProviders([
    LoggerService,
    {
      provide: LoggerConfig,
      useValue: merged,
    },
    {
      provide: LOG_FORMATTER,
      useValue: merged.formatter,
    },
    merged.appenders.map((a) => ({
      provide: LOG_APPENDERS,
      useClass: a,
      multi: true,
    })),
    features?.map((f) => f.providers),
  ]);
}
```

Then add it to the application

```typescript
bootstrapApplication (AppComponent, {
    providers: [
        provideLogger (loggerConfig, withColor({ debug: 3 })
    ]
});
```

Lazy loading routing with default export

```typescript
{
    path: 'flight-booking',
    canActivate: [() => inject(AuthService).isAuthenticated()],
    loadChildren: () =>
            import('./domains/ticketing/feature-booking')
                .then(m => m.FLIGHT_BOOKING_ROUTES)
},
{
    path: 'flight-booking',
    //                  👇 With default export
    loadChildren: () => import('./[...]/flight-booking.routes')
},
```

### Functional Guards and Resolvers

```typescript
export const APP_ROUTES: Routes = [ 
    [...]
    {
        path: 'flight-booking',
        //                  👇 Functional Guard
        canActivate: [() => inject(AuthService).isAuthenticated()],
        // 
        resolve: {
        //                👇 Functional Resolver
            flights: () => inject(FlightService).findAll()
        },
        component: FlightBookingComponent
    },
]
```

### Functional Interceptors

```typescript
// Creating the interceptor
export const authInterceptor: HttpInterceptorFn = (req, next) => {
    [...]
}

// Registering the interceptor
bootstrapApplication(AppComponent, { providers: [
    provideHttpClient(
      withInterceptors([authInterceptor]),
),]
});
```

### Reactive State Management with Signals

```typescript
@Injectable({ providedIn: 'root' })
export class FlightBookingFacade {
    private flightService = inject(FlightService);
    private _flights = signal<Flight[]>([]);
   
    readonly flights = this._flights.asReadonly(); 
    [...]

    // 
    async load(from: string, to: string) {
        const flights = await this.flightService.findPromise(from, to);
        this._flights.set(flights);
    }
}
```

Sources:

* [https://github.com/softarc-consulting/sheriff](https://github.com/softarc-consulting/sheriff)
    
* [https://www.angulararchitects.io/en/konferenzen/lightweight-architectures-with-angulars-latest-innovations-4/](https://www.angulararchitects.io/en/konferenzen/lightweight-architectures-with-angulars-latest-innovations-4/)
    
* [https://github.com/alfredoperez/standalone-example-cli](https://github.com/alfredoperez/standalone-example-cli)
    

%[https://www.youtube.com/watch?v=Y1_KwcJxQKg]