---
layout: post
title: "WeCodeFest 2019"
date: 2019-02-25 21:12
author: Ruben Chavarria
categories: 
- Eventos
---

El año pasado ya me quedé con las ganas de asistir al [WeCodeFest]. Mucha gente
a la que respeto por el trabajo que hacen, hablaba muy bien de una conferencia
nueva, en Valladolid. Bueno, no era tanto como una conferencia, era un evento 
más enfocado en la práctica, en arremangarse y probar cosas, hacer cosas. Y eso 
me gusta bastante, pero no pudo ser.

Este año sí, y ha sido super especial, me ha encantado. Se puede decir que es
un evento *pequeño*, organizado con mucho amor y mucho atención al detalle, y
sobretodo práctico. Lo más interesante no son las charlas programadas, si no
los talleres o las conversaciones planteadas en el open space.

![WeCodeFest 2019]({{ site.baseurl }}/assets/images/2019/wecode.jpg)

Enlace: [No sé, donde sea](http://google.es)

<!-- more -->

## Talleres

¿He dicho ya que lo más interesante eran los talleres? Aquí están aquellos
a los que asistí:

#### Introducción a Property Based Testing

Taller sobre testing automático impartido por [Sergio Arroyo Cuevas].
[Property Based Testing] se basa en ejecutar varias veces un mismo test,
usando datos aleatorios como entrada a nuestro código. De esta forma, trata
de encontrar casos límites (números muy grandes, o negativos, cadenas con un
formato peculiar,...) donde nuestro código puede fallar.

El taller estuvo muy bien organizado, en bloques fijos de tiempo, para
asegurar cierto avance a los asistentes. Comenzó suave, tomando contacto con
las librerías, y terminó probando código ajeno al taller, para demostrar lo
fácil/complicado que sería usar este tipo de testing en nuestro día a día.

Se podían utilizar distintos lenguajes, yo usé JavaScript. Puedes encontrar
las instrucciones y enlaces a cada uno de los lenguajes disponibles en el
repositorio [WeCodeProperties] de Sergio.

#### Programación cuántica para gente que programa

Taller impartido por [Salvador de la Puente] sobre un tema bastante desconocido
para mí, programar ordenadores cuánticos. Según trató de explicarnos Salva,
*no se trata de ordenadores más rápidos, sino de una forma de operar 
esencialmente distinta*.

El taller fue muy, muy práctico, y pudimos aprender los principales fenómenos 
de la computación cuántica tales como superposición, entrelazamiento e
interferencia. Lo de comprenderlos 100% lo dejé para otro día. No es que sean
conceptos complicadísimos de comprender, pero sí que son conceptos que uno
necesita reposar un tiempo.

Aún así, jugar con el simulador [Quirk] e implementar algunos algoritmos
cuánticos (puertas lógicas o pequeños cálculos) fue muy divertido.

También me sirvió para repasar conceptos básicos de computación, como lógica
booleana o las clásicas puertas lógicas.

#### Aplicación serverless en 2 horas, de Vicenç García Altés

Taller impartido por [Vicenç García Altés] donde el objetivo era desarollar
una aplicación para una universidad con servicios de AWS, donde la lógica
de negocio estuviera implementada a través de *Functions*/*Lambdas* en AWS.

El taller está basado en el repositorio [taller serverless] de Vicenç. En
él, hay mucho contenido, repito, **mucho** contenido. Se nota que hay un
inmenso trabajo detrás del taller. Se nota porque hay que integrar muchos
servicios y el taller está enfocado en tests, despliegues automáticos y otras
buenas prácticas que no suelen estar presentes en otros talleres.

En realidad, es un taller bastante más extenso de horas, de hecho, Vicenç
acaba de hacer público el taller [The Serverless Course], por si te quieres
apuntar.

## Open Spaces

En el open space salieron unas cuantas charlas/conversaciones interesantes. No
pude ir a todas las que me interesaban, aquí están las notas de las que sí fui:

#### Testing en front-end

No recuerdo quién la propuso.

Empezamos hablando sobre los distintos tipos de tests, y la conversación se
enfocó sobretodo en los tests end-to-end, donde todos coincidíamos en que son
los más costosos de escribir y ejecutar.

Luego, la conversación se dirigió hacia los equipos de QA, donde salió la idea
de que no debería haber equipos de QA separados de Desarrollo.

#### Desarrollo de software en provincias

Propuesta por [Azahara Fernández], aquí llegué un poquito tarde.

Se habló de coste en tiempo y energía personal que tienen los largos trayectos
de casa al trabajo y del trabajo a casa en las grandes ciudades, y del
ahorro en tiempo que hay al trabajar en ciudades más pequeñitas, donde puedes
estar en el trabajo en 15 minutos o menos.

También se habló de lo difícil que es, para las empresas tecnológicas, encontrar
talento en ciudades modestas. Luego, la conversación giró hacia la creación
de empresas, y se expuso que en el caso de Salamanca podemos encontrar varios
casos de pequeñas empresas cosiguiendo grandes cosas.

Esto me alegró bastante, trabajo en los alrededores de Madrid, pero me gusta
vivir en un lugar tranquilo, no en una gran ciudad, y saber que hay casos de
empresas en pequeñas ciudades que hacen cosas muy interesantes enciende un
rayo de esperanza en que algún día pueda trabajar para alguna de ellas.

También se habló de teletrabajo, y de cómo cada vez más los trabajadores
*exigimos* esa posibilidad. ¡Bien por nosotros!

#### Chatbot

Propuesto por Javier XXXX, de mainbait el patrocinador

#### Programación para niños

Propuesto por XXX de Zamora

#### Jueces online, programar por diversión

Propuesta por XXX

## Charlas

Y finalmente las charlas, lo *no tan interesante* del evento:

#### Implementando APIs en 2019, de Jose Ramón Huerga

## Personas

Azahara, https://twitter.com/azahara_fergui, del open space sobre
Desarrollo de software en provincias

Cristina, https://twitter.com/cristinafsanz

Amalia, https://twitter.com/amaliahern

Daniel Primo, https://twitter.com/delineas

Javier Gamarra, nhpatt

Juan Manuel Serrano, @juanshac

Salva de la Puente

## La conferencia en sí

- feedback por todos los lados
- respuesta ante el feedback
- debo dar feedback *en caliente*, pero no demasiado
- muy divertido el kahoot
- inlcuso hay una lista de música: https://open.spotify.com/playlist/5LFLfet0LRTipeiO8Hw9zt?si=CFb0ySRVRhu5yUlTieI9og

[Sergio Arroyo Cuevas]: https://twitter.com/@delr3ves
[Property Based Testing]: http://blog.jessitron.com/2013/04/property-based-testing-what-is-it.html
[WeCodeProperties]: https://github.com/delr3ves/WeCodeProperties
[Salvador de la Puente]: https://salvadelapuente.com/
[Quirk]: https://algassert.com/quirk
[Vicenç García Altés]: https://twitter.com/vgaltes
[taller serverless]: https://github.com/vgaltes/WCF
[The Serverless Course]: https://theserverlesscourse.com/
