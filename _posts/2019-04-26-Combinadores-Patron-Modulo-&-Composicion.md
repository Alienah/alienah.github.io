---
layout: page
title:  "Combinadores, patrón módulo y composición"
subtitle: "Pero ¡qué es todo esto!"
date:   2018-04-26 12:00:00 -0400
header-img: "img/posts/combinadores_background.jpg"
author: "Aida Albarrán"
comments: true
tags: [JavaScript, FunctionalProgramming]
---

Creo que ya puedo responder más o menos a esa pregunta, aunque aún me quedan por ver muchos ejemplos más.

## Combinadores:

Son funciones que devuelven otra función.

Cuando queremos reutilizar parte de las funciones, pero otra parte dentro de ésta va a cambiar, podemos usar las funciones combinatorias y así no repetimos tanto código. Vamos a ver un ejemplo.

```js
function uno() {
    console.log('--executing-function');
    const x = 1 + 1;  //La única parte en la que se diferencian
    console.log('--end of function');
}

function dos() {
    console.log('--executing-function');
    const x = 2 + 2;  //La única parte en la que se diferencian
    console.log('--end of function');
}

uno();
dos();
```

En el anterior ejemplo, podemos ver cómo sería si __NO__ usásemos la función combinatoria. Como se puede comprobar, hay dos *console.log* que se repiten en ambas funciones *uno* y *dos*, y que lo único que cambia es lo que está en medio.

Ahora vamos a ver cómo se resolvería con una función combinatoria. Tendríamos por un lado la función de preparación, que es lo que no quiero cambiar, y tendríamos la segunda parte ```f();```, que la coloco en medio y que además voy a querer que me la pase como argumento (callback) y que realmente es lo que va a cambiar luego.

```js
function combinator(f) {  //Introducimos la función que va a cambiar como parámetro
    return function() {
        console.log('--executing-function');
        f();  //Esto es lo que va a cambiar
        console.log('--end of function');
    }
}

const uno = combinator(function(){ const x = 1 + 1; });
const dos = combinator(function(){ const x = 2 + 2; });
uno();
dos();
```


## Patrón módulo:

* Es una función que devuelve un objeto.

* Es autosuficiente, es decir que tiene todo lo que necesita para funcionar en su interior, lo cual hace que resulte muy útil para mantener el código limpio.

* Se caracterizan por tener una parte del código que es privada (los módulos y variables), de manera que ayuda a evitar el colisionamiento con otras variables globales, etc.

* Tienen otra parte que es la pública y que es la dada por el return de la función principal.

Vamos a ver un ejemplo.

```js
function encriptaPalabras() {
    function encriptaPalabra(palabra) {
        const caracteres = palabra.split('');
        return caracteres.reverse().join('');
    }
    return {
        encripta: function(frase) {
            const palabras = frase.split(' ');
            return palabras.map((palabra) => {
                return encriptaPalabra(palabra);
            }).join(' ');
        }
    }
}
```

En el ejemplo, la función *encriptapalabra(palabra)* sería la parte privada, que posteriormente se va a usar en la parte pública, pero cuyos algoritmos, variables, operaciones,etc. queremos mantener en privado.

Como hemos dicho antes, la parte pública es la que viene después del return, es decir, el objeto, o más concretamente, el método del objeto, que tiene es su return más interno el resultado de aplicar nuestro módulo *encriptapalabra(palabra)*.

Se puede consultar más información al respecto en este [link](http://www.etnassoft.com/2011/04/11/el-patron-de-modulo-en-javascript-en-profundidad/).

## Composición:

La composición, hablando en un lenguaje llano, consiste en unir en un mismo destino varias funciones que hacen cosas distintas.

Por ejemplo, tengo una función que hace hablar a un perro, otra que hace bailar a un perro y tengo una función común que me graba al perro hablando o bailando, dependiendo de qué función le haya metido como parámetro.

Si usamos el ejemplo del encriptador de palabras anterior:

```js
function encriptaPalabraSuperBien(palabra) {
    const caracteres = palabra.split('');
    return caracteres.reverse().join('');
}

function encriptaPalabraDeOtraForma(palabra) {
    const caracteres = palabra.split('');
    return caracteres.map((caracter) => {
        return 'chi' + caracter;
    }).join('');
}

function encriptaPalabras(encriptaPalabra) {  //Función parámetro que variará
    return {
        encripta: function(frase) {
            const palabras = frase.split(' ');
            return palabras.map((palabra) => {
                return encriptaPalabra(palabra);
            }).join(' ');
        }
    }
}
```

Tendríamos dos funciones *encriptaPalabraSuperBien(palabra)* y *encriptaPalabraDeOtraForma(palabra)*, que tienen algoritmos distintos, pero las cuales se pueden aplicar igualmente bien a la función *encriptaPalabras(aquíIríaNuestraFunción)*, que sería el destino común.

Ejemplos de [Carlos Ble](https://medium.com/@Carlosble)
