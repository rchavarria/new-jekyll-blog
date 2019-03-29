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

El curso estará basado en el repositorio de GitHub [pwa-training-labs][10]

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

Capítulo sobre [promesas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).
Le echaré un vistazo por encima.

`Promise.all` se resuelve si todas las promesas que le pasamos se resuelven. En
este caso hay que tener cuidado si en alguna de ellas usamos `catch`. `catch`
será llamado si hay algún error, pero `catch` puede devolver una promesa que
resuelva satisfactoriamente, por lo que podemos estar *ocultando* algún error
en `Promise.all` sin querer.

`Promise.race` también es interesante. Este método también acepta un listado
de promesas. El resultado es el mismo resultado de la primera de ellas que
finalize, tanto si resuelve como si es rechazada.

# [Developing PWAs 03.2](https://codelabs.developers.google.com/codelabs/pwa-fetch/index.html?index=..%2F..dev-pwa-training#0)

El API `fetch`, cómo hacer distintas peticiones con `fetch` y usos y limitaciones
de CORS.

`fetch` devuelve una promesa, pero la promesa se resuelve satisfactoriamente
incluso si la respuesta es un `404`. Hay que tener cuidado con este detalle.
Siempre hay que comprobar el código que devuelve la respuesta.

Las respuestas devueltas por `fetch` son [`ReadableStreams`][1], y deben ser
leídos antes de intentar procesarlas. Podemos leerlas como `JSON` con el
método `response.json()`. El objeto `Response` tiene otros [métodos][3].

Las imágenes pueden ser leídas como un *blob*, y luego mostradas en un tag
`img`:

```javascript
function showImage(responseAsBlob) {
  const imgElem = document.createElement('img');
  imgElem.src = URL.createObjectURL(responseAsBlob);
  // add imgElem to a container to show it
}
response.blob()
  .then(blob => showImage(blob))
```

También se puede leer texto con `response.text()`. Y añadirlo a elementos HTML
con `element.textContent = responseAsText`.

También se pueden hacer peticiones con otros *verbos* HTTP, como `HEAD`. Para
ello, hay que pasar un nuevo parámetro a `fetch`: `fetch(url, { method: 'HEAD' })`.

Para hacer una petición `POST` se haría igual y habría que indicar también el
campo `body`:

```javascript
fetch('http://example.com/', {
    method: 'POST',
    body: 'name=david&message=hello'
  })
```

El cuerpo de la petición puede ser los datos de un formulario, utilizando
`FormData`:

```javascript
fetch('http://example.com/', {
    method: 'POST',
    body: new FormData(document.getElementById('my-form'))
  })
```

Hacer peticiones a otros servidores (o a otros puertos) nos causará algún problema
con [CORS][4]. Con `fetch` podemos saltarnos las restricciones pasando un nuevo
campo al parámetro opcional: `fetch(url, { mode: 'no-cors' })`. Al hacer esto,
por temas de seguridad, la respuesta que obtendremos es una [respuesta opaca][5].
El problema de estas respuestas es que no podremos acceder a ellas con
JavaScript (no podremos chequear código respuesta HTTP ni leerla como JSON, blob
o texto), aunque sí podrán ser consumidas por otras APIs o en los Service Workers.

Es posible modificar las cabeceras de las peticiones enviadas por `fetch`:

```javascript
const messageHeaders = new Headers({
  'Content-Type': 'application/json',
  'X-Custom': 'hello world',
})
fetch('http://localhost:5000/', {
  method: 'POST',
  body: JSON.stringify({ lab: 'fetch', status: 'fun' }),
  headers: messageHeaders
})
```

# [Developing PWAs 04.0](https://codelabs.developers.google.com/codelabs/pwa-workbox/index.html?index=..%2F..dev-pwa-training#0)

Este capítulo habla de [Workbox][6], una librería para crear tus propios
Service Workers.

Workbox dispone de muchísimas opciones. El inicio parece sencillo, en nuestro
Service Worker, cargamos Workbox y ahí podemos configurar cachés, rutas,...

Por ejemplo, para las cachés, podemmos indicarle una lista de ficheros a cachear,
con sus hashes, para que Workbox los actualize cuando es debido.

La forma de funcionar de Workbox, es la de generar cierta informacion de configuración
en la fase de construcción del proyecto (en el lab usan gulp). Esa configuración
se usará en tiempo de ejecución para realizar las tareas deseadas.

Además de precachear recursos, el método de Workbox `precacheAndRoute` habilita
un manejador de caché de tipo *cache-first*, es decir, primero se mira si un
recurso está en la caché, si está se sirve, si no, se pide al servidor.

Workbox tiene un módulo de *routing*, para añadir rutas a nuestro Service Worker.

Con estas rutas podemos tener distintas caches, con distintas estrategias y 
distintas políticas de caducidad para cada ruta (tipo de archivo,...). Una ruta
se puede definir por expresiones regulares.

Merece la atención la documentación de Workbox sobre las [estrategias de cacheo][7].
En el lab se utilizan por ejemplo la de cache-first, stale-while-revalidate y
network-first.

