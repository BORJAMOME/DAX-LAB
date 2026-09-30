## ALL vs REMOVEFILTERS

Durante un tiempo pensé que ALL y REMOVEFILTERS hacían cosas distintas dentro de CALCULATE. No es así: **como modificador de CALCULATE son equivalentes**. REMOVEFILTERS existe desde 2019 como alias con un nombre que dice lo que hace.

La diferencia real es otra:

| | Dentro de CALCULATE | Fuera de CALCULATE |
|---|---|---|
| `ALL` | Quita filtros | Devuelve una tabla (todas las filas o valores, sin filtros) |
| `REMOVEFILTERS` | Quita filtros | No se puede usar: no devuelve tabla |

Por eso `COUNTROWS ( ALL ( dim_Producto[Color] ) )` funciona y `COUNTROWS ( REMOVEFILTERS ( dim_Producto[Color] ) )` da error.

Cuando lo que quiero es *quitar un filtro*, escribo REMOVEFILTERS: quien lea la medida entiende la intención sin pensar. Reservo ALL para cuando necesito la tabla.

![ejemplo](/docs/imagenes/ALLREMOVE.png)

### Las medidas

#### Todos los colores

```DAX
Todos_Colores_REMOVEFILTERS =
CALCULATE (
    [Cantidad],
    REMOVEFILTERS ( dim_Producto[Color] )
)
```

```DAX
Todos_Colores_ALL =
CALCULATE (
    [Cantidad],
    ALL ( dim_Producto[Color] )
)
```

Las dos devuelven lo mismo en cada fila: el total de todos los colores, marque lo que marque el slicer de color. Los filtros de fecha, tienda, etc. se siguen respetando.

#### Solo rojo

```DAX
Color_Rojo =
CALCULATE (
    [Cantidad],
    dim_Producto[Color] = "Rojo"
)
```

El filtro `dim_Producto[Color] = "Rojo"` es en realidad `FILTER ( ALL ( dim_Producto[Color] ), dim_Producto[Color] = "Rojo" )`. Es decir, **ya sobrescribe** cualquier filtro de color que venga de fuera. Por eso en una matriz por color esta medida devuelve el total de rojo en todas las filas.

#### Rojo + REMOVEFILTERS

```DAX
Color_Rojo_REMOVEFILTERS =
CALCULATE (
    [Cantidad],
    dim_Producto[Color] = "Rojo",
    REMOVEFILTERS ( dim_Producto[Color] )
)
```

Mismo resultado que `Color_Rojo`. CALCULATE aplica primero los modificadores (REMOVEFILTERS, ALL, USERELATIONSHIP…) y después los filtros, así que el REMOVEFILTERS sobra: el filtro booleano ya había quitado el color externo.

Si lo que quieres es lo contrario (rojo *solo si* rojo está en la selección del usuario), la herramienta es KEEPFILTERS:

```DAX
Color_Rojo_KEEPFILTERS =
CALCULATE (
    [Cantidad],
    KEEPFILTERS ( dim_Producto[Color] = "Rojo" )
)
```

En la fila "Azul" devuelve blanco; en la fila "Rojo", el valor de rojo.
