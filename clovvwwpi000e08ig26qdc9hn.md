---
title: "Using Prettier in Angular HTML"
seoTitle: "Prettier for Angular HTML Optimization"
seoDescription: "Enhance Angular HTML with `prettier-plugin-organize-attributes` for ordered attributes, quicker navigation. Supports Vue, React, Angular"
datePublished: Tue Sep 19 2023 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clovvwwpi000e08ig26qdc9hn
slug: using-prettier-in-angular-html
canonical: https://medium.com/ngconf/using-prettier-in-angular-html-52efa7af625b
tags: angularjs, web-development, html5, prettier

---

![](https://miro.medium.com/v2/resize:fit:1100/0*0RuQIgxUR9cRGP9e align="left")

**Do you have a hard time reading the HTML of your components?**

You are not alone, and many factors can contribute to this. However, today I will show you a prettier plugin that will help to organize the HTML attributes and hopefully make it easier to read and understand everything that is going on with your components.

# **Say hello 👋 to the** `prettier-plugin-organize-attributes`

Having a defined order for the HTML attributes can also help us quickly find out what the most important parts of an element are since we will be used to looking for attributes in the same order in every component.

![](https://miro.medium.com/v2/resize:fit:770/1*c6ov_fIGHCMt70Rjh8VrIg.gif align="left")

For this, we will use the plugin`prettier-plugin-organize-attributes` that, fortunately, helps us set the order for HTML attributes and also supports Vue, React and Angular.

# **Installing and configuring the library**

Simply run the following command to install the library:

```plaintext
npm i prettier prettier-plugin-organize-attributes -D
```

Now in the prettier configuration file, add the following configuration:

```plaintext
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

Let’s break out why this order was chosen so you can rearrange and adapt as you need.

## **Invalid Attributes**

```plaintext
'^(id|name)$',
```

The first line is to bring to the top any HTML attributes that you don’t allow to make it easier to find them during code reviews.

## **Angular Structural Directives**

```plaintext
'$ANGULAR_STRUCTURAL_DIRECTIVE',
```

Commonly used directives like `*ngIf` and`*ngFor`, as well as any directives that may alter or add additional elements to the DOM,

## **Custom Directives**

```plaintext
'^app',
```

Putting the directives close to the first items is best because they may also change the way things look, which makes it easy to see when a directive is changing the HTML. In our case, we use `app` selector for our directives and that is why we use `'^app'`,

## **Element Ref**

```plaintext
'$ANGULAR_ELEMENT_REF',
```

## **Test Ids**

```plaintext
'data-testid',
```

This includes any HTML attribute to select HTML elements while executing tests, like `data-testid` or `data-cy`

## **HTML Accessibility Attributes**

```plaintext
 'tabindex',  
 '$ALT',  
 '$ARIA',  
 '$ROLE',  
 '$TYPE',
```

## **Angular Animations**

```plaintext
'$ANGULAR_ANIMATION',
'$ANGULAR_ANIMATION_INPUT',
```

## **Stylistic Attribute**

```plaintext
 '$CLASS',  
 'ngClass',  
 '^\\[style',
```

This would include any attributes that directly affect how the element is styled, such as `class`, `[ngStyle]`, and `[ngClass]`.

After knowing that the element will be in the DOM, the next logical thing is to know how the element will look on the page. This rule includes any conditional classes added with Angular data binding syntax, like`[class.some-class]="true"`, which is just an alternative to `ngClass` and`ngStyle`.

## **Angular Accessibility Attributes**

```plaintext
 '^\\[attr',
```

Anything that can be modified with Angular using `[aria.`

## **Angular Inputs and outputs**

```plaintext
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

This configuration makes sure that all the DOM Event handlers are grouped together in a logical way to make it easier to find counterparts, like, for example, `(copy)`, `(cut)` and `(paste)` will be close together regardless of which other outputs exist.

## **DOM Event Handlers**

Finally, include any DOM event handlers, such as `(mouseenter)`, `(copy)`, `(keyup)`, etc.

# **Putting it all together**

The picture below shows how using it on a component with too many attributes can get them all in order. This way, we can train ourselves to find inputs and outputs quickly because we know they will always be in the same spot.

![](https://miro.medium.com/v2/resize:fit:770/1*DXlD6wgB7NwkxPt-NhhxVA.png align="left")

👇👇 Follow me on Twitter 👇👇

Alfredo