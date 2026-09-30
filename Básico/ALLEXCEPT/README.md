## ALLEXCEPT

ALLEXCEPT quita todos los filtros de una tabla menos los de las columnas que le indicas. Lo uso para totales de grupo: "total del país de esta fila", sea cual sea el producto, el color o la tienda.

![ejemplo](/docs/imagenes/ALLEXCEPT.png)

En una matriz País × Color, la medida devuelve en cada celda el total del país, ignorando el color. Dividir `[Cantidad]` entre ella da el peso de cada color dentro de su país.

```DAX
ALLEXCEPT_País =
CALCULATE (
    [Cantidad],
    ALLEXCEPT ( Ventas, DIM_Clientes[País] )
)
```

- CALCULATE recalcula la medida [Cantidad] bajo un nuevo contexto.

- ALLEXCEPT elimina todos los filtros que llegan a la tabla Ventas (incluidos los de sus dimensiones relacionadas), menos el de país.

### Diferencia clave:

- ALL(Ventas) eliminaría todos los filtros, país incluido.

- ALLEXCEPT mantiene los que eliges.


### Cuándo no lo uso

- Si solo quiero ignorar una o dos columnas, `REMOVEFILTERS ( dim_Producto[Color] )` es más claro: dice qué se quita en lugar de qué se queda.
- ALLEXCEPT también borra filtros de slicers que el usuario sí quiere respetar (año, canal…). Si el informe tiene slicers, suelo preferir `REMOVEFILTERS` de las columnas concretas, o `CALCULATE ( [Cantidad], REMOVEFILTERS ( Ventas ), VALUES ( DIM_Clientes[País] ) )` cuando el país no está en el visual y quiero mantener igualmente el país seleccionado.
