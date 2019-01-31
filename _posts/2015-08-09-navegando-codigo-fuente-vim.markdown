---
title: "Navegando el código fuente con Vim"
date: 2015-08-09 22:16
author: Rubén Chavarría
comments: true
categories: 
- vim
- productivity
published: true
footer: false
sidebar: true
---

Últimamente se están popularizando muchos editores de código (Atom, Brakets,
Sublime Text,...). Pero mucho antes que ellos existieron al menos otros dos:
Emacs y [Vim][1]. No se muy bien la razón, quizá fue simplemente por el placer
de aprender, pero me decidí a [aprender a utilizar Vim][2]. Ya llevo un tiempo
con ello, aunque el aprendizaje va despacio.

Hay muchas cosas que me encantan de Vim, pero echo de menos algunas cosas que
me dan ciertos IDEs: refactorizaciones sencillas, búsqueda en múltiples
ficheros, integración con otras herramientas,... En este post voy a contar
cómo se puede **solucionar el problema de la navegación de código**.

<!-- more -->

Vim lo soluciona con un fichero de *tags*. Dentro de un lenguaje de programación,
un tag es simplemente el nombre de una función, método, clase,... Cada tag va
acompañado de una referencia para localizar dónde aparece: básicamente fichero y
número de línea. Con la herramienta [ctags][3] es posible generar un fichero que
sirve a Vim a conocer todas las tags de nuestro proyecto, y mediante comandos de
Vim, podemos movernos entre ellas de un fichero a otro.

## Uso de la herramienta `ctags`

Lo primero es instalar la herramienta. Para Linux es tan sencillo como ejecutar
un simple comando. Para otros sistemas operativos es también bastante sencillo,
pero se recomienda visitar la página del proyecto [Exuberant Ctags][3] para
conocer más detalles.

```bash
$ sudo apt-get install exuberant-ctags
```

El siguiente paso es generar el fichero de tags de nuestro proyecto:

```bash
$ cd /home/rchavarria/my-super-interesting-project
$ ctags -R .
```

Y por último, indicar a Vim dónde está el fichero de tags que queremos usar.
Para ello, una vez el editor está abierto, introducir el comando:

```
:set tags=/path/to/tags/file
```

Para facilitar las cosas y no tener que recordar cómo cargar el fichero de tags
manualmente, es recomendable poner el comando anterior en el fichero `.vimrc`,
de forma que al abrir Vim cargue siempre el fichero de tags.

## Navegar

Ahora, veamos cómo podemos movernos entre tags. Estos movimientos permiten
movernos dentro del mismo fichero, entre ficheros abiertos, o incluso ficheros
que no estén abiertos (pero están referenciados en el fichero de tags claro).

- `CTRL + ]`: en modo normal, nos lleva a la definición del tag donde está el
cursor. Con el teclado español, para mostrar el carácter `]` es necesario pulsar
`AlgGr` + `]`, pero para ejecutar este comando de Vim no se debe pulsar `AltGr`,
es suficiente con pulsar la tecla que contiene los carácteres `+`, `*` y `]`.
- `CTRL + t`: en modo normal, te lleva de vuelta al punto antes de navegar al tag.
- `CTRL + w` + `CTRL + ]`: en modo normal, divide la pantalla horizontalmente y
abre el fichero donde se encuentra la definición del tag.
- `:tag <tag name>` para ir directamente a la definición de una tag cualquiera
- `:tnext` y `:tprevious` (`:tn` y `:tp`) te llevan a la tag siguiente o anterior.
hay quien recomienda mapear estos comandos a otra secuencia de carácteres, como
por ejemplo: `]t` y `[t`.
- `:ltag` carga las tags en la ventana de lista de localizaciones (como `:ls`
cuando lista los búfferes abiertos)
- `:lopen` abre dicha ventana

## Referencias y enlaces relacionados

- [Vim and Ctags][4], fenomenal artículo que cubre casi todo lo relacionado con
Vim y ctags.
- [Aprendiendo Vim][2], en este mismo blog

[1]: http://www.vim.org
[2]: /blog/2014/10/11/aprendiendo-vim
[3]: http://ctags.sourceforge.net
[4]: http://andrewradev.com/2011/06/08/vim-and-ctags
