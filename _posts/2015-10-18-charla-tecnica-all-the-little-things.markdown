---
layout: post
title: "Charla técnica: All the little things"
date: 2015-10-18 18:40
author: Rubén Chavarría
comments: true
categories: 
- technical talks
- oop
- code duplication
published: true
footer: false
sidebar: true
---

A través del blog [Garajeando] llegué a esta charla de [Sandi Metz] que se
titula [All the little things]. Sandi Metz es autora del libro
[Practical Object Oriented Design in Ruby] y ya he visto algunas de sus charlas, por lo
que la calidad estaba asegurada. Sandi es muy defensora de la OOP y es una
profesional excelente.

La charla trata de una refactorización siguiendo la kata [The Gilded Rose]. La
refactorización parte de un método gigante lleno de `if`s y lleva el código
hacia clases pequeñas, métodos pequeños.

<!-- more -->

<iframe width="560"
        height="315"
        src="https://www.youtube.com/embed/8bZh5LMaSmE"
        frameborder="0"
        allowfullscreen></iframe>

## Notas tomadas de la charla

> Es preferible algo de duplicación a una mala abstracción

La duplicación es fácil de detectar y de eliminar llegado el caso. Una mala
abstracción al arrastraremos toda la vida del proyecto.

Unos días después, Sandi publicó un [artículo en forma de newsletter]
explicando algo más esta frase, porque parece que creó confusión o al menos,
causó reacciones que hicieron pensar a Sandi que la gente no entendió realmente
lo que ella quería transmitir.

La idea principal de ese artículo es que una vez identificada una mala
abstracción en el código (le pasan parámetros y hay `if`s y más `if`s), es
recomendable recrear la duplicación en el código y dejar que él mismo nos
indique nuevas abstracciones.

> Que tu objetivo sea llegar a Open/Closed

Uno de los principios de la Programación Orientada a Objectos, es el
Open/Closed Principle. Que para añadir nueva funcionalidad, no tengas que
modificar código existente (abierto a extensión, cerrado a modificación). Lo
ideal es que toda la aplicación siguiera este principio, no vas a llegar nunca,
pero cuanto más lo sigas, más fácil será añadir nueva funcionalidad sin romper
la existente.  

> Crea cosas pequeñas

Clases pequeñas, métodos pequeños,...

> Refactoriza basándote en la complejidad, mediante la complejidad

Uno de los objetivos de las refactorizaciones es hacer el código más sencillo.
Puedes utilizar métricas de complejidad para hacer un seguimiento de la
refactorización. En pasos intermedios, ésta puede crecer, pero debes tener
confianza en que la refactorización dará sus frutos y conseguirás un código más
sencillo y limpio.

> Refactoriza hacia la simplicidad

> La herencia no siempre es mala

Se habla mucho de composición en lugar de herencia, pero para Sandi hay una
ocasión, donde si se dan las siguientes circunstancias, la herencia es la mejor
solución:

1. La jerarquía no es profunda ni ancha, es poco profunda y estrecha (no
   involucra muchas clases hijas)
2. Las subclases son hojas dentro de tu árbol de clases, las subclases están en
   los extremos de tu árbol de clases, no entre medias
3. Las subclases usan todo el código de la clase padre

[Garajeando]: http://garajeando.blogspot.com.es/2015/08/interesting-talk-all-little-things.html
[Sandi Metz]: http://www.sandimetz.com
[All the little things]: https://www.youtube.com/watch?v=8bZh5LMaSmE
[Practical Object Oriented Design in Ruby]: http://www.sandimetz.com/products
[The Gilded Rose]: https://github.com/emilybache/GildedRose-Refactoring-Kata
[artículo en forma de newsletter]: http://us3.campaign-archive2.com/?u=1090565ccff48ac602d0a84b4&id=92902a19e4&e=072f6853e8

