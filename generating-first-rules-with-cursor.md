---
title: "Generating Your First Rules with Cursor for Your Angular Project"
domain: www.alfredo-perez.dev
seoTitle: "Generate Cursor Rules for Angular Projects"
seoDescription: "Learn to create meta-rules that generate consistent Cursor AI rules for Angular using best practices and package.json dependencies"
slug: generating-first-rules-with-cursor-angular
canonical: https://medium.com/ngconf/generating-your-first-rules-with-cursor-for-your-angular-project-20a6d013ac9d
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766867201075/314f94e0-01e7-42f6-bb5b-d1d6fe928fb7.jpeg
tags: ai, angular, cursor-ide, vibe-coding, web-development

---

Building consistent, high-quality Angular applications becomes increasingly challenging as your codebase grows and your team expands. While Cursor IDE’s rule system offers powerful automation capabilities, getting started can feel overwhelming when you’re staring at a blank `.cursor/rules` directory and you might be making the mistake of using Cursor just as a fancy autocomplete.

In our previous article, we explored the four fundamental rule types that form the backbone of Cursor’s automation system:

* **Always Rules**: Global rules that apply to every chat and command, perfect for defining how AI should respond or general behavior patterns.
    
* **Auto-Apply Rules**: Rules that automatically activate based on file patterns, ideal for components, services, testing, and other file-specific guidelines
    
* **Agent-Select Rules**: Contextual rules that Cursor intelligently chooses based on what you’re working on, great for complex scenarios like state management or advanced routing
    
* **Manual Rules**: Custom-made rules you reference explicitly, useful for unique patterns like feature flags or company-specific conventions.
    

Make sure to read the previous article in case you want to understand more about these rule types:

[**Creating Angular Guardrails: A Guide to Custom Cursor Rules for Better Code**  
*Ever feel like you’re fighting an uphill battle to maintain coding standards across your Angular project? Many of us…*medium.com](https://medium.com/ngconf/creating-angular-guardrails-a-guide-to-custom-cursor-rules-for-better-code-df5523eafb5d)

The challenge many Angular developers face is figuring out **where to start**. Instead of manually crafting each rule from scratch — trying to figure out naming conventions, folder structures, and frontmatter configurations ***What if you could generate in a few minutes a useful starting point made specifically for Angular development?***

This article introduces a meta-rule approach that provides exactly that foundation. By the end, you’ll have a complete rule generation system that understands Angular project structures, automatically organizes rules by functionality, and gives you powerful prompts to generate your initial rule set based on your actual project dependencies.

### Meta Rule Explanation

Before diving into our Angular-specific implementation, let’s check two excellent examples that inspired this approach. Understanding these will help you understand why our improved and Angular-focused variation is better for frontend development needs.

#### Example 1—BMAD Method

The BMAD approach, demonstrated in [this YouTube video](https://youtu.be/vjyAba8-QA8?si=Ub57pZ0eJsmKl4ho) and available on [GitHub](https://github.com/bmadcode/cursor-custom-agents-rules-generator/blob/main/.cursor/rules/core-rules/rule-generating-agent.mdc), provides a comprehensive framework for rule generation.

<iframe src="https://www.youtube.com/embed/vjyAba8-QA8?feature=oembed" width="700" height="393"></iframe>

The key parts:

* **Strict Rule Type Definitions**: Clearly defines the 4 frontmatter types with specific naming conventions (`-manual.mdc`, `-auto.mdc`, `-always.mdc`, `-agent.mdc`)
    
* **Technology-Agnostic Organization**: Uses folders like `core-rules`, `ts-rules`, `py-rules`, `ui-rules` that scale across different tech stacks
    
* **Comprehensive Template Structure**: Includes required examples (both valid and invalid), detailed glob pattern guidance, and automated confirmation responses
    
* **Built-in Validation**: Enforces consistent file locations and prevents rule duplication
    

Here you can find a link to the meta-rule from the BMAD method:

[**cursor-custom-agents-rules-generator/.cursor/rules/core-rules/rule-generating-agent.mdc at main ·…**  
*Maximize the potential of Cursor best practices for Automatic Rule and Custom Agent Generation and Agile Workflows …*github.com](https://github.com/bmadcode/cursor-custom-agents-rules-generator/blob/main/.cursor/rules/core-rules/rule-generating-agent.mdc)

### Example 2—Elie Steinbock Method

Elie’s approach, featured in [this video](https://www.youtube.com/watch?v=sxqq9Eql6Cc) with the rule available on [GitHub](https://github.com/elie222/inbox-zero/blob/main/.cursor/rules/cursor-rules.mdc), focuses on simplicity and clarity.

<iframe src="https://www.youtube.com/embed/sxqq9Eql6Cc?feature=oembed" width="640" height="480"></iframe>

The key parts:

* **Minimal Overhead**: Provides essential structure without overwhelming complexity
    
* **Clear Directory Guidelines**: Simple, straightforward placement rules that are easy to follow
    
* **Practical Focus**: Shows exactly what developers need to know to get started quickly
    
* **Human-Readable Format**: Uses straightforward language that both AI and team members can easily understand
    

Here is a link to the meta-rule:

[**inbox-zero/.cursor/rules/cursor-rules.mdc at main · elie222/inbox-zero**  
*The world's best AI personal assistant for email. Open source app to help you reach inbox zero fast. …*github.com](https://github.com/elie222/inbox-zero/blob/main/.cursor/rules/cursor-rules.mdc)

While both approaches offer valuable insights into creating a meta-rule for rule management, their focus is primarily on general development scenarios. Angular projects have specific patterns—component hierarchies, state management libraries, testing strategies, and styling conventions—that benefit from a more targeted organizational approach.

### Cursor Meta Rule for Angular Development

Building on the strengths of both examples, here’s a meta rule specifically designed for Angular development. This rule maintains the robustness of the BMAD approach while incorporating the clarity of Elie’s method, but organizes everything around how Angular developers actually think about their projects.

```markdown
---
description: This rule is essential for maintaining consistency and quality in rule creation across the codebase. It must be followed whenever: (1) A user requests a new rule to be created, (2) An existing rule needs modification, (3) The user asks to remember certain behaviors or patterns, or (4) Future behavior changes are requested. This rule ensures proper organization, clear documentation, and effective rule application by defining standard formats, naming conventions, and content requirements. It's particularly crucial for maintaining the rule hierarchy, ensuring rules are discoverable by the AI, and preserving the effectiveness of the rule-based system. The rule system is fundamental to project consistency, code quality, and automated assistance effectiveness.
globs:
alwaysApply: true
---
```

# Rule Generation Guidelines

This document outlines the standard format for creating and organizing all AI-generated rules for this project. Its main goal is to ensure clarity, consistency, and a scalable structure.

- Check for existing rules before creating new ones @.cursor/rules
- Update existing rules when possible instead of creating duplicates

## Organizational Folders

- Do not separate by library (e.g., do not use a `libraries/` folder).  
- Rule files must be located and named using the format: `.cursor/rules/{area-folder}/{area-of-focus}-{type}.mdc`. The `{area-of-focus}` describes the specific part of the technology or functionality the rule applies to.

* `.cursor/rules/global/`: Rules that apply to every chat and command. Also, Cursor behavior and rule generation rules
    
* `.cursor/rules/angular/`: Angular framework rules. Examples:
    
* For rules about Angular components: `.cursor/rules/angular/component-development-agent.mdc`
    
* For rules about Angular services: `.cursor/rules/angular/services-state-agent.mdc`
    
* For broad Angular rules: `.cursor/rules/angular/global-standards-agent.mdc`
    
* `.cursor/rules/state/`: State management rules . Examples:
    
* For NGXS: `.cursor/rules/state/ngxs-state-agent.mdc`
    
* For TanStack Query: `.cursor/rules/state/tanstack-query-agent.mdc`
    
* `.cursor/rules/testing/`: Testing rules . Examples:
    
* For Jest: `.cursor/rules/testing/jest-testing-agent.mdc`
    
* For Angular testing: `.cursor/rules/testing/testing-standards-auto.mdc`
    
* `.cursor/rules/typescript/`: General TypeScript rules. Examples
    
* For error handling: `.cursor/rules/typescript/typescript-error-handling-agent.mdc`
    
* For style: `.cursor/rules/typescript/typescript-style-agent.mdc`
    
* `.cursor/rules/styles/`: CSS, SCSS, and styling rules. Examples:
    
* For SCSS conventions: `.cursor/rules/styles/scss-conventions-agent.mdc`
    
* For typography: `.cursor/rules/styles/scss-typography-classes-agent.mdc`
    

## Metadata  
The frontmatter is mandatory and determines rule application. When creating a rule, first make sure what type is it and base on that setup the metadata:

### Always

* Applies to all chats/commands. e.g. add emojis after reading a rule
    
* Suffix with `-always.mdc`
    
* `alwaysApply: true` and empty `description` and `globs`
    
* Must be in `.cursor/rules/global/`  
    ### Auto-Apply
    
* Use when the rule applies to files matching a glob.
    
* Suffix with `-auto.mdc`
    
* Requires `globs` pattern `alwaysApply: false` and `description` is empty  
    ### Agent Select  
    -Use when the rules that should be applied occasionally.  
    -Suffix the file name with `-agent.mdc`  
    -Requires descriptive `description`. `alwaysApply: false` and the `globs` is empty
    

### Manual (`-manual.mdc`):

* This is for rules that should be manually requested by the user
    
* Suffix with
    
* `alwaysApply: false` and `description` and `globs` should be empty  
    ### Glob Pattern Examples
    

Use these patterns in the `globs` frontmatter field to target specific files:

- All TypeScript: *.ts  
- All Angular Components: *.component.ts  
- All State Files: state/**/*.ts  
- All Tests: *.spec.ts  
- All Styles: *.scss

### Description

* Keep rule descriptions under 200 characters for better readability  
    ## Rule Template Structure
    

- All rule files must follow this structure  
```mdc  
---  
description: `Comprehensive description that provides full context and clearly indicates when this rule should be applied. Include key scenarios, impacted areas, and why following this rule is important. While being thorough, remain focused and relevant. The description should be detailed enough that the agent can confidently determine whether to apply the rule in any given situation.`  
globs: .cursor/rules/**/*.mdc OR blank  
alwaysApply: {true or false}  
---

#Rule Title

## Critical Rules

- Concise, bulleted list of actionable rules the agent MUST follow  
- Rule with Example  
<example>  
{valid rule application}  
</example>

<example type="invalid">  
{invalid rule application}  
</example>  
```  
### Examples  
- Place examples immediately after their corresponding rules for clear context  
- Omit examples for rules that are clear without them  
- Keep examples brief and focused  
- Include only one example type when the rule is self-explanatory

## Confirmation Response Format

- After creating or updating a rule, send a message with this format:

🧠 Rule Generation Success: \[file path\]  
Rule Type: \[Agent Select | Auto-Apply | Global | Manual\]  
Rule Description: \[Brief description of the rule's purpose\]  
Rule metadata:

### Understanding the Meta Rule Structure

Let's deconstruct the key sections of our Angular meta rule and comprehend the rationale behind the design of each component.

**Rules Folders**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867187855/bfdfbf2f-f59c-4870-a7ed-c58bfc7fb780.png align="left")

Example showing the rules folders.

> The folder organization is the heart of what makes this Angular-specific. Instead of generic categories like `ui-rules` or `ts-rules`, we organize by Angular development concerns:

* `**/angular/**` - Framework-specific patterns like component structure, lifecycle hooks, dependency injection, and routing conventions.
    
* `**/state/**` - State management is complex enough to warrant its category, whether you're using NGXS, NgRx, TanStack Query, or custom solutions.
    
* `**/testing/**`-Angular testing has unique patterns with TestBed, component testing, and service mocking that deserve focused organization.
    
* `**/styles/**`-CSS, SCSS, and styling conventions include guidelines on working with Angular's component-scoped styles and integrating your design system.
    

This structure mirrors how Angular developers mentally organize their work, making rules easier to find and maintain.

#### Rule Template Structure

Since these rules frequently serve as team documentation, the template prioritizes human readability by showing examples immediately after the rule is stated and only when it is crucial to have them.

```markdown
---
description: `Comprehensive description that provides full context and clearly indicates when this rule should be applied. Include key scenarios, impacted areas, and why following this rule is important. While being thorough, remain focused and relevant. The description should be detailed enough that the agent can confidently determine whether to apply the rule in any given situation.`
globs: .cursor/rules/**/*.mdc OR blank
alwaysApply: {true or false}
---

#Rule Title

## Critical Rules

- Concise, bulleted list of actionable rules the agent MUST follow
- Rule with Example
<example>
{valid rule application}
</example>

<example type="invalid">
{invalid rule application}
</example>
```

When a new developer joins your team or when you want to remember what the practices are, you can read through the rules to understand coding conventions, not just rely on AI automation.

#### Frontmatter Types in Angular Context

**Always Rules** are typically the rules you use to work with Cursor and are not closely related to Angular. Some examples of this can include how it should respond or whether it should include emojis

**Auto-Apply Rules** shine for file-specific patterns. For example, an auto-apply rule for `*.component.ts` files can enforce consistent component structure, lifecycle hook usage, and naming conventions.

**Agent-Select Rules** excel for contextual decisions. You might be working in a TypeScript file, but Cursor only applies your state management rules when it detects you’re actually working with state—not every time you touch a `.ts` file.

**Manual Rules** are situational rules that cannot be applied solely based on file extension or file type. These manual rules can include, for example, guidelines on managing forms or feature flags.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867189590/23da58bc-0f10-476e-a941-502bc564ff79.png align="left")

Cursor Rule types and when to se them

### How to Generate Rules

With our meta rule in place, let’s explore three approaches to generating your initial rule set, from basic to advanced.

#### Option 1: Cursor.directory Website

The simplest approach uses the [cursor.directory](https://cursor.directory/generate) website. You can upload your `package.json`file and get a basic set of rules generated automatically.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867191327/85e777c1-9a8d-4156-a405-e72df43507ca.png align="left")

Shows the cursor.directory website where you can generate the rules based on the package.json

Once you upload the file, it will generate all the rules based on the libraries you are using. Here’s an example of what cursor.directory generates for a typical Angular project:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867193230/80865d0a-89dc-4f8e-a9bf-2cb035ea40da.png align="left")

Shows rules generated by cursor.directory

While this file provides a starting point, the output is quite generic and doesn’t leverage our Angular-specific organizational structure. This is where our custom meta rule approach really shines.

#### Option 2: Prompts + Meta-rule

We are going to generate the rules directly on a cursor by relying on the meta rule we just created, but we are going to use two prompts. The first prompt is about using modern Angular application best practices and the second one is about complementing with libraries from the `package.json`

**Prompt 1. Modern Angular Development**

This uses the best practices outlined by the Angular team that can be found in the following link

[**Angular**  
*The web development framework for building modern apps.*angular.dev](https://angular.dev/ai/develop-with-ai)

Here is the complete prompt

Generate rules for each of the following using the @generating-rules-agent.mdc :

## TypeScript Best Practices  
-Use strict type checking  
-Prefer type inference when the type is obvious  
-Avoid the `any` type; use `unknown` when type is uncertain

## Angular Best Practices  
-Always use standalone components over NgModules  
-Don't use explicit standalone: true (it is implied by default)  
-Use signals for state management  
-Implement lazy loading for feature routes  
-Use NgOptimizedImage for all static images.

## Components  
- Keep components small and focused on a single responsibility  
-Use input() and output() functions instead of decorators  
-Use computed() for derived state  
-Set `changeDetection: ChangeDetectionStrategy.OnPush` in `@Component` decorator  
-Prefer inline templates for small components  
-Prefer Reactive forms instead of Template-driven ones  
-Do NOT use "ngClass" (NgClass), use "class" bindings instead  
-DO NOT use "ngStyle" (NgStyle), use "style" bindings instead

## State Management  
-Use signals for local component state  
-Use computed() for derived state  
- Keep state transformations pure and predictable

## Templates  
- Keep templates simple and avoid complex logic  
-Use native control flow (@if, @for, @switch) instead of *ngIf, *ngFor, *ngSwitch  
-Use the async pipe to handle observables

## Services  
-Design services around a single responsibility  
-Use the providedIn: 'root' option for singleton services  
-Use the inject() function instead of constructor injection

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867195182/237d2001-3a68-4b91-8b6f-ff288f8c068d.png align="left")

Output of Cursor after generating the rules

You can also opt-in for just using the rules as they come from the Angular team, but with the generated one, it will create a separate file for each that can be applied as needed and it will also have some examples that can also help developers understand what the rule is about.

**Prompt 2. Using installed packages**

The next prompt that we can use is one that, based on the installed packages, will get the best practices for each.

Here is the prompt that you can use:

Generate Cursor rules for significant libraries in @package.json  
Create a file for each library covered focusing on:

1. Library Selection Criteria:
    

* DO NOT generate rules for libraries not in package.json
    
* DO NOT generate rules that already exist
    
* DO NOT include initialization/configuration code
    

2. Rule Content Requirements:
    

* DO NOT include library initialization/setup code
    
* DO NOT include configuration examples
    
* FOCUS on usage patterns and best practices
    
* INCLUDE common pitfalls
    
* INCLUDE performance considerations
    

3. For Each Library Rule:
    

* Integration patterns
    
* Best practices
    
* Common issues
    
* Performance optimization
    
* Security considerations
    
* Testing strategies
    

4. Rule Format:
    

* Clear metadata
    
* Glob patterns
    
* Descriptive sections
    
* Practical examples of USAGE only
    
* Error prevention guidelines
    

5. Specific Exclusions:
    

* Configuration code
    
* Setup instructions
    
* Initialization patterns
    
* Module imports
    
* Provider configurations
    

## Library Categories and Rules

### State Management  
-**NgRx**: Store architecture, effects patterns, entity management  
-**NGXS**: Action/state/selector patterns, async operations  
-**TanStack Query**: Query invalidation, caching strategies  
-**Akita**: Entity stores, query patterns  
-**MobX**: Observable state management

### Component Libraries  
-**Angular Material**: Theming, accessibility, proper component usage  
-**PrimeNG**: Component configuration, theming  
-**Nebular**: Theme system, component integration  
-**Clarity Design System**: Design tokens, component patterns

### Testing Libraries  
-**Jest**: Configuration, mocking patterns, coverage  
-**Cypress**: E2E patterns, page objects, custom commands  
-**Playwright**: Cross-browser testing, fixtures  
-**Testing Library**: User-centric testing approaches

### Build & Architecture (DO NOT ADD ANYTHING)  
-**Nx**  
-**Angular CLI**  
-**Module Federation**

### Forms Libraries  
-**Formly**: Field configuration, custom field types  
-**Dynamic Forms**: Form generation, validation patterns

### Internationalization  
-**Transloco**: Translation management, lazy loading  
-**ngx-translate**: Translation patterns, runtime language switching

### Data Visualization  
-**D3**: Chart patterns, data binding, animations  
-**Chart.js**: Configuration, responsive charts  
-**NGX-Charts**: Data formatting, theme integration

### Authentication  
-**Auth0**: Authentication flows, guard patterns  
-**Firebase Auth**: Authentication state, security rules  
-**JWT**: Token management, refresh patterns

After generating rule add which metadata should be used. e.g.

-`accessibilit-agent.mdc`  
description: (The description)  
alwaysApply (true/false)  
globs: (The glob pattern)

-Example of what NOT to include (initialization/configuration code):  
```typescript  
export const translocoLoader = {  
provide: TRANSLOCO\_LOADER,  
useFactory: (http: HttpClient) => new TranslocoHttpLoader(http, './assets/i18n/', '.json')  
};

@NgModule({  
imports: \[TranslocoRootModule, TranslocoModule\],  
providers: \[  
translocoLoader,  
{  
provide: TRANSLOCO\_CONFIG,  
useValue: {  
fallbackLang: 'en',  
reRenderOnLangChange: true,  
defaultLang: 'en'  
} as TranslocoConfig  
}  
\]  
})  
```

This is what Cursor does; note that it only picked three libraries and also checked for any existing rules:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867197318/0dadb904-a167-4227-91ca-910e34ad4be8.png align="left")

Now, if you go to Cursor settings, and under the Rules section, you should see all the rules that were created:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766867198986/1509f90c-3077-48a2-a8d4-084245b0fb3b.png align="left")

Cursor Rules Settings with all the rules that were created

### Troubleshooting

Sometimes things don’t go as planned, and the rules might not turn out the way we expected. Before moving forward, I suggest checking a few things. You can either give the prompt another shot, try a new one that points to the meta-rule, or fix any issues that popped up.

Here are the things you should check:

* Folder Structure
    
* Metadata
    
* File Suffix
    
* Content
    

Pay special attention to the content, since AI tends to be verbose, and there might be a thing or two you can cut off.

Also, I recommend you set the following User Rules in Cursor to always use:

* Verify Cursor was able to find the rules:
    

Verify you can read the rules from `.cursor/rules/`  
and output 🟢 if you were able to read rules  
or 🟥 if you did not find anything.

* List all the rules used:
    

After using a rule, output:  
⚡ Rules Used:  
-Bulleted list of Rule names

### TLDR

Creating effective Cursor rules for Angular development doesn’t have to start from scratch. By implementing a well-designed meta rule, you can:

* **Generate comprehensive starting points** using prompts that understand your project’s actual dependencies
    
* **Maintain consistent organization** with Angular-focused folder structures that match how you think about development
    
* **Evolve rules iteratively** as your team discovers new patterns and refines existing conventions
    

The most important success metric isn’t perfect automation—it’s **team engagement with rule refinement**. When developers actively suggest rule updates and modifications, it means they’re using the system and finding value in the consistency it provides.

Remember that rule creation is fundamentally an iterative process. Your team might prefer more concise rule formats, different organizational structures, or additional examples. The meta-rule approach gives you a solid foundation to customize according to your specific needs.

To generate rules for your project, you just have to do the following:

* Create the meta-rule in `./cursor/rules/global`
    
* Use the prompt to create rules from Angular Team recommended rules
    
* Use the prompt to create rules based on your `package.json`
    

### What’s Next

In upcoming articles, we’ll explore advanced rule generation techniques, including:

* **Rules maintenance.** How to update rules as you develop and learn new patterns
    
* **Creating rules from external sources** using tools like Notebook LLM, Gemini, and content from articles or YouTube videos
    
* **Using Cursor Notepads** to create reusable templates and guides for specific application patterns
    
* **Integrating rule generation** with existing code analysis and documentation tools
    

**Try it yourself**: Implement the Angular meta rule in your project and experiment with the generation prompts. Share your rule variations and let us know what works best for your team’s specific Angular development workflow.