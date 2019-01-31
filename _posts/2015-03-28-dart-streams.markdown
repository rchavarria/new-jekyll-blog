---
title: "Dart Streams"
date: 2015-03-28 01:39
author: Rubén Chavarría
comments: true
categories: 
- dart
- streams
published: true
footer: false
sidebar: true
---

Siendo Dart un lenguaje muy influenciado por JavaScript, no podría dejar de lado
conceptos como la **programación asíncrona**. Existen varios métodos para manejar
la asincronía, y uno de ellos son los **Streams**. Si estás muy interesado en
el método te recomiendo una charla de [Rod Vagg], [Embrace the asynchronous].

Los *Streams* en Dart forman una interfaz única para cualquier cosa que necesite
enviar repetidamente una serie de datos de forma asíncrona, ya sean eventos HTML
(como escuchar clicks del usuario), o eventos de entrada/salida en una aplicación
de servidor.

Operaciones sobre *Streams*:

- Consumir: los datos son sacados de un `Stream` a uno o varios `StreamSubscriber`
- Producir: los datos son introducidos en un `Stream` desde un `StreamController`

<!-- more -->

## Consumiendo un `Stream`

En lugar de introducir datos al Stream a través de un `StreamController`,
que podría ser algo largo de contar, vamos a utilizar el constructor
`Stream.fromIterable()`, el cual nos permite construir un `Stream` a partir de
una lista de objetos, por ejemplo.

Típicamente, se usa el método `listen()` para subscribirse a un `Stream`. Este método
es llamado cada vez que se recibe un dato:

```
var data = [1, 2, 3, 4, 5];
var stream = new Stream.fromIterable(data);

// subscribe to the streams events
stream.listen((value) {
  print("Received: $value");
});
```

`Stream` también tiene otros métodos: `first`, `last`, `length` y `isEmpty`. Todos
ellos devuelven un `Future`, como los que estudiamos en [Futures con Dart], el cual
se completará con el valor apropiado dentro del `Stream`:

```
streamProperties() {
  var stream;

  stream = new Stream.fromIterable([1, 2, 3, 4, 5]);
  stream.first.then((value) => print("stream.first: $value"));  // 1

  stream = new Stream.fromIterable([1, 2, 3, 4, 5]);
  stream.last.then((value) => print("stream.last: $value"));  // 5  

  stream = new Stream.fromIterable([1, 2, 3, 4, 5]);
  stream.isEmpty.then((value) => print("stream.isEmpty: $value")); // false

  stream = new Stream.fromIterable([1, 2, 3, 4, 5]);
  stream.length.then((value) => print("stream.length: $value")); // 5
}
```

También se pueden tener varios listeners, pero para eso hay que convertir el `Stream`
en un stream de broadcast con `asBroadcastStream()`. Podremos comprobar de qué tipo
es un stream con la propiedad `isBroadcast`.

```
var data = [1, 2, 3, 4, 5];
var stream = new Stream.fromIterable(data);
var broadcastStream = stream.asBroadcastStream();

broadcastStream.listen((value) => print("stream.listen: $value")); 
broadcastStream.first.then((value) => print("stream.first: $value"));
//...
```

## Subconjuntos de datos de un `Stream`

Los `Stream`s tienen algunos métodos de utilidad que permiten seleccionar un subconjunto
de los datos que vendrán en él. Cada uno de estos métodos devuelve un
`Stream` al que podemos registrar un listener. Para una lista completa de estos métodos
se puede consultar la documentación oficial: [API Streams].

`where`: selecciona aquellos datos que cumplen una condición:

```
broadcastStream
    .where((value) => value % 2 == 0) 
    .listen((value) => print("where: $value"));
```

`take`: toma solo los `n` primeros elementos:

```
broadcastStream
    .take(3) 
    .listen((value) => print("take: $value"));
```

`skip`: se salta los `n` primeros elementos:

```
broadcastStream
    .skip(3)
    .listen((value) => print("skip: $value"));
```

`takeWhile`: va tomando datos mientras la condición sea verdadera:

```
broadcastStream
    .takeWhile((value) => value < 3) 
    .listen((value) => print("takeWhile: $value"));
```

`skipWhile`: va saltando datos mientras la condición sea verdadera:

```
broadcastStream
    .skipWhile((value) => value < 3)
    .listen((value) => print("skipWhile: $value"));
```

## Transformando streams

El método `Stream.transform()` admite un parámetro de tipo `StreamTransformer`.
Es posible crear uno con el método `fromHandlers()`, el cual admite un
método como parámetro. Este método se llamará con 2 parámetros: cada uno de
los valores del `Stream` original, y un `StreamSink` al cual podremos añadir
el valor transformado. La salida de `transform()` es un nuevo `Stream` cuyos
valores han sido transformados por el `StreamTransformer`.

Todo esto se ve mucho más claro con un poco de código:

```
var transformer = new StreamTransformer.fromHandlers(handleData: (value, sink) {
  // create two new values from the original value
  sink.add("Message: $value");
  sink.add("Body: $value");
});
    
// transform the stream and listen to its output
stream.transform(transformer).listen((value) => print("listen: $value"));
```

Un ejemplo de la vida real podría ser la lectura de un fichero o de una
petición HTTP, transformando los datos recibidos a `String` con el decodificador
`UTF8.decoder()` del paquete `dart:convert`.

```
File file = new File("some_file.txt");
file.openRead()
    .transform(UTF8.decoder) // use a UTF8.decoder
    .listen((String data) => print(data));
```

## Validando los valores de un `Stream`

Otros métodos útiles podrían ser `any()`, `every()` y `contains()`, los cuales
devuelven todos `Future<boolean>`, es decir, un `Future` que se completa con un
valor de `true` o `false`. 

Estos métodos servirían para realizar ciertas comprobaciones sobre los datos
recibidos por el `Stream`. Siguiendo con el ejemplo que crea un `Stream` a partir
de un array con los valores del 1 al 5:

```
broadcastStream
    .any((value) => value < 5)
    .then((result) => print("Any less than 5?: $result")); // true
  
broadcastStream
    .every((value) => value < 5)
    .then((result) => print("All less than 5?: $result")); // false
  
broadcastStream
    .contains(4)
    .then((result) => print("Contains 4?: $result")); // true
```

## Gestión de errores con `StreamSubscription`

Existen dos alternativas para la gestión de errores en los streams. Usando el
objeto `StreamSubscription` que retorna `listen()` o pasando los manejadores de
eventos al propio método `listen()`.

Usando `StreamSubscription` devuelto por `listen()`:

```
var subscription = stream.listen(null);
subscription.onData((value) => print("listen: $value"));
subscription.onError((err) => print("error: $err"));
subscription.onDone(() => print("done"));
```

Pásandolos como parámetros:

```
var subscription = stream.listen(
    (value) => print("listen: $value"),
    onError: (err) => print("error: $err"),
    onDone: () => print("done")
    );
```

## Eliminando la suscripción a un `Stream`

Con el método anterior de obtener un objeto `StreamSubscription`, podemos
utilizar dicha referencia para cancelar la suscripción con el método `cancel()`.

```
var subscription = stream.listen(null);
subscription.onData((value) {
  print("listen: $value");
  if (value == 2) subscription.cancel();
});
```

## Referencias:

- [Embrace the asynchronous]
- [Futures con Dart], en este propio blog
- [API Streams]

[Embrace the asynchronous]: https://www.youtube.com/watch?v=oeKkwZhKgP0
[Rod Vagg]: http://r.va.gg
[Futures con Dart]: /blog/2015/01/29/operaciones-asincronas-en-dart-con-futures/
[API Streams]: http://api.dartlang.org/dart_async/Stream.html
