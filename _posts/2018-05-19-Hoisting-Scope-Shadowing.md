---
layout: post
title:  "Sobre el Hoisting, el Scope y el Shadowing"
subtitle: "Los conceptos que te evitarán quebraderos de cabeza..."
date:   2018-05-19 12:00:00 -0400
header-img: "img/posts/hoisting_background2.jpeg"
author: "Aida Albarrán"
comments: true
tags: [JavaScript]
---

Algo curioso que ya había usado pero que no era tan consciente de ello es el *hoisting*. En concreto, donde más lo usamos es en la declaración de funciones.

Cuando defines una función, da igual dónde coloques esa definición en tu código, Javascript va a interpretar que está al inicio, así, cuando ejecutes dicha función, siempre va a funcionar, independiente de que se ejecute antes o después de la definición. Eso es lo que se conoce como __hoisting__.

Por ejemplo, si yo defino esta función y la ejecuto:

```js
function saludar(nombre) {
    console.log(`Hola, ${nombre}`);
}

saludar('Daniela');
```

Eso sería igual que si la ejecución la coloco antes de la definición:

```js
saludar('Daniela');

function saludar(nombre) {
    console.log(`Hola, ${nombre}`);
}
```

Ya que Javascript automáticamente ha elevado la declaración de la función al inicio.

Con las variables también pasa lo mismo. Partimos de este ejemplo:

```js
var x = 5;

(function () {
    console.log("x: ", x); // No obtenemos '5' sino 'undefined'
    var x = 10;
    console.log("x: ", x); // 10
})
```
(Fuente del ejemplo: MDN Web Docs)

¿Por qué obtenemos *undefined* en el primer *console.log*?

Aquí hay que remarcar dos cosas:

1) En esta función se vuelve a declarar la variable *x*, con lo cual el __scope__ de dicha variable es local y deja de ser global, es lo que se conoce como __shadowing__, es decir, que se está haciendo sombra a la asignación de la global y es como si ésta no existiera.

2) Puesto que se está volviendo a declarar x, lo que entiende Javascript es que lo has hecho al inicio de la función, por el principio del hoisting, es decir, que interpreta algo así:

```js
var x = 5;

(function () {
    var x; // Ha elevado la declaración al principio
    console.log("x: ", x); // 'undefined' porque sún no se le ha asignado ningún valor
    var x = 10;
    console.log("x: ", x); // 10
})
```

Así pues, ya que el propio Javascript interpreta que la declaración de variables y funciones está al inicio, lo recomendable es que nosotros efectivamente las declaremos al comienzo (no hace falta asignar ningún valor, sólo declararlas) y así hacer el código más acorde con la realidad y por lo tanto, más entendible.