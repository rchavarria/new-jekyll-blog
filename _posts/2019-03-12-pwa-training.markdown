---
title: "Progressive web apps training"
date: 2019-03-12 22:39
author: Ruben Chavarria
categories:
- Cursos
- Web
---

Notas tomadas de un curso de Google Chrome Developers titulado
[Progressive web apps training](https://www.youtube.com/watch?list=PLNYkxOF6rcIB2xHBZ7opgc2Mv009X87Hh).
Según el primer episodio, el formato del curso será algo así como: nosotros te
mostramos algo, tú creas alguna cosa, repetimos.

Esa lista de vídeos en YouTube son material de apoyo del curso de Google
Developers [Developing Progressive Web Apps (PWAs) Course](https://codelabs.developers.google.com/dev-pwa-training/).
Primero, comencé siguiendo los vídeos de la playlist, pero una vez descubierto
el *codelabs* es lo que voy a seguir.

No tengo ni idea de qué saldrá de aquí, no tengo ni idea si el curso está bien
o no. Pero aquí iré contando lo que vaya creando, las notas que vaya tomando,
las ideas que vaya teniendo.

<!-- more -->

# [Developing PWA 01.0](https://codelabs.developers.google.com/codelabs/pwa-welcome/index.html?index=..%2F..dev-pwa-training#0)

El curso estará basado en el repositorio de GitHub [pwa-training-labs]

# [Developing PWA 02.0](https://codelabs.developers.google.com/codelabs/pwa-offline-quickstart/index.html?index=..%2F..dev-pwa-training#0)

Vamos a estar usando [Lighthouse] durante todo el curso.

### Registrar un Service Worker

```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('service-worker.js')
      .then(reg => {});
  });
}
```

1. Comprobar que el navegador soporta los Service Workers
2. Esperar a que la página esté cargada
3. Registrar el Service Worker

### Precachear recursos

Uno de los requisitos de las PWAs es el de devolver `HTTP 200` incluso cuando
estemos offline. Para ello, hay que precachear los recursos mínimos que
necesitemos:

```javascript
const cacheName = 'cache-v1';
const precacheResources = [ /* fill in whatever you need */];

self.addEventListener('install', event => {
  console.log('Service worker install event!');
  event.waitUntil(...);
});

self.addEventListener('activate', event => {
  console.log('Service worker activate event!');
});

self.addEventListener('fetch', event => {
  console.log('Fetch intercepted for:', event.request.url);
  event.respondWith(...);
});
```

La primera vez que se carga la página, el Service Worker se registra, instala y
(si no hay ningún Service Worker más) se activa.

El SW no puede cachearse a sí mismo, si no, nunca podríamos actualizarlo, y los
clientes se quedarían con la misma versión para siempre.

`self` en un SW se refiere a sí mismo, es el objeto global.

`install`, `activate` y `fetch` son eventos interesantes a los que tendremos
que responder.

### Añadir la posibilidad de instalar en el dispositivo

Necesitaremos un fichero *Manifest*, definir un *splash screen* y los colores de
la barra de direcciones.

A partir de Chrome 68, la instalación de una PWA debe hacerse programáticamente.
Añadiremos un script, escuchando el evento `beforeinstallprompt`, el cual, tiene
un método `prompt` al que podremos llamar cuando el usuario pinche en un botón
determinado. Con el resultado (al resolverse una promesa), ya podremos saber
si el usuario ha instalado la PWA o no.

# [Developing PWA 03.0](https://codelabs.developers.google.com/codelabs/pwa-scripting-the-service-worker/index.html?index=..%2F..dev-pwa-training#0)



## References

- [Lighthouse]

[Lighthouse]: https://developers.google.com/web/tools/lighthouse/
[pwa-training-labs]: https://github.com/google-developer-training/pwa-training-labs
