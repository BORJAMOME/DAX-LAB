## Filtrar ignorando el contexto externo (pero solo en una columna)

Dos medidas que fijan el color dentro de CALCULATE:

```DAX
Plateado =
CALCULATE ( [Cantidad], dim_Producto[Color] = "Plateado" )
```

```DAX
Semáforo =
CALCULATE ( [Cantidad], dim_Producto[Color] IN { "Rojo", "Amarillo", "Azul" } )
```

En la matriz por color, `Semáforo` devuelve 155.264 en todas las filas: el filtro de CALCULATE sobre `Color` sobrescribe el color de la fila.

![ejemplo](/docs/imagenes/sincontexto.png)

Pero al marcar *Bicicletas* en el slicer de Categoría, `Semáforo` baja a 92.448. El filtro de categoría **se respeta**, porque CALCULATE solo sobrescribe las columnas que tú le nombras.

![ejemplo](/docs/imagenes/sincontexto_II.png)

Es la idea que más uso para leer una medida ajena: mirar qué columnas aparecen en los filtros de CALCULATE. Esas son las que la medida ignora del contexto; todas las demás siguen mandando.
