---
title: "View Transitions API en Angular 17"
datePublished: Wed Nov 22 2023 14:10:18 GMT+0000 (Coordinated Universal Time)
cuid: clp9udhqs001z0al5e14b27p5
slug: view-transitions-api-en-angular-17
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1700662196078/875964e8-dd8c-406a-87ed-17302b1ace05.png
tags: webdesign, web-development, angular, web-animation, angular17

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700519671858/63661c0c-a94f-4c1a-b421-d48541b054cc.png align="center")

🚀 La versión 17 de Angular está a la vuelta de la esquina e incluirá algo que está causando sensación en el mundo del desarrollo web: el soporte para [View Transitions API.](https://developer.chrome.com/docs/web-platform/view-transitions/)

Las View Transitions API hacen que sea muy sencillo añadir animaciones llamativas y transiciones suaves cuando estás construyendo aplicaciones web. Puedes crear efectos llamativos con solo un poco de código, lo que hace que tu aplicación sea más atractiva para los usuarios.

Además, no se trata solo de hacer que las cosas luzcan elegantes 🎩. Esta API ayuda a los usuarios a entender cómo se conectan diferentes partes de su aplicación, e incluso puede hacer que las cosas parezcan más rápidas cuando se están cargando datos. Así que no se trata solo de hacer que tu aplicación luzca bien; se trata de hacer que funcione mejor para tus usuarios también. 💪

[Smooth and simple transitions with the View Transitions API - Chrome for Developers](https://developer.chrome.com/docs/web-platform/view-transitions/?source=post_page-----1d1ea8bb2703--------------------------------)

## **Habilitar ViewTransitions en las rutas**

Para habilitarlo, solo necesitas un paso. ¡Solo una cosa! Debes agregar `withViewTransitions` a `provideRouter` en tu `ApplicationConfig`*:*

```plaintext
export const appConfig: ApplicationConfig = {
  providers: [
    provideAnimations(),
    provideRouter(
        routes, 
        withViewTransitions(),   // 👈
        withComponentInputBinding()
   )]
};
```

Luego puedes configurar las animaciones de ruta como lo haces normalmente (o seguir esta [guía](https://angular.io/guide/route-animations#enable-routing-transition-animation)), pero si observas detenidamente, notarás que el pseudo-elemento para las View Transitions se agrega al DOM cuando ocurre la animación:

![](https://miro.medium.com/v2/resize:fit:770/0*hY7sCJqsN4EglbU7.png align="left")

Además, si vas a “Animations” en las Herramientas para Desarrolladores de Chrome, ahora puedes depurar la View Transitions API:

![](https://miro.medium.com/v2/resize:fit:770/0*Rox3psp6MnUs-XTo.gif align="left")

## **Utilizando transiciones para mostrar el flujo del usuario**

Ahora, hagamos algo en lo que guiemos al usuario a través de una transición al navegar de una página principal a una página secundaria.

En el siguiente ejemplo, verás cómo la portada del libro realiza una transición a su nueva ubicación, indicándonos que estamos ingresando a la página de detalle.

![](https://miro.medium.com/v2/resize:fit:770/0*HT2O67_Tt5OZ_H-r.gif align="left")

Me inspiré para hacer este ejemplo a partir de un video de [Midudev](https://twitter.com/midudev?lang=en) en el que muestra cómo Astro 3.0 agrega soporte para las View Transitions API, pero sinceramente, creo que usar esta nueva API en Angular es mucho más fácil, ya que solo tienes que agregarla al enrutador y este sabe qué hacer. Puedes ver el video original aquí.

[https://youtu.be/ODv-VFRFGcY](https://youtu.be/ODv-VFRFGcY)

Para agregar esta funcionalidad en Angular, solo necesitamos agregar el mismo nombre en `view-transition-name` para cada libro en las dos páginas que lo utilizan.

Para hacerlo, mapeé el ID a una nueva propiedad:

```plaintext
books.map(book books.map)
 {
    ...book,
     viewTransitionName: `view-transition-name: book-${book.id}`
 }
));
```

Después, lo agregas al elemento de la imagen en ambas páginas:

```plaintext
<picture class="mb-8 w-full relative">
 <img
    class="aspect-[389/500]rounded"
    [ngSrc]="book.img"
    width="389"
    height="500"
    [style]="book.style"
 />
</picture>
```

# **Resumen**

En este artículo, discutimos el próximo soporte para la View Transitions API en Angular version 17, que permite a los desarrolladores agregar fácilmente animaciones y transiciones suaves en aplicaciones web.

Puedes agregarlo en cuestión de segundos con un par de líneas de código, y de inmediato podrás tener una elegante aplicación web.

Puedes probar el código aquí: [https://github.com/alfredoperez/angular-17-demos](https://github.com/alfredoperez/angular-17-demos)

👇👇 Sígueme en X 👇👇

[@alfrodo\_perez](https://twitter.com/alfrodo_perez)