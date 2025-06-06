---
title: "Creating Angular Guardrails: A Guide to Custom Cursor Rules for Better Code"
slug: creating-angular-guardrails-a-guide-to-custom-cursor-rules-for-better-code

---

<mark>Image: Cursor logo with Angular logo and a shield icon representing guardrails/protection</mark>

Ever feel like you're fighting an uphill battle to maintain coding standards across your Angular project? Many of us have. You establish style guides, meticulously review PRs, and hold team meetings—yet, inconsistencies often creep in. Component structures vary, naming conventions drift, and that agreed-upon state management pattern? It can sometimes feel more like a suggestion than a firm rule.

What if your editor could help enforce these standards as your team codes? Enter Cursor AI's rules system—a significant advantage for Angular teams aiming to maintain consistency without constant manual oversight.

## What Are Cursor Rules?

Cursor Rules are persistent, reusable instructions that guide Cursor's AI as it assists with your code. Think of them as "system prompts" active across your interactions, ensuring the AI consistently adheres to your team's standards and best practices.

For Angular developers, rules are particularly effective because the framework itself has many established opinions and patterns:

* Component structure and lifecycle management
    
* Naming conventions
    
* Reactive patterns with RxJS
    
* Use of signals vs observables
    
* State management approaches
    

Without guidance, an AI assistant might generate technically correct code that clashes with your project's conventions. Rules address this by embedding your standards directly into the AI's decision-making process.

Crucially, these rules also benefit developers directly. They serve as a readable, up-to-date reference for team guidelines. **It's a win-win: the AI performs more effectively, and your team has a clear, current source for coding standards.**

## Understanding the .mdc structure

Cursor stores project-specific rules as Markdown Component files (`.mdc`) within a `.cursor/rules/` directory at the root of your project. This allows your rules to be version-controlled alongside your codebase—a major asset for team consistency.

Consider the basic structure of an `.mdc` rule file:

```md
---
description:
globs:
alwaysApply: true
---

## Section

- Rule
- Rule
<example type="valid">

</example>
<example type="invalid">

</example>

- Rule
```

This basic structure places metadata at the top—configuring where and when rules apply—followed by the rules themselves, with examples if necessary.

<mark>TODO: Get the definition and link from cursor about the metadata</mark>

## Rule Types

By working with the three metadata options (`description`, `globs`, and `alwaysApply`), we can organize Cursor rules into four distinct types.

Next, I'll describe each type, its setup, and a suggested suffix for easy identification:

**Auto Rule**

These rules are automatically applied based on the file patterns you're working with.

For the metadata, only the `globs` need to be configured, enabling Cursor to identify which files are being modified and apply the rules automatically.

```yaml
---
description: # Optional: A brief note about the rule's purpose
globs: ["*.component.ts", "src/app/components/**/*.ts"]
alwaysApply: false # Default, can be omitted if false
---
```

For this type of rule, use the suffix `auto`, e.g., `.cursor/rules/angular-component-auto.mdc`.

An example of an Auto Rule for Angular components is shown below, enforcing standards whenever working on such files:

````yaml

---
globs: ["*.component.ts", "src/app/components/**/*.ts"]
---

# Angular Component Standards

All components should follow these standards:

## Structure

- Use standalone components when applicable
- Property order:
  1. Injected services (using `inject()`)
  2. Inputs
  3. Outputs
  4. Signals
  5. Other properties
- Method order:
  1. Public methods
  2. Protected methods
  3. Private methods

## Modern Angular Features

- Use `input()` and `input.required()` instead of `@Input()`
- Use `output()` instead of `@Output()`
- Use `model()` for two-way binding
- Use `inject()` for dependency injection
- Avoid `public` accessor
- Use `protected` when possible

## Event Handling

- Output names should be the action (e.g., `click`, `submit`)
- Handler methods should use `on` prefix (e.g., `onClick`, `onSubmit`)
- Example: `<button (click)="onClick($event)">`

## Example Structure:

```typescript
@Component({
  selector: 'app-feature',
  templateUrl: './feature.component.html',
  styleUrls: ['./feature.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush,
  standalone: true,
  imports: [CommonModule, RouterModule, SharedModule]
})
export class FeatureComponent implements OnInit {
  // 1. Injected services (using inject())
  private readonly featureService = inject(FeatureService);
  private readonly cdr = inject(ChangeDetectorRef);

  // 2. Inputs
  data = input<FeatureData | null>(null);
  requiredId = input.required<string>();

  // 3. Outputs
  click = output<Action>();
  submit = output<FormData>();

  // 4. Signals
  loading = signal(false);
  items = signal<Item[]>([]);

  // Computed signals for derived state
  hasItems = computed(() => this.items().length > 0);
  itemCount = computed(() => this.items().length);

  // 5. Other properties
  // (none in this example)

  constructor() {
    // Use effects for reactive behavior with signals
    effect(() => {
      const currentData = this.data();
      if (currentData) {
        // Handle data changes
        this.items.set(currentData.items || []);
      }
    });
  }

  ngOnInit(): void {
    // Component initialization
  }

  // 1. Public methods
  onClick(event: Event): void {
    this.click.emit({ type: 'action', event });
  }

  onSubmit(formData: FormData): void {
    this.submit.emit(formData);
  }

  // 2. Protected methods (when needed)
  protected handleData(data: Item[]): void {
    this.items.set(data);
  }

  // 3. Private methods
  private updateLoadingState(isLoading: boolean): void {
    this.loading.set(isLoading);
  }
}
````

**Always Rule**

These rules establish foundational guidelines that apply to all AI interactions within your project. Since these are always applied, we don't need to set the `globs` or the `description` fields.

```md
---
alwaysApply: true
---
```

For this type of file you can use the suffix of `always`, e.g. `.cursor/rules/angular-always.mdc`

```md
---
alwaysApply: true
---


