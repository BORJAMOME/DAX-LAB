## ALLSELECTED

ALLSELECTED recalcula respetando lo que el usuario ha seleccionado, pero ignorando el nivel de detalle del visual.

```
ALLSELECTED(
    [Tabla | Columna [, Columna…]]
)
```

Puedes usarla sobre:

- una tabla entera

- columnas concretas


### Ejemplo

```
% Ventas II =
DIVIDE(
    [Ventas Totales],
    [Función ALL I],
    0
)
```


```
% Ventas ALLSELECTED = 
DIVIDE (
    SUMX (
        FactSales,
        FactSales[SalesQuantity] * FactSales[UnitPrice]
    ),
    SUMX (
        ALLSELECTED ( FactSales ),
        FactSales[SalesQuantity] * FactSales[UnitPrice]
    ),
    0
)
```

***¿Qué está pasando aquí?***

Numerador: Ventas en el contexto actual del visual

- Producto

- Mes

- Lo que toque

Denominador: Ventas totales del conjunto seleccionado

- Respeta slicers (Año, Región, Cliente…)

- Ignora el desglose por filas/columnas

Resultado: Cada fila se divide entre el total visible, no el total del modelo.


![ejemplo](/docs/imagenes/ALLSELECTED1.png)

![ejemplo](/docs/imagenes/ALLSELECTED2.png)











