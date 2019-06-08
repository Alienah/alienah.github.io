---
layout: post
title:  "Observer Vs EventBus"
subtitle: "Patrones de diseño"
date:   2018-05-03 12:00:00 -0400
header-img: "img/posts/observer_background.jpg"
author: "Aida Albarrán"
comments: true
tags: [JavaScript, FunctionalProgramming]
---

En nuestro empeño por aprender a programar creando código robusto y mantenible, nos encontramos con más patrones de diseño, en este caso, hablamos de los relacionados con el manejo de los eventos.

Antes de meternos en materia y desengranar estos patrones, vamos a definir los conceptos, para entender mejor sus diferencias, porque aunque parezcan lo mismo, no lo son.

## Observador u Observer Pattern

Es un patrón de diseño en el que un objeto, llamado sujeto, tiene otros varios del cual dependen, que son los observadores. Es decir define una dependencia del tipo __uno a muchos__ entre objetos, de manera que cuando uno de los objetos cambia su estado(sujeto), notifica este cambio a todos los dependientes, normalmente a través de uno de su métodos.

## Pub-Sub(Publisher-Subscriber) Pattern:

Es un patrón de diseño que tiene mucha similitud con el anterior, puesto que hay alguien que envía notificaciones (publisher) y hay otros objetos que las reciben (subscribers), pero con una diferencia: el publisher no es el que programa las notificaciones. El que lo hace es el event bus o broker o message broker.

En este caso, el publisher y el subscriber no tienen por qué tener consciencia el uno del otro, ya que es el __intermediario (event bus)__ el que se comunica con ellos.