# Global Angular Conventions

These conventions apply to all Angular code in this project:

## Core Principles

- Write clear, maintainable, and scalable code
- Apply immutability and pure functions
- Favor component composition for modularity
- Use meaningful variable names (e.g., `isActive`, `hasPermission`)
- Follow Angular's official style guide

## File Structure & Naming

- Use kebab-case for all files
- Components: PascalCase with Component suffix (UserProfileComponent)
- Services: PascalCase with Service suffix (AuthenticationService)
- Interfaces: PascalCase with prefix I (IUserProfile) or descriptive name (UserProfile)
- Enums: PascalCase (UserRole)
- Constants: UPPER_SNAKE_CASE (API_ENDPOINTS)

## Import Order

1. Angular core and common modules
2. RxJS modules
3. Other Angular modules
4. Application core imports
5. Shared module imports
6. Environment-specific imports
7. Relative path imports

## Code Style

- Use TypeScript strict mode
- No any types - use proper typing or unknown if necessary
- Prefer const over let, avoid var
- Use async/await when possible instead of Promise chains
- Document public APIs with JSDoc comments

## Structure

- Organize code by feature modules
- Keep components under 300 lines where possible
- Extract complex logic to services
- Use smart/dumb component pattern:
  - Smart (container) components handle data and state
  - Dumb (presentation) components receive inputs and emit outputs

## State Management

- Use NgRx for global application state
- Use component state for UI-only concerns
- Use services with signals for shared state within a feature
```

**Agent Requested Rules**

These are context-specific rules that the AI can intelligently pull in based on task relevance. The `description` field must provide comprehensive context about when to apply the rule, while `globs` should be left blank and `alwaysApply` should be false. These are perfect for specialized Angular patterns that only apply in certain contexts, like state management, API integration, or NgRx setup.

```md
---
description: "Guidelines for implementing state management in Angular services using RxJS"
alwaysApply: false
---
```

For this type, use the suffix `-agent.mdc`, e.g. `.cursor/rules/rxjs-state-agent.mdc`

```md
---
description: "Guidelines for implementing state management in Angular services using RxJS"
---

# RxJS State Management in Services

For services that manage state:

1. Use BehaviorSubject for internal state
2. Expose readonly Observables via public getters
3. Provide methods to update state
4. Document selector functions

## Example Implementation:

<example>
@Injectable({
  providedIn: 'root'
})
export class UserStateService {
  // Private state
  private readonly _users = new BehaviorSubject<User[]>([]);

  // Public selectors
  readonly users$ = this._users.asObservable();
  readonly userCount$ = this.users$.pipe(
    map(users => users.length)
  );

  // Action methods
  addUser(user: User): void {
    const currentUsers = this._users.getValue();
    this._users.next([...currentUsers, user]);
  }

  // More methods...
}
</example>
```

**Manual Rules**

These are specialized rules intended for application only when explicitly requested. For this type, the `description` and `globs` fields should be left blank, and `alwaysApply` set to `false`. This approach is ideal for specific patterns in less common tasks, like internationalization setup or complex authentication flows.

```yaml
---
# No metadata flags needed
alwaysApply: false # Explicitly set to false or omit
---
```

For this type, use the suffix `-manual.mdc`, e.g., `.cursor/rules/i18n-setup-manual.mdc`.

Notice how AI-generated code, when guided by rules, perfectly aligns with our project standards:

* Employs OnPush change detection
    
* Adheres to proper naming conventions
    
* Implements lifecycle methods correctly
    
* Manages subscriptions effectively (e.g., with a takeUntil pattern)
    
* Uses the correct selector prefix
    
* Enforces proper typing
    
* Delegates HTTP calls to services
    
* Includes loading states and error handling
    

## TL;DR;

When to Use Each Type:

| Rule Type | Frequency | Scope | Activation |
| --- | --- | --- | --- |
| Auto | High | File-specific | Automatic (file patterns) |
| Always | Constant | Project-wide | Every AI interaction |
| Agent | Medium | Context-specific | AI-decided (relevance) |
| Manual | Low | Specialized tasks | Explicitly requested |

Practical Angular Example Strategy: Always: Core principles, naming conventions, import order. Auto: Component structure, service patterns (for `.component.ts`, `.service.ts` files). Agent: State management (RxJS/Signals), NgRx setup. Manual: Migration guides, i18n setup, complex authentication flows. This hierarchy helps ensure the AI isn't overwhelmed with excessive context, while still having the right guidance available when needed.\*\*

## Conclusion

Implementing custom Cursor rules for your Angular project establishes guardrails that gently guide both your team and the AI toward consistent, high-quality code. By codifying your standards directly within your development environment, you reduce cognitive load, minimize code review debates, and help maintain architectural integrity.

Rules become particularly valuable as your team grows or as you integrate AI more deeply into your workflow. They ensure that no matter who (or what) is writing the code, it adheres to the same consistent patterns.

In the next article, we'll build on this foundation and explore how to create reusable templates with Cursor Notepads, taking your productivity to the next level.

Have you implemented rules in your Angular projects? What standards do you find most important to enforce? Share your experiences in the comments below!

---

*Looking for more ways to supercharge your Angular development? Check out our full series on Cursor AI for Angular:*

1. **Building Your Angular Guardrails: Creating Custom Cursor Rules for Consistent Code**
    
2. *Coming next: Creating Your First Set of Angular Cursor Rules in 10 Minutes*
    
3. *Coming next: Reusable Angular Patterns: Mastering Cursor Notepads & Templates*