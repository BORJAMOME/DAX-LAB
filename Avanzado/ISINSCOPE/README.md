## ISINSCOPE

ISINSCOPE responde a una pregunta concreta: ¿esta columna es un nivel de agrupación en la celda que estoy calculando?

Con una jerarquía Año → Mes → Día en una matriz:

| Celda | `ISINSCOPE(Año)` | `ISINSCOPE(Mes)` | `ISINSCOPE(Día)` |
|---|---|---|---|
| Total general | FALSE | FALSE | FALSE |
| Fila de año | TRUE | FALSE | FALSE |
| Fila de mes | TRUE | TRUE | FALSE |
| Fila de día | TRUE | TRUE | TRUE |

En una fila de día, el mes también está "en scope": la celda pertenece a un mes y a un año concretos. Por eso, cuando uso ISINSCOPE en un SWITCH, **siempre pregunto primero por el nivel más bajo**. Si empiezo por Año, todas las filas entran en esa rama.

![ejemplo](/docs/imagenes/ISINSCOPE.png)

### Ejemplo: marcar el producto que más vende

```DAX
Max Ventas Producto =
MAXX ( ALLSELECTED ( dim_Producto[Producto] ), [Cantidad] )
```

```DAX
Ventas Inteligentes =
IF (
    ISINSCOPE ( dim_Producto[Producto] ),
    [Cantidad],
    [Max Ventas Producto]
)
```

En las filas de producto muestra la cantidad; en el total, el máximo (40.755 en la captura) en lugar de la suma.

```DAX
Producto es Máximo Ventas =
ISINSCOPE ( dim_Producto[Producto] )
    && [Cantidad] = [Max Ventas Producto]
```

```DAX
Icono Máximo =
IF ( [Producto es Máximo Ventas], "⬤" )
```

TRUE solo en la fila ganadora. El icono se puede usar tal cual en una tabla o como base de un formato condicional.

### SWITCH por niveles

El patrón que más reutilizo: una medida que cambia de cálculo según el nivel del drill-down.

```DAX
Ventas por Nivel =
SWITCH (
    TRUE (),
    ISINSCOPE ( dim_Producto[Producto] ),  [Cantidad],
    ISINSCOPE ( dim_Producto[Categoría] ), [Ventas Categoría],
    [Ventas Totales]
)
```

Producto va primero porque es el nivel más bajo.

### ISINSCOPE vs HASONEVALUE

HASONEVALUE devuelve TRUE si la columna tiene un único valor visible, venga de donde venga (fila del visual, slicer, filtro de página). ISINSCOPE solo mira las columnas de agrupación del visual. Para lógica de matrices uso ISINSCOPE; para tarjetas que dependen de un slicer, HASONEVALUE.
