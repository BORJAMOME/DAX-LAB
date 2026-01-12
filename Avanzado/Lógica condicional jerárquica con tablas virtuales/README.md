### Lógica condicional jerárquica con tablas virtuales

Cuando una regla de negocio debe aplicarse producto a producto, pero el informe permite ver categorías o subcategorías, necesitas forzar el cálculo al nivel correcto usando una tabla virtual y luego sumar el resultado.

Si no lo haces así, Power BI aplica la lógica después de agrupar y los números dejan de tener sentido.

#### Ejemplo:

Tienes una matriz con la jerarquía:

 - Categoría

 - Subcategoría

 - Producto

Y una métrica base:

```
Ventas =
SUM(Ventas[Importe])
```

Además, el negocio define otra métrica:

```
Ventas Color = 
    CALCULATE([Cantidad],
    dim_Producto[Color] IN { "Rojo", "Negro" }
)
```

Ahora se pide otra métrica de negocio:

Reglas:

- Si un producto no es rojo o negro, no cuenta

- Si las ventas de rojo o negro superan a las ventas totales, usa ventas totales

En caso contrario, usa ventas de rojo y negro. 


#### Medida: 

```
Ventas Netas Ajustadas = 

VAR TablaVirtualProducto =
    SUMMARIZE(
        Ventas,
        dim_Producto[Categoría],
        dim_Producto[Subcategoría],
        dim_Producto[Producto],
        "ResultadoLogica",
            IF(
                [Cantidad] < 0, BLANK(),
                IF(
                    [Ventas Color] < 0, BLANK(),
                    IF(
                        [Ventas Color] > [Cantidad],
                        [Cantidad],
                        [Ventas Color]
                    )
                )
            )
    )

VAR ResultadoFinal =
    SUMX(
        TablaVirtualProducto,
        [ResultadoLogica]
    )

RETURN
ResultadoFinal
```






