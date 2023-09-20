---
title: "Prettifying HTML in Angular"
seoTitle: "Using Prettier in Angular HTML"
seoDescription: "Improve Angular HTML readability using prettier-plugin-organize-attributes; sort attributes, boost code reviews, supports Vue, React, Angular"
datePublished: Tue Sep 19 2023 13:19:05 GMT+0000 (Coordinated Universal Time)
cuid: clmqcd40y000009l4g45mdzry
slug: prettifying-html-in-angular
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695129281030/74e435f3-44ec-4da6-8c9a-81929138bdc3.png
tags: angularjs, developer, html5, prettier

---

**Do you have a hard time reading the HTML of your components?** You are not alone, and many factors can contribute to this. However, today I will show you a Prettier plugin that will help to organize the HTML attributes and hopefully make it easier to read and understand everything that is going on with your components.

## Say hello 👋 to the `prettier-plugin-organize-attributes`

When using [Prettier](https://prettier.io/), any HTML line that is longer than the `printWidth` setting will be split into multiple lines, one line for each attribute, helping to make the HTML cleaner and readable.

Having a standard order for the HTML attributes can also help us quickly find out what the most important parts of an element are since we will be used to looking for attributes in the same order in every component.

For this, we will use the plugin `prettier-plugin-organize-attributes` that, fortunately, helps us set the order for HTML attributes and also supports Vue, React, and Angular.

### Installing and configuring the library

Simply run the following command to install the library:

```shell
npm i prettier prettier-plugin-organize-attributes -D
```

Now in the prettier configuration file, add the following configuration:

```json
attributeGroups: [  
 '^(id|name)$',  
 '$ANGULAR_STRUCTURAL_DIRECTIVE',  
 '^app', 
 '$ANGULAR_ELEMENT_REF',  
 'data-testid',  
 'tabindex',  
 '$ALT',  
 '$ARIA',  
 '$ROLE',  
 '$TYPE',  
 '$CLASS',  
 'ngClass',  
 '^\\[style',  
 '$ANGULAR_ANIMATION',  
 '$ANGULAR_ANIMATION_INPUT',  
 '^\\[attr',  
 '$ANGULAR_TWO_WAY_BINDING',  
 '$ANGULAR_INPUT',  
 '^(\\(blur\\)|\\(focus\\)|)$',  
 '^(\\(click\\)|\\(dbclick\\)|\\(submit\\))$',  
 '^(\\(cut\\)|\\(copy\\)|\\(paste\\))$',  
 '^(\\(keyup\\)|\\(keypress\\)|\\(keydown\\))$',  
 '^(\\(mouseup\\)|\\(mousedown\\)|\\(mouseenter\\)|\\(scroll\\))$',  
 '^(\\(drag\\)|\\(drop\\)|\\(dragover\\))$',  
 '$ANGULAR_OUTPUT'  
],
```

Let's break out why this order was chosen so you can rearrange and adapt as you need.

#### Invalid Attributes

```json
'^(id|name)$',
```

The first line is to bring to the top any HTML attributes that you don't allow to make it easier to find them during code reviews.

#### Angular Structural Directives

```JSON
'$ANGULAR_STRUCTURAL_DIRECTIVE',
```

Commonly used directives like `*ngIf` and `*ngFor`, as well as any directives that may alter or add additional elements to the DOM.

#### Custom Directives

```JSON
'^app',
```

Since the directives also might change presentation is good to have them close to the first items to easily identify that the current HTML is affected by a directive. In our case, we use `app` selector for our directives and that is why we use `'^app'`,

#### Element Ref

```JSON
 '$ANGULAR_ELEMENT_REF',
```

#### Test Ids

```json
 'data-testid',
```

This includes any HTML attribute to select HTML elements while executing tests like `data-testid` or `data-cy`

#### HTML Accessibility Attributes

```json
 'tabindex',  
 '$ALT',  
 '$ARIA',  
 '$ROLE',  
 '$TYPE',
```

#### Angular Animations

```json
 '$ANGULAR_ANIMATION',  
 '$ANGULAR_ANIMATION_INPUT',
```

#### Stylistic Attributes

```json
 '$CLASS',  
 'ngClass',  
 '^\\[style',
```

This would include any attributes that directly affect how the element is styled, such as `class`, `[ngStyle]`, and `[ngClass]`.

After knowing that the element will be in the DOM, the next logical thing is knowing how the element will look on the page. This rule includes any conditional classes added with Angular data binding syntax like `[class.some-class]="true"`, which is just an alternative to `ngClass` and `ngStyle`.

#### Angular Accessibility Attributes

```json
 '^\\[attr',
```

Anything that can be modified with Angular using `[aria.`

#### Angular Inputs and outputs

```json
 '$ANGULAR_TWO_WAY_BINDING',  
 '$ANGULAR_INPUT',
 '^(\\(blur\\)|\\(focus\\)|)$',  
 '^(\\(click\\)|\\(dbclick\\)|\\(submit\\))$',  
 '^(\\(cut\\)|\\(copy\\)|\\(paste\\))$',  
 '^(\\(keyup\\)|\\(keypress\\)|\\(keydown\\))$',  
 '^(\\(mouseup\\)|\\(mousedown\\)|\\(mouseenter\\)|\\(scroll\\))$',  
 '^(\\(drag\\)|\\(drop\\)|\\(dragover\\))$',  
 '$ANGULAR_OUTPUT'
```

This includes standard element attributes, inputs, outputs, and DOM event handlers.

This configuration makes sure that all the DOM Event handlers are grouped in a logical way to make it easier to find counterparts, like, for example, `(copy)`, `(cut)` and `(paste)` will be close together regardless of which other outputs exist.

#### DOM Event Handlers

Finally, include any DOM event handlers, such as `(mouseenter)`, `(copy)`, `(keyup)`, etc.

## Putting it all together

Now, let's see how this will work if, instead of relying on code reviews, we change it to use the library `prettier-plugin-organize-attributes`

Let's give it a try in this component that has way to many attributes but that can help us see how the plugin works when the HTML element has a lot of different attribute types

%[https://snappify.com/view/0e7dd090-f28c-41ec-b0f7-881e293ce30e]