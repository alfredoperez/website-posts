---
title: "Notes from Accessibility Workshop from Enterprise NG 2020"
datePublished: Mon Nov 23 2020 15:57:15 GMT+0000 (Coordinated Universal Time)
cuid: clh429vim00010alaakr30hph
slug: notes-from-accessibility-workshop-from-enterprise-ng-2020
canonical: https://dev.to/alfredoperez/learnings-from-accessibility-workshop-from-enterprise-ng-2020-2k57
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682898551749/21918cd7-91d8-48ac-8d0a-70fb26b0b790.png
tags: web-development, angular, accessibility

---

Here are some of the interesting points from the [Build for Accessibility with Angular](https://www.ng-conf.org/2020/sessions/build-for-accessibility-with-angular/)  workshop by [Martine Dowden](https://twitter.com/Martine_Dowden) and [Michael Dowden](https://twitter.com/mrdowden)

- Labels are only for form fields. Do not use labels anywhere else
- Wave can be used to see the page structure

![2020-11-23_9-17-59](https://cdn.hashnode.com/res/hashnode/image/upload/v1681999831317/8dcb3e37-133f-44dc-ab1a-a4c75fa9ed8b.png)
 
- AXE provides detailed information and links about the problem and possible resolutions

![image](https://cdn.hashnode.com/res/hashnode/image/upload/v1681999832704/6be73804-fbe5-46e0-af8f-d3678ca6b113.png)

- Use unit tests that test for accessibility-related attributes like roles and aria labels
  
```ts
it('icon butttons should have aria labels', ( ) => {
 const iconButtons = fixture.debugElement.nativeElement.querySelectorAlK'button[mat-icon-button]'); 
 const missingLabels => Array.from(iconButtons).some((button: HTMLElement) => ¡button.getAttribute('aria-label'); 
 expect(missingLabels).toBeFalsy();
});

it('mat list should have a role of list', () => {
 const list = fixture.debugElement.nativeElement.querySelector('mat-list'); 
 expect(list.getAttribute('role*)).toEqual('list');
});
```

- Prefer the use of the attribute `type="submit"` for the buttons in the form instead of calling the method to submit from the `click` handler

![image (1)](https://cdn.hashnode.com/res/hashnode/image/upload/v1681999833936/00bacd70-ab4d-4084-bc96-fd5b0a297a05.png)

- Prefer to enable/disable instead of showing/hiding when it is needed to show data depending on a condition. This helps to avoid confusing the user and having UI jumping around.

- Links are links. Buttons are buttons. Div and spans are not buttons nor links.  Divs and spans miss some accessibility features for example they are not focusable and cannot be disabled.

- When having a link that is only an icon, use the `aria-hidden="true"` and add a label

```html
<a href="profile" 
   aria-label="Profile">
 <i aria-hidden="true"
    classss="material-icons">
   Face
 </i>
</a>
```

- You should always have the `alt` attribute on images, however, it can be blank. The `alt` should describe what the image is trying to convey. 

- Decorative images can have an empty `alt` attribute or have a `role="presentation"`

- In alerts or notifications use `role="alert"`, also use  `aria-live="polite"` for status notifcaions and `aria-live="assertive"` for something that requires attention 
 
## Chrome Developer Tools 

- There is an option for accessibility in the "[Audits](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference#audits)" tab
- There is an accessibility tab in Chrome that contains the [accessibility tree](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference), the [ARIA attributes](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference#aria), and the accessibility-related computed attributes


## Angular Material

- Make sure that the custom color pass the contrast

![2020-11-23_9-17-23](https://cdn.hashnode.com/res/hashnode/image/upload/v1681999835296/42007cf0-b6dd-432f-bc7b-09685c9d3515.png)

## Links
- [AXE](https://www.deque.com/axe/)
- [WAVE](https://wave.webaim.org/)
- [Accessibility Insight](https://accessibilityinsights.io/en/)
- [Angular theming guide](https://material.angular.io/guide/theming)
- [Angular typography guide] (https://material.angular.io/guide/typography)
- [Google Fonts](https://fonts.google.com/)
- [Webaim contrast checker](https://webaim.org/resources/contrastchecker/) 
- [Color palette builder](http://mcg.mbitson.com) 
- [Theme builder](https://materialtheme.arcsine.dev/)
 