---
title: "Deuda técnica"
date: 2015-10-12 18:58
author: Rubén Chavarría
comments: true
categories: 
- technical debt
- glossary
published: true
footer: false
sidebar: true
---

Cuando hay que implementar una funcionalidad nueva en una aplicación software
existen básicamente dos maneras de hacerlo: la rápida (el *hack*, la *ñapa*) y
la correcta. Esta última más costosa y generalmente más compleja de llevar a
cabo.

La **deuda técnica** es una metáfora en el mundo del software que nos permite
discutir sobre este problema. Mediante la cual, se establece que **implementar
una funcionalidad eligiendo el camino rápido en lugar del correcto, es como
pedir un préstamo**, el cual genera unos intereses que habría que pagar en el
futuro en forma de un trabajo extra para mejorar o arreglar los atajos tomados
al no haber hecho la funcionalidad de una forma más correcta.

<!-- more -->

La metáfora fue propuesta por Ward Cunningham en un informe para la
[OOPSLA 1992]. Se suele utilizar para explicar a la gente *no técnica* la necesidad de
refactorizar. Ya ha llovido desde entonces, y se ha discutido mucho sobre el
tema. Así que también hay gente que no está de acuerdo con la metáfora. Yo creo
que como todas las metáforas, flaquea si uno empieza a profundizar en el tema.
Martin Fowler responde a los contrarios a la metáfora documentando que hay más
de una forma de deuda técnica en su famoso [cuadrante de la deuda técnica].

Mucho antes que Cunningham, Meir Manny Lehman, en el 1980 ya comentaba
conceptos relacionados:

> Un programa en evolución está en contínuo cambio, por lo tanto, su
complejidad (reflejada en una estructura que se deteriora) se incrementa a no
ser que se haga un trabajo para mantenerla o reducirla

![Technical debt]({{ site.baseurl }}/assets/images/2015/technical-debt.jpg)

*Imagen tomada del tweet de [Kristian Hellang](https://twitter.com/khellang/status/626716128379830273)*

## Causas

La principal culpable en la generación de deuda técnica, y así coinciden muchos
autores, es en la presión en plazos y planes de entrega que el departamento de
desarrollo se *deja* imponer por parte de negocio (marketing, ventas,...).
Estas presiones ocasionan que se publiquen versiones del software sin que se
implementen correctamente las funcionalidades.

Casi todas las demás causas se pueden derivar de esta primera, por ejemplo: recortes
en procesos de pruebas, incluso eliminación de pruebas automáticas, lo cual
hace muy difícil y extremadamente caro la detección y corrección de errores.
Los procesos de aseguramiento de la calidad también se ven recortados, así como
la documentación.

Las prisas también ocasionan que se pospongan trabajos necesarios o que se
retrasen refactorizaciones en el tiempo, ya que para adaptarse a los cambios, a
veces hay que refactorizar. Cuanto más se retrasa esta refactorización, más
cambios habrá que hacer y más caro será el cambio. Evitar refactorizaciones
puede llevar a que ciertas partes del código sean confusas.

También existe quien le echa la culpa a la cultura de la empresa: procesos
pobres, falta de entendimiento, falta de educación, de cuidado y de
colaboración con otros departamentos. Mientras existen otras causas maś
atribuibles a individuos: incompetencia y falta de profesionalidad.

Hablando de profesionalidad, hay una cita Robert C. Martin que me encanta:

> The only way to go fast is to go well

> La única forma de ir rápido, es hacer las cosas bien

## Consecuencias

Estoy de acuerdo con que la principal causa por la que se genera deuda técnica
es por *atajos* que se toman para poder entregar más de lo que se debería en
plazos cortos de tiempo. Pues **aunque parezca contradictorio, la mayor
consecuencia de la deuda técnica es precisamente que los proyectos no se
entreguen a tiempo**.

La deuda incurrida puede hacer que un desarrollo previsiblemente corto pueda
llevar mucho más tiempo de lo previsto en implementarse, ya que el mayor coste
de la deuda técnica es el hecho de que ralentiza el desarrollo de futuras
funcionalidades. Comprometiendo la viabilidad del proyecto a largo plazo.
Justamente como una deuda financiera, que produce un beneficio a corto plazo
pero puede tener resultados desastrosos a largo plazo si la deuda contraída no
se va pagando.

Otras consecuencias no tan mortales para el proyecto incluirían errores no
subsanados o desconocidos, que harían que el producto fuera inestable. Lo cual
tiene sentido, recortando o eliminando tests automáticos y calidad no se puede
esperar obtener mejores resultados.

La documentación entregada también se resiente, ya que se suele encontrar
desactualizada, escasa o inservible.

## Soluciones

La única forma de pagar la deuda contraída es completando el trabajo que no se
hizo correctamente. Y esto se hace refactorizando para mejorar el trabajo que
se dejó pendiente. El mejor momento para refactorizar es justo antes de
comenzar una nueva funcionalidad, adaptando el código a los nuevos
requerimientos.

Existen herramientas que permiten atacar ciertos puntos de la deuda técnica,
las herramientas de análisis estático del código. Pero por sí solas no son
suficientes, ya que la mayoría de las veces, la deuda técnia no se refleja
directamente en el código, sino también en la arquitectura, en componentes
desactualizados o en ciertas estructuras que estas herramientas no son capaces
de analizar.

Desde el punto de vista del equipo de desarrollo, hacer la deuda visible puede
ayudar. Podría ser intersante mantener una lista explícita sobre tareas
necesarias para reducir la deuda. Y a un nivel más alto, hacer entender a
marketing y otros departamentos de negocio que si no se planifica cierto tiempo
para reducir la deuda técnica se corre el riesgo de que no sea posible entregar
todas las funcionalidades que ellos quieren.

## Referencias:

- [Wikipedia en español sobre la deuda técnica]
- [Javier Garzás sobre la deuda técnica]
- [OOPSLA 1992]
- [Technical debt, wikipedia en inglés]
- [Martin Fowler sobre la deuda técnica]
- [Deuda técnica en Cunningham & Cunningham wiki]
- [Cálculo de la deuda técnica basado en fórmulas]
- [Technical Debt: From Metaphor to Theory and Practice]

## Actualización (01-12-2015)

Por twitter encontré esto de [J.B.Rainsberger](https://twitter.com/jbrains/status/665625243382325251)

> Para la mayoría de aplicaciones, el camino rápido siempre lleva a la muerte y
destrucción del proyecto; para aplicaciones pequeñas, el buen camino siempre
parece una exageración

[Wikipedia en español sobre la deuda técnica]: https://es.wikipedia.org/wiki/Deuda_t%C3%A9cnica
[Javier Garzás sobre la deuda técnica]: http://www.javiergarzas.com/2012/11/deuda-tecnica-2.html
[cuadrante de la deuda técnica]: http://martinfowler.com/bliki/TechnicalDebtQuadrant.html
[OOPSLA 1992]: http://c2.com/doc/oopsla92.html
[Technical debt, wikipedia en inglés]: https://en.wikipedia.org/wiki/Technical_debt
[Martin Fowler sobre la deuda técnica]: http://martinfowler.com/bliki/TechnicalDebt.html
[Deuda técnica en Cunningham & Cunningham wiki]: http://www.c2.com/cgi/wiki?TechnicalDebt
[Cálculo de la deuda técnica basado en fórmulas]: http://docs.sonarqube.org/display/SONARQUBE44/Technical+Debt+Calculation
[Technical Debt: From Metaphor to Theory and Practice]: http://www.computer.org/csdl/mags/so/2012/06/mso2012060018.html