Workbox ofrece muchísimas posibilidades, pero parece un poco complicada para
usarla desde el principio de un proyecto. Pero está claro que ahorra mucho
trabajo repetitivo y soluciona muchos aspectos del uso de Service Workers.

# [Developing PWAs 04.1](https://codelabs.developers.google.com/codelabs/pwa-gulp/index.html?index=..%2F..dev-pwa-training#0)

Aquí se repasa la configuración de `gulp` del capítulo anterior. No sé cómo de
interesante será, pero no estoy muy interesado en esta herramienta.

El módulo npm `browser-sync` nos puede servir para crear un servidor web muy
rápidamente.

# [Developing PWAs 04.5](https://codelabs.developers.google.com/codelabs/pwa-caching-service-worker/index.html?index=..%2F..dev-pwa-training#0)

Este capítulo cubre lo básico de cacheo de ficheros con un Service Worker. Las
tecnologías que se verán son el [API de caché][8] y el [API Service Worker][9].

Parece que se verá lo estudiado en el capítulo sobre Workbox, pero esta vez se
implementará *manualmente*. Veamos de qué va.

Lo primero que va a cachear el la *shell de la aplicación*, es decir, los recursos
estáticos mínimos necesarios para que la aplicación arranque. Si no es posible
cachear estos recursos, el Service Worker no se instalará ni activará.

Para ello, en el evento `install` del Service Worker, *esperamos hasta* que todos
los recursos estén cacheados `cache.addAll`:

```javascript
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open('cache-name')
      .then(cache => cache.addAll([ /* ... */ ]))
  );
});
```

Una vez instalado el Service Worker, escucha eventos `fetch`. Implementa la
estrategia *cache first*, o como lo llama en este capítulo
[cache respaldada en la red][11] (mira en la cache, si no existe, lo pide a la
red).

La clave aquí está en [`caches.match`][12], que devuelve una promesa. Si resuelve
con un objecto respuesta, está en la caché. Si resuelve sin objecto respuesta,
no se encuentra en la caché. Si falla, estamos offline.

```javascript
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
    .then(response => {
      if (response) {
        // resource is cached
      }
      
      // resource must be fetched from network
    })
    .catch(error => {
      // we're offline
    })
  );
});
```

No solamente vas a hacer peticiones a la red, también se cachea la respuesta
de la red:

```javascript
fetch(event.request)  // petición a la red
  .then(response => {
    return caches.open('cache-name') // abrir la cache
      .then(cache => {
        cache.put(event.request.url, response.clone());  // añadir respuesta a cache 
        return response;
      });
  });
```

**Nota**: al añadir la respuesta a la cache, hay que clonarla porque son *streams*
y solo pueden ser leídas una sola vez.

¿Qué pasa si la petición a la red falla? ¿Cómo se devuelvería una página de error
`404` personalizada?

```javascript
fetch(event.request)
  .then(response => {
    if (response.status === 404) {
      return caches.match('pages/404.html');
    }
    return caches.open('cache-name').then(/* ... */)
  });
```

Se puede ir teniendo distintas versiones para la cache, o distintas caches para
distintos motivos: `cache-v1`, `cache-v2`, `img-cache-v1`, `css-cache-v10`,...

Es importante borrar las caches que no vayamos a volver a usar, para ahorrar espacio
en los dispositivos de los usuarios.

Este borrado se hace en el evento `activate` del Service Worker. Si se hace antes
de que esté activo, podemos estar borrando caches cuando estamos offline, eliminando
una de las ventajas de Service Worker.

```javascript
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(cacheNames => {
      const deletions = cacheNames
        // filtrar las caches a borrar por el nombre, por lo que debemos conocer
        // los nombres de las caches que habrá y los nombres de las caches que
        // nos interesa mantener
        .filter(filterCachesByName)  
        .map(cacheName => caches.delete(cacheName));
        
      return Promise.all(deletions);
    })
  );
});
```

## References

- [Código fuente del laboratorio][10]
- [Lighthouse]
- [Especificaciones de streams en JavaScript][2]
- [Cache API][8]
- [Service Worker API][9]
- [The offline cookbook][13], de Jake Archibald, donde explica cómo funciona la
cache y estrategias de cacheo y todo sobre caches

[Lighthouse]: https://developers.google.com/web/tools/lighthouse/
[1]: https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream
[2]: https://streams.spec.whatwg.org/
[3]: https://developer.mozilla.org/en-US/docs/Web/API/Response#Methods
[4]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS
[5]: https://stackoverflow.com/questions/39109789/what-limitations-apply-to-opaque-responses
[6]: https://workboxjs.org/
[7]: https://developers.google.com/web/tools/workbox/modules/workbox-strategies#stale-while-revalidate
[8]: https://developer.mozilla.org/en-US/docs/Web/API/Cache
[9]: https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
[10]: https://github.com/google-developer-training/pwa-training-labs
[11]: https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#cache-falling-back-to-network
[12]: https://developer.mozilla.org/en-US/docs/Web/API/Cache/match
[13]: https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/
