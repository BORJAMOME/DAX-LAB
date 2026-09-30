## Contexto de filtro II: cinco formas de pedir "rojo"

Las cinco medidas de la captura buscan lo mismo, cantidad de productos rojos, pero en una matriz por color dan resultados muy distintos. La diferencia no está en el rojo, sino en qué hace cada una con el color que ya viene de la fila.

![Ejemplo_2](/docs/imagenes/Contexto_Filtro_II.png)

| Medida | Fila "Azul" | Fila "Rojo" | Qué hace con el filtro de la fila |
|---|---|---|---|
| `Color_Rojo` | 71.090 | 71.090 | Lo sobrescribe |
| `FILTER_Color_Rojo` | (blanco) | 71.090 | Lo respeta |
| `FILTER_ALL_Color_Rojo` | 71.090 | 71.090 | Lo sobrescribe |
| `VALUES_Color_Rojo` | (blanco) | 71.090 | Lo respeta |
| `DISTINCT_Color_Rojo` | (blanco) | 71.090 | Lo respeta |

### Las que sobrescriben

```DAX
Color_Rojo =
CALCULATE ( [Cantidad], dim_Producto[Color] = "Rojo" )
```

```DAX
FILTER_ALL_Color_Rojo =
CALCULATE (
    [Cantidad],
    FILTER ( ALL ( dim_Producto[Color] ), dim_Producto[Color] = "Rojo" )
)
```

Son la misma medida. El filtro booleano de la primera se traduce internamente a la segunda: `ALL` sobre la columna y luego quedarse con rojo. Por eso las dos devuelven 71.090 en todas las filas.

### Las que respetan el contexto

```DAX
FILTER_Color_Rojo =
CALCULATE (
    [Cantidad],
    FILTER ( dim_Producto, dim_Producto[Color] = "Rojo" )
)
```

`FILTER` recorre `dim_Producto` *ya filtrada* por la fila. En la fila Azul solo hay productos azules, ninguno pasa el filtro y el resultado es blanco.

Funciona, pero filtrar la tabla entera es la opción cara: mete en el filtro todas las columnas de la dimensión. Si solo necesito respetar el contexto, uso KEEPFILTERS:

```DAX
KEEPFILTERS_Color_Rojo =
CALCULATE ( [Cantidad], KEEPFILTERS ( dim_Producto[Color] = "Rojo" ) )
```

```DAX
VALUES_Color_Rojo =
CALCULATE (
    [Cantidad],
    dim_Producto[Color] = "Rojo",
    VALUES ( dim_Producto[Color] )
)
```

```DAX
DISTINCT_Color_Rojo =
CALCULATE (
    [Cantidad],
    dim_Producto[Color] = "Rojo",
    DISTINCT ( dim_Producto[Color] )
)
```

Aquí está lo interesante. `VALUES ( dim_Producto[Color] )` se evalúa **antes** de entrar en CALCULATE, con el contexto de la fila: en la fila Azul devuelve `{"Azul"}`. CALCULATE recibe dos filtros sobre la misma columna, `{"Rojo"}` y `{"Azul"}`, y los intersecta. Resultado: vacío.

Es el mismo efecto que KEEPFILTERS, escrito a mano. DISTINCT se comporta igual que VALUES; solo cambia cómo tratan la fila en blanco que aparece cuando hay claves huérfanas en la tabla de hechos.
