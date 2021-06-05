---
layout: post
title:  "Webpack y sus colegas"
subtitle: "O cómo reconciliarte con Webpack y empezar una nueva vida juntos..."
date:   2021-06-05 15:00:00 -0400
header-img: "img/posts/webpack_and_friends.jpg"
author: "Aida Albarrán"
comments: true
tags: [Webpack, JavaScript]
---

No sé si a ti te habrá pasado, pero para mí Webpack siempre ha sido todo un misterio. Sé que hay un fichero ahí de configuración, que hace cosas, pero no he logrado entender bien para qué es cada cosa…. Hasta ahora…


Aquí voy a hacer mención especial a Víctor Borrego, formador en el máster de [LemonCode](https://lemoncode.net/), que nos ha conseguido explicar con pocas palabras qué es Webpack y que yo lo entienda.


## ¿Qué es Webpack entonces?


Es una librería que nos ayuda a convertir nuestro código en algo que el navegador pueda entender y que además sea lo más óptimo posible. Bueno, esa es la explicación a groso modo, pero, si entramos en el detalle, Webpack nos va a ayudar a muchas cosas:

* Si en nuestros archivos de Javascript usamos ES6, nos va a ayudar a transformarlo a ES5 y que lo puedan entender más navegadores.

* Si trabajamos con Typescript, nos va a ayudar a interpretarlo y transformarlo.

* Si trabajamos con React, igualmente nos va a ayudar a transformarlo en código de Javascript que un navegador pueda entender.

* Si trabajamos con SASS, nos va a ayudar a convertirlo en estilos que se puedan entender.

Y así podría seguir con cada librería que usemos en nuestro proyecto.

Al final lo que va a hacer es crear un solo archivo *js*, que es lo estándar y lo que entiende el navegador (aunque se puede dividir si se quiere), con todo nuestro código y va a insertarlo en un *html* mediante un *script*. Además, podríamos querer que ese *js* esté minificado (código más liviano), y Webpack nos ayuda también a hacerlo.

Todos los archivos generados por Webpack los meterá en una carpeta a la que solemos llamar *dist* o *build*, por ejemplo y eso es lo que usará producción para crear nuestra aplicación.

## ¿Cómo hace Webpack todo eso?

Pues aquí es donde viene lo interesante y que me ha hecho “click” en la cabeza. Webpack no lo hace, o al menos no lo hace por sí solo, sino que necesita “colegas” (como dice Víctor) que le ayuden a hacerlo. Webpack, por tanto, actuaría como “orquestador”, que va organizando el equipo.

### ¿Y quiénes forman ese equipo de colegas?

Los loader y los plugin

Cada integrante será especialista en lo suyo, por ejemplo:

Habrá especialistas en transpilar el código de typescript a javascript.

Habrá colegas que lo hagan de sass a un código de css que entienda el navegador.

Y así con cada una de las cosas que queramos añadir a nuestro proyecto.

### ¿Cuál es la diferencia entre loaders y plugins?

* __Loaders__

Los loaders tienen como objetivo recorrerse todo nuestro código en busca de archivos, pero no cualquier archivo sino de los que son especialistas.

Es decir, si es un loader de Typescript, se recorrerá todo nuestro proyecto en busca de *ts* (o *tsx*) y va a ir transpilando (o traduciendo) el código de typescript a javascript.

Si es un loader de SASS, se recorrerá nuestro proyecto en busca de los ficheros con extensión *scss* y transformándolo.

Y así todos.

Vale, entonces, si ya el loader los traduce, entonces ¿qué leches hace el plugin?

* __Plugins__

Los loaders saben mucho de lo suyo, pero sólo tienen la visión de sus propios archivos, es decir, que no conocen el resto de archivos que tienen extensiones diferentes a la suya.

Vamos a necesitar de alguien que sí tenga una visión más global y que sepa qué otros archivos existen, para poder conectarlo con el que sea necesario.

Es decir, un plugin sería por ejemplo el que inserta en el *html* que genera el bundle un *script* con el enlace al bundle de javascript generado [HtmlWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/).

También hay otros plugins que trabajan sobre la carpeta *dist* haciendo una limpieza antes de generar un nuevo build.

Y así podríamos seguir mencionando plugins, pero no hace falta porque lo interesante es que se entienda para qué sirven.

En definitiva, el plugin sí conoce de manera global lo que pasa al crear los bundles.

## ¿Y ya está?

Evidentemente, no :). A partir de aquí es cuestión de buscar los loader o plugin que necesitemos y ver cómo se usan, pero al menos sabemos para qué se necesitan y si nos falla algo sabremos por dónde rascar.

A mí me ha abierto un mundo esto, oye…


Fuente de la imagen: [Min An from Pexels](https://www.pexels.com/@minan1398?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)
