## Orden de los filtros en DAX

En DAX el orden de los filtros importa. Cuando anidas CALCULATE, el filtro más interno se aplica primero, pero luego el externo puede modificar o incluso eliminar lo que hizo el interno.
En otras palabras: DAX tiene su propia “coreografía de filtros” y entenderla evita resultados que parecen mágicos… o incorrectos.

#### Ejemplo:

Quieres sumar ventas de un país concreto, pero al mismo tiempo quieres ignorar cualquier filtro externo sobre país. Si no controlas el orden, el total puede mostrar un país distinto o incluso todas las ventas del mundo.

![ejemplo](/docs/imagenes/OrdenFiltros.png)

#### Explicación:

- CALCULATE recalcula una medida bajo un nuevo contexto de filtro.

- Cuando anidas CALCULATE, el filtro interno se evalúa primero, luego el externo puede modificar, mantener o eliminar esos filtros.

- ALL elimina filtros existentes sobre la columna o tabla.

- KEEPFILTERS preserva los filtros que ya se han aplicado, en lugar de reemplazarlos.

Clave:

- Filtro externo → último en escribir la fórmula

- Filtro interno → primero en ejecutarse

- DAX aplica filtros internos antes que externos, pero el externo puede anular al interno si no usas KEEPFILTERS.

##### ALL País
```
  ALL País =
CALCULATE(
    CALCULATE(
        [Cantidad],
        KEEPFILTERS(dim_Clientes[País] = "Francia")
    ),
    ALL(dim_Clientes[País])
)
```

- Filtro interno: solo Francia → inicialmente queremos solo Francia.

- Filtro externo: ALL(dim_Clientes[País]) → elimina cualquier filtro de país externo, incluso nuestro “Francia”.

Resultado: todas las ventas de todos los países.

El filtro interno se ejecuta, pero el externo lo borra.


##### País ALL
```
País ALL =
CALCULATE(
    CALCULATE(
        [Cantidad],
        ALL(dim_Clientes[País])
    ),
    KEEPFILTERS(dim_Clientes[País] = "Francia")
)
```
- Filtro interno: ALL(dim_Clientes[País]) → ignoramos cualquier filtro previo de país.

- Filtro externo: KEEPFILTERS(dim_Clientes[País] = "Francia") → aplicamos Francia sin que lo borre nada.

Resultado: solo Francia, pero ahora ignorando cualquier filtro previo.

Aquí el orden cambia completamente el resultado.










