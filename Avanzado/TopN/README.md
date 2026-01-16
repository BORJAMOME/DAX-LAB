## TOPN

![ejemplo](/docs/imagenes/topn.png)



### Top 3 productos más vendidos

```
TOP3 Productos = 
VAR vTop3 = 
    TOPN(
        3,
        'dim_Producto',
        [Cantidad]
    )
RETURN
    CALCULATE(
        [Cantidad],
        vTop3
    )
```

- TOPN crea una tabla virtual con los 3 productos con más ventas

- CALCULATE dice: “Calcula Cantidad solo para estos productos”


### Producto Top Ventas
Queremos el nombre del producto ganador, no la cantidad.

```
Producto Top Ventas =
VAR vTop1 =
    TOPN(
        1,
        'dim_Producto',
        [Cantidad]
    )
RETURN
    MAXX(
        vTop1,
        'dim_Producto'[Producto]
    )
```

### Ventas Top Dinámico (Top N por selector)

Creamos una tabla desconectada:

```
Selector Top N =
DATATABLE(
    "Top", INTEGER,
    {
        {1},
        {5},
        {10}
    }
)
```


Medida dinámica:

```
Ventas Top Dinámico =
VAR N =
    SELECTEDVALUE(Selector Top N[Top], 3)

VAR vTopN =
    TOPN(
        N,
        'dim_Producto',
        [Cantidad]
    )
RETURN
    CALCULATE(
        [Cantidad],
        vTopN
    )
```

### * Extra: Mostrar ● si el producto está en el Top N en la tabla

```
Marca Top Dinámico =
VAR N =
    SELECTEDVALUE('Selector Top N'[Top], 3)

VAR vTopN =
    TOPN(
        N,
        ALL('dim_Producto'),
        [Cantidad]
    )

VAR ProductoActual =
    SELECTEDVALUE('dim_Producto'[Producto])

RETURN
    IF(
        CONTAINS(
            vTopN,
            'dim_Producto'[Producto],
            ProductoActual
        ),
        "●",
        BLANK()
    )
```






