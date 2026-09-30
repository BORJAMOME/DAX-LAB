## Contexto de filtro I

Misma métrica, dos tarjetas: una muestra el total y otra solo lo rojo. Lo único que cambia es el contexto de filtro.

```DAX
Cantidad = SUM ( Ventas[Cantidades] )
```

```DAX
Color_Rojo =
CALCULATE (
    [Cantidad],
    dim_Producto[Color] = "Rojo"
)
```

`Cantidad` responde a "¿cuántas unidades se han vendido?". `Color_Rojo` hace la misma pregunta añadiendo "…de productos rojos". CALCULATE no cambia el cálculo, cambia el conjunto de filas sobre el que se hace.

![filtro](/docs/imagenes/Contexto_Filtro.png)

Siguiente paso: [Contexto de filtro II](<../Contexto de filtro II/>), donde la misma idea da cinco resultados distintos según cómo se escriba el filtro.