![Esquema de las relaciones entre emisores y receptores que hemos comentado. Fuente de imagen http://ww1.developers-club.com/](/img/posts/observer_esquema.jpeg)

## Ejemplos para aclarar las mentes

Para entender mejor las diferentes posibilidades que nos ofrecen ambos para manejar eventos, vamos a ir analizando varios ejemplos:

### Observer con DOM de nivel 0:

```js
function deposito() {
    let capacidadMaxima = 200;
    let contenido = 100;
    return {
        llenar: function(litros) {
            if(contenido + litros >= capacidadMaxima) {
                if(typeof(this.onDepositoLLeno) == 'function') {
                    this.onDepositoLleno(capacidadMaxima);
                }
            } else {
                contenido = contenido + litros;
            }
        }
    }
}
```

Esta función *depósito* es un patrón módulo que nos devuelve un método *llenar* de un objeto, el cual va a llenar de litros un depósito, siempre que haya capacidad. En el momento que exista una función *onDepositoLleno*, se activará la misma y dejará de llenarse.

Bien, ahora creamos un depósito.

```js
const unDeposito = deposito();
```
Si ahora usara mi método *llenar* y llenara entero mi depósito, no ocurriría nada, porque aún no me he subscrito a ningún método *onDepositoLLeno*.

```js
unDeposito.llenar(300);    //No ocurriría nada
```

Ahora vamos a subscribirnos al evento:

```js
unDeposito.onDepositoLleno = function(capacidad) {
        console.log('Lleno!', capacidad);
    }
```

Si volvemos a llenar el depósito

```js
unDeposito.llenar(300);
//Lleno! 200
```

Ahora sí se activa el evento y nos dice que está lleno.

Este tipo de manejador de eventos, sólo nos valdría con un subscriptor, al que vamos a llamar *observador*. El hecho de sólo poder tener un observador es lo que se conoce como __subscripción de nivel 0__, que es __de uno a uno__.

### Observer de uno a muchos

Si quisiésemos que en el ejemplo anterior se pudiesen subscribir varios observadores al evento, podríamos hacerlo así:

```js
function deposito() {
    let capacidadMaxima = 200;
    let contenido = 100;
    const subscriptores = [];
    return {
        addEventListenerCustom: function(funcionASubscribir) {
            subscriptores.push(funcionASubscribir);
        },
        llenar: function(litros) {
            if(contenido + litros >= capacidadMaxima) {
                if(subscriptores.length > 0) {
                    for(i = 0; i < subscriptores.length; i++) {
                        const cadaFuncionSubscrita = subscriptores[i];
                        cadaFuncionSubscrita(capacidadMaxima);
                    }
                }
            } else {
                contenido = contenido + litros;
            }
        }
    };
}
```

Lo primero que vemos diferente es que hemos añadido un array vacío en el que van a ir introduciéndose nuestros observadores o subscriptores.

También hemos modificado nuestro manejador de eventos, ahora se llama *addEventListenerCustom* (para que se vea la diferencia con respecto al otro ejemplo, que era más como un onclick), y lo que va a hacer es manejar a los que se van subscribiendo y los introduce en el array.

Ahora nuestro método *llenar* lo que hace es, mientras haya subscriptores en el array, recorre dicho array y asigna a cada uno de ellos como parámetro (recordemos que los observadores o subscriptores son funciones) la capacidad máxima.

Ahora sólo nos queda ver cómo se pueden subscribir a este evento los observadores. Sería más o menos así:

```js
const unDeposito = deposito();
unDeposito.addEventListenerCustom(function(capacidad) { console.log('Lleno!', capacidad); });
unDeposito.addEventListenerCustom(function(capacidad) { console.log('Este otro depósito también se entera de que está lleno', capacidad); });
```
Es decir, que si ahora llenáramos nuestro depósito...

```js
unDeposito.llenar(300)
// Lleno! 200
// Este otro depósito también se entera de que está lleno 200
```

Este patrón de observer es __de uno a muchos__.

### EventBus o patrón Pub-sub

Ahora vamos a analizar un ejemplo de patrón de diseño tipo eventBus o pub-sub.

```js
function eventBus() {
    const subscribersInfo = [];

    this.addSubscriber = function(callback) {
        const eventNames = [].slice.call(arguments).slice(1);
        subscribersInfo.push({
            subscriber: callback,
            eventNames: eventNames
        });
    };
    //pub sub
    this.emit = function(eventName, eventArgs) {
        for(let i = 0; i < subscribersInfo.length; i++) {
            const info = subscribersInfo[i];
            for(let j = 0; j < info.eventNames.length; j++) {
                const eventNameFromList = info.eventNames[j];
                if (eventNameFromList === eventName) {
                    info.subscriber(eventName, eventArgs);
                };
            };
        };
    };
};
```

En este eventBus podemos ver que hay dos métodos principales:

* __addSubscriber__, que como bien indica, es la manera en la que se subscriben los observadores o, en este caso, sería más adecuado llamarles *subscriptores*.

Analizándolo con más detenimiento, vemos que es una función con otra función como parámetro, por eso lo llama *callback*, porque va a llamar a otra función. Esta función *addSubscriber* lo que hace es ir añadiendo en un array por un lado las funciones subscritas (callback) y por otro, unos listados de los nombres de los eventos a los que están subscritas (eventNames). Esos nombres de eventos vienen definidos en los propios argumentos. Cuando veamos el ejemplo del depósito se entenderá mejor.

* __emit__, que dispara el evento al que se han subscrito.

Esta función tiene dos parámetros, *eventName* y *eventArgs*. Lo que hace es recorrer primero el array de subscriptores y, para cada uno de ellos, recorre el array de eventos a los que están subscritos. Si alguno de éstos es igual que el que viene dado en el parámetro (eventName), entonces se ejecuta o se dispara esa función subscriptora (que recordemos que es una función de llamada) con los argumentos *eventArgs*.

* Ambos métodos son controlados por el __bus__.

Ahora vamos a aplicar este *eventBus* a nuestro ejemplo del depósito,

Primero creamos nuestro bus:

```js
const bus = new eventBus();
```

Ahora vamos a suscribir un evento a este bus, dicho evento en realidad no va a saber cuándo va a ser lanzado, porque ese poder lo tiene el eventBus (es decir, *bus*):

```js
bus.addSubscriber(
    //Hemos puesto en el console log tambiién los argumentos que va a recibir la función, para así verlo más claro.
    function() { console.log('Evento lanzado con estos argumentos:', arguments); }, 
    'onDepositoLleno')
```

Como vemos, se ha usado el patrón definido en el eventBus para suscribir eventos al que llamábamos *addSubscriber*.

Recordemos que el método *addSubscriber* recibía como parámetro una función callback (que estamos definiendo aquí mismo) y con este método se va a añadir a la lista de eventos de nuestro callback, el eventName ‘onDepositoLleno’.

Ahora vamos a redefinir nuestra función del depósito:

```js
function deposito(bus) {
    const capacidadMaxima = 200;
    let contenido = 100;
    return {
        llenar: function(litros) {
            if(contenido + litros >= capacidadMaxima) {
                bus.emit('onDepositoLleno', capacidadMaxima);
            } else {
                contenido = contenido + litros;
            }
        }
    }
}
```

Aquí indicamos que nuestro *bus*, en su método *emit*, va a tener dos parámetros:

* El que habíamos llamado __eventName__, aquí se llamará *‘onDepositoLleno’*.
* Lo que habíamos definido como __eventArgs__, va a ser la *capacidadMaxima*.

Ahora ya nos hemos quitado la responsabilidad de saber qué suscriptores existen, ya que de eso se encarga el eventBus, con lo cual, hemos simplificado nuestra función, que lo que va a hacer es enviar lo que deba a los que se hayan suscrito al evento con nombre 'onDepositoLleno' y lo va a hacer a través del eventBus.

Ahora sólo nos queda definir la variable y llenar nuestro depósito:

```js
const unDeposito = deposito(bus);  //Vemos que esta vez hemos introducido como parámetro el eventBus creado anteriormente

unDeposito.llenar(300)

// Evento lanzado con estos argumentos: Arguments(2) ["onDepositoLleno", 200, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```


Como vemos, hay varias maneras de subscribirse a eventos y, dependiendo de la situciación, elegiremos una u otra.  Sin embargo, hay que tener en cuenta que un patrón como el *pub-sub* o *eventBus*, nos permite desacomplamiento, pero también es menos cohesivo, es decir, que el hecho de estar tan separada la gestión de los eventos de los propios eventos, hace que efectivamente se pueda usar todas las veces que queramos, pero también lo hace más complicado de entender y, por tanto, de usar correctamente.  Con lo cual, nuevamente recordamos que es importante tener en cuenta cuál es nuestra situación y cómo es nuestra aplicación, para decidir cómo gestionaremos los eventos.


En un futuro, pondré un ejemplo más real del uso del eventBus para que quede el concepto, claro no, ¡cristalino!.

Fuente de los ejemplos: [Carlos Blé](https://medium.com/@Carlosble)

Fuente de la imagen: [freepik](https://www.freepik.es/vector-gratis/fondo-espacio-telescopio_1536602.htm)