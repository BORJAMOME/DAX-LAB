## Modelo en Estrella

Un Modelo en Estrella es una forma ordenada e inteligente de organizar los datos para analizarlos bien y rápido.

- En el centro: ***una tabla de hechos***

- Alrededor: ***varias tablas de dimensiones***


### La idea clave

Imagina que quieres responder preguntas como:

- ¿Cuánto he vendido?

- ¿Por producto?

- ¿Por tienda?

- ¿Por promoción?

- ¿Por canal?

Si todo estuviera mezclado en una sola tabla:

- el modelo sería pesado

- los cálculos lentos

- y DAX sería una pesadilla

El modelo en estrella separa responsabilidades. Cada tabla tiene un único propósito.


### El corazón del modelo: la tabla de hechos

***FactSales*** -> ¿Qué ha pasado? -> La tabla de hechos no describe, solo mide.

Aquí están los datos numéricos:

- Ventas

- Cantidades

- Devoluciones

- Descuentos

Y también las claves que la conectan con todo lo demás:

- Producto

- Tienda

- Canal

- Promoción

- Moneda

- Fecha

### Las dimensiones: el contexto

Alrededor de FactSales tienes las dimensiones:

***DimStore*** -> ¿Dónde se ha vendido?

***DimProduct*** -> ¿Qué se ha vendido?

***DimPromotion*** -> ¿Bajo qué promoción?

***DimCurrency*** -> ¿En qué moneda?

***DimClass*** -> ¿A qué familia o clasificación pertenece?

### Las relaciones

Todas las dimensiones se conectan con FactSales mediante relaciones:

- 1 → *

- Dirección de filtro: de la dimensión al hecho

Esto permite algo fundamental:

- Cuando filtras una dimensión, automáticamente filtras las ventas.



![ejemplo](/docs/imagenes/modeloestrella.png)



















