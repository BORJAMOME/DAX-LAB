## Evitar que un slicer muestre opciones sin datos

Imagina que trabajas en una empresa de logística que gestiona envíos de distintos transportistas a distintos destinos.

- Tabla de hechos: fact_envios (cada envío con transportista, destino y cantidad de paquetes)

- Dimensiones: dim_transportistas y dim_destinos

Problema típico:

- En un informe, el usuario selecciona un transportista, por ejemplo “Transportista X”.

- Hay un slicer de destinos donde aparecen todos los destinos posibles.

- Algunos destinos como “Destino Z” no tienen envíos con ese transportista.

- Si el usuario selecciona “Destino Z”, los gráficos muestran blancos, lo que confunde y frustra al usuario.


##### Medida para resolverlo

```DAX
Destinos Válidos = COUNTROWS ( fact_envios )
```

#### Cómo funciona

El slicer de destinos evalúa la medida una vez por cada destino. En cada evaluación, el filtro del destino y el del transportista seleccionado llegan a `fact_envios`, así que COUNTROWS devuelve los envíos de esa combinación, o blanco si no hay ninguno.

En el panel de filtros del slicer: **Destinos Válidos → no está en blanco** (o `>= 1`).

La primera versión usaba `COUNTROWS ( RELATEDTABLE ( fact_envios ) )`. Da lo mismo: en una medida, RELATEDTABLE equivale a CALCULATETABLE sobre la tabla y el contexto ya la filtra. La versión corta se entiende mejor.

---

#### Múltiples tablas de hechos

Si el destino también vale cuando solo tiene devoluciones:

```DAX
Destinos Válidos =
COUNTROWS ( fact_envios ) + COUNTROWS ( fact_devoluciones )
```

BLANK + número da número, así que basta con que una de las dos tablas tenga filas.

Por qué hace falta: con relaciones unidireccionales, el slicer de transportista filtra `fact_envios`, pero ese filtro no sube hasta `dim_destinos`. El slicer de destinos no se entera. La medida como filtro de visual cierra ese hueco sin volver bidireccional la relación en el modelo.
