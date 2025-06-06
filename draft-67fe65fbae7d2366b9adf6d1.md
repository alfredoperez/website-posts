---
title: "Creating Angular Guardrails: A Guide to Custom Cursor Rules for Better Code"
slug: creating-angular-guardrails-a-guide-to-custom-cursor-rules-for-better-code

---

<mark>Image: Cursor logo with Angular logo and a shield icon representing guardrails/protection</mark>

Ever feel like you're fighting an uphill battle to maintain coding standards across your Angular project? Many of us have. You establish style guides, meticulously review PRs, and hold team meetings—yet, inconsistencies often creep in. Component structures vary, naming conventions drift, and that agreed-upon state management pattern? It can sometimes feel more like a suggestion than a firm rule.

What if your editor could help enforce these standards as your team codes? Enter Cursor AI's rules system—a significant advantage for Angular teams aiming to maintain consistency without constant manual oversight.

## What Are Cursor Rules?

[Cursor Rules](https://docs.cursor.com/context/rules) are persistent, reusable instructions that guide Cursor's AI as it assists with your code. Think of them as "system prompts" active across your interactions, ensuring the AI consistently adheres to your team's standards and best practices.

For Angular developers, rules are particularly effective because the framework itself has many established opinions and patterns:

* Component structure and lifecycle management
    
* Naming conventions
    
* Reactivity patterns
    
* Forms
    
* Tables
    
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

## Rule Types

By working with the three metadata options (`description`, `globs`, and `alwaysApply`), we can organize Cursor rules into four [distinct types](https://docs.cursor.com/context/rules#rule-type).

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

For this type of file you can use the suffix of `always`, e.g. `.cursor/rules/angular-always.mdc`. *Always Rules* are particularly useful for enforcing core Angular practices across your project, such as consistent use of standalone components, proper dependency injection patterns, OnPush change detection strategy, and standardized error handling in HTTP interceptors.

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

These are context-specific rules that the AI can intelligently pull in based on task relevance. The `description`field must provide comprehensive context about when to apply the rule, while `globs` should be left blank and `alwaysApply` should be false.

```markdown
---
description: "Guidelines for implementing modern Angular patterns using signals and defer blocks"
---
```

For this type, use the suffix `-agent.mdc`, e.g. `.cursor/rules/rxjs-state-agent.mdc` .

Agent Requested Rules are perfect for specialized Angular patterns that only apply in certain contexts, such as implementing NgRx selectors, setting up complex RxJS operator chains, configuring route guards, or implementing custom form validators.

```md
---
description: "Guidelines for implementing modern Angular patterns using signals and defer blocks"
alwaysApply: false
---
# Modern Component with Signals and Defer

## Core Rules
1. Use signals for all component state
2. Implement computed values for derived state
3. Use @defer for lazy loading heavy components
4. Leverage new control flow syntax (@if, @for)
5. Keep components standalone by default

## Best Practices
1. Use inject() for dependency injection
2. Implement proper cleanup in effects
3. Use proper typing for signals
4. Keep components focused and small
5. Use proper error boundaries

<example>
@Component({
  selector: 'app-user-list',
  standalone: true,
  imports: [CommonModule],
  template: `
    @if (loading()) {
      <div>Loading...</div>
    } @else {
      <div class="user-list">
        @for (user of users(); track user.id) {
          <div class="user-card">
            <h3>{{ user.name }}</h3>
            <p>{{ user.email }}</p>
          </div>
        } @empty {
          <p>No users found</p>
        }
      </div>
    }

    @defer (on viewport) {
      <app-user-stats [users]="users()" />
    } @loading {
      <div>Loading stats...</div>
    }
  `
})
export class UserListComponent {
  private readonly userService = inject(UserService);

  // State with signals
  loading = signal(false);
  users = signal<User[]>([]);

  // Computed values
  userCount = computed(() => this.users().length);
  activeUsers = computed(() =>
    this.users().filter(user => user.status === 'active')
  );
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

This approach is ideal for specific patterns in less common tasks, like implementing custom form validators with signals, setting up complex animations with the new animation API, or creating custom structural directives with modern Angular features.

````markdown
```md
---
---
# Custom Form Validator with Signals
## Core Rules
1. Use signals for reactive form validation
2. Implement computed values for error messages
3. Use proper typing for form controls
4. Keep validation logic in separate methods

## Best Practices
1. Use inject() for dependency injection
2. Implement proper form cleanup
3. Use proper typing for validators
4. Keep validation rules focused and small


<example>
@Component({
  selector: 'app-password-form',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  template: `
    <form [formGroup]="form">
      <input
        type="password"
        formControlName="password"
        [class.error]="passwordError()"
      />
      @if (passwordError()) {
        <div class="error-message">
          {{ passwordError() }}
        </div>
      }
    </form>
  `
})
export class PasswordFormComponent {
  private readonly fb = inject(FormBuilder);

  form = this.fb.group({
    password: ['', [Validators.required, this.passwordStrengthValidator()]]
  });

  passwordError = computed(() => {
    const control = this.form.get('password');
    if (!control?.touched) return null;
    return control.errors?.['passwordStrength']?.message;
  });

  private passwordStrengthValidator(): ValidatorFn {
    return (control: AbstractControl) => {
      const value = control.value;
      if (!value) return null;

      const hasUpperCase = /[A-Z]/.test(value);
      const hasLowerCase = /[a-z]/.test(value);
      const hasNumber = /[0-9]/.test(value);

      if (!hasUpperCase || !hasLowerCase || !hasNumber) {
        return {
          passwordStrength: {
            message: 'Password must contain uppercase, lowercase, and numbers'
          }
        };
      }
      return null;
    };
  }
}
</example>
```
````

## Example

Let's see how AI generates a component both with and without rules. First, here's a basic component without rules:

```typescript
@Component({
  selector: 'app-users',
  templateUrl: './users.component.html',
  styleUrls: ['./users.component.scss']
})
export class UsersComponent implements OnInit {
  @Input() title: string = 'Users';
  @Output() userAdded = new EventEmitter<User>();

  public users: User[] = [];
  public loading = false;
  private userService: UserService;

  constructor(userService: UserService) {
    this.userService = userService;
  }

  ngOnInit() {
    this.loadUsers();
  }

  loadUsers() {
    this.loading = true;
    this.userService.getUsers().subscribe({
      next: (users) => {
        this.users = users;
        this.loading = false;
      },
      error: (error) => {
        console.error('Error loading users:', error);
        this.loading = false;
      }
    });
  }

  addUser() {
    // Add user logic
    this.userAdded.emit(newUser);
  }
}
```

Now, here's the same component following our rules:

```typescript
@Component({
  selector: 'app-users',
  standalone: true,
  imports: [CommonModule, MatTableModule, MatButtonModule],
  template: `
    <div class="users-container">
      <h2>{{ title() }}</h2>

      @if (loading()) {
        <div>Loading...</div>
      } @else {
        <table mat-table [dataSource]="users()">
          <ng-container matColumnDef="name">
            <th mat-header-cell *matHeaderCellDef>Name</th>
            <td mat-cell *matCellDef="let user">{{ user.name }}</td>
          </ng-container>

          <ng-container matColumnDef="email">
            <th mat-header-cell *matHeaderCellDef>Email</th>
            <td mat-cell *matCellDef="let user">{{ user.email }}</td>
          </ng-container>

          <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
          <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
        </table>

        <button mat-raised-button color="primary" (click)="onAddUser()">
          Add User
        </button>
      }
    </div>
  `
})
export class UsersComponent {
  // 1. Injected services
  private readonly userService = inject(UserService);
  private readonly cdr = inject(ChangeDetectorRef);

  // 2. Inputs
  title = input<string>('Users');

  // 3. Outputs
  userAdded = output<User>();

  // 4. Signals
  loading = signal(false);
  users = signal<User[]>([]);

  // 5. Other properties
  displayedColumns = ['name', 'email'];

  // Computed values
  userCount = computed(() => this.users().length);

  constructor() {
    // Effect for data loading
    effect(() => {
      this.loading.set(true);
      this.userService.getUsers().subscribe({
        next: (users) => {
          this.users.set(users);
          this.loading.set(false);
        },
        error: (error) => {
          console.error('Error loading users:', error);
          this.loading.set(false);
        }
      });
    });
  }

  // 1. Public methods
  onAddUser(): void {
    const newUser = { name: 'New User', email: 'new@example.com' };
    this.userAdded.emit(newUser);
  }

  // 2. Protected methods (none in this example)

  // 3. Private methods
  private updateLoadingState(isLoading: boolean): void {
    this.loading.set(isLoading);
  }
}
```

The rules-guided version is like upgrading from a basic sedan to a modern sports car. Here's what changed:

1. **Structure & Organization**
    
    * Everything's in the right place (services → inputs → outputs → signals)
        
    * No more scattered properties and methods
        
    * Clean, inline template instead of separate files
        
2. **Modern Angular Features**
    
    * Swapped old decorators for modern `input()`/`output()`
        
    * Replaced manual state management with signals
        
    * Using the new control flow syntax (`@if`) instead of `*ngIf`
        
3. **Performance & Maintainability**
    
    * Signals for better performance
        
    * Computed values for derived state
        
    * Proper error handling
        
    * Material Design integration
        

The result? Code that's not just cleaner but actually performs better and is easier to maintain.

## TL;DR;

Different rule types in Angular projects can be organized strategically to enhance AI assistance.

* "Auto" rules are frequently applied automatically based on specific file patterns, making them perfect for file-specific tasks.
    
* "Always" rules are constant and apply across the entire project, triggered during every AI interaction to uphold core principles, naming conventions, and import order.
    
* "Agent" rules are used with medium frequency and are context-specific, determined by the AI based on relevance, making them suitable for state management tasks like RxJS/Signals and NgRx setup.
    
* "Manual" rules are less common and are used for specialized tasks that require explicit requests, such as migration guides, internationalization (i18n) setup, and complex authentication flows.
    

This hierarchy ensures the AI isn't overwhelmed with too much context while still providing the necessary guidance when needed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749236367630/ff04da39-9f68-47e4-8910-3e122601f968.png align="center")

Implementing custom Cursor rules for your Angular project establishes guardrails that gently guide both your team and the AI toward consistent, high-quality code. By codifying your standards directly within your development environment, you reduce cognitive load, minimize code review debates, and help maintain architectural integrity.

Rules become particularly valuable as your team grows or as you integrate AI more deeply into your workflow. They ensure that regardless of who (or what) is writing the code, it adheres to the same consistent patterns.

---

In the next article, we'll build on this foundation and explore how to create your first rules for your angular project by providing a custom structure and an easy way to update them.