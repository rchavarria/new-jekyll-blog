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

El Service Worker no puede cachearse a sí mismo, si no, nunca podríamos actualizarlo, y los
clientes se quedarían con la misma versión para siempre.

`self` en un Service Worker se refiere a sí mismo, es el objeto global.

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

En este módulo parece que se ve el funcionamiento del Service Worker en más
profundidad.

En las Chrome Dev Tools, en la pesaña de *Application*, podemos ver los
Service Workers instalados por dominio. También podemos ver los recursos
cacheados en *Cache Storage*.

El registro se hace a través del método `register`, de la
[API Service Worker Container](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer).
Este método devuelve una promesa. Si el navegador soporta la tecnología, la
promesa se resolverá satisfactoriamente.

### Ciclo de vida

Las fases serían: registrar, instalar, activar.

En el código de nuestro Service Worker podemos escuchar los eventos `install` y `activate`.

En el momento de registrar el Service Worker, se comprueba si es nuevo o distinto del
ya instalado anteriormente. Si es así, se instala, con lo que se lanza el
evento `install`.

El evento `install` es ideal para **cachear** recursos estáticos.

El evento `activate` se lanza cuando el Service Worker toma el control de la página.
Este evento es ideal para **actualizar** recursos cacheados.

Solo un Service Worker puede tener el control de la página. Para transferir el control a un
nuevo Service Worker no vale simplemente con refrescar la página, porque la nueva página
se pide antes de eliminar la anterior, con lo que el Service Worker no se desinstala
a tiempo. Para forzar la activación de un Service Worker tenemos dos opciones:

1. Cerrar todas las pestañas asociadas al Service Worker y volver a empezar
2. Programáticamente, con [skipWaiting](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)

Este método se puede llamar en el evento `install`, en el fichero de nuestro
Service Worker:

```javascript
self.addEventListener('install', event => {
  // ...
  self.skipWaiting();
});
```

El método se puede llamar en cualquier momento antes o durante la fase de
espera a la activación.

### Interceptar peticiones de red

Los Service Worker pueden funcionar como un proxy entre nuestra aplicación web y la red.

Para ello, podemos escuchar eventos de tipo `fetch`. Escuchar estos eventos en
los Service Worker es como escuchar `click` en el DOM.

La primera vez que se carga la página, ninguna petición emitirá un evento
`fetch`. ¿Por qué? Por consistencia, si la petición de una página no pasa
por un Service Worker, ninguna de sus siguientes peticiones pasará por el Service Worker.

### Ámbito de un Service Worker

El ámbito (scope) de un Service Worker determina desde qué rutas intercepta peticiones.

Puedes conocer el ámbito de un Service Worker mediante `registration.scope`, donde
`registration` es el resultado al resolverse la promesa al registrarse el
Service Worker:

```javascript
navigator.serviceWorker.register('service-worker.js')
  .then(registration => {
    console.log('Service Worker registered with scope:', registration.scope);
  })
```

El ámbito por defecto es el directorio del propio Service Worker. Por eso, se
suelen poner en la raiz del proyecto. Pero el ámbito se puede modificar si
pasamos un parámetro a la hora de registrarlo:

```javascript
navigator.serviceWorker.register('service-worker.js', { scope: '/below/' })
  .then(registration => {
    console.log('Service Worker registered with scope:', registration.scope);
  })
```

# [Developing PWAs 03.1](https://codelabs.developers.google.com/codelabs/pwa-promises/index.html?index=..%2F..dev-pwa-training#0)

Capítulo sobre promesas y el API `fetch`. Le echaré un vistazo por encima.

## References

- [Lighthouse]

[Lighthouse]: https://developers.google.com/web/tools/lighthouse/
[pwa-training-labs]: https://github.com/google-developer-training/pwa-training-labs
