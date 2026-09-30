## ALLSELECTED

ALLSELECTED quita los filtros que vienen de las filas y columnas del visual, pero conserva los de slicers y filtros externos. Es la función para calcular "% sobre el total visible".

```
ALLSELECTED(
    [Tabla | Columna [, Columna…]]
)
```

Puedes usarla sobre:

- una tabla entera

- columnas concretas


### Ejemplo

Para comparar, el % sobre el total del modelo (ignora también los slicers):

```DAX
% Ventas ALL =
DIVIDE (
    [Ventas Totales],
    CALCULATE ( [Ventas Totales], ALL ( FactSales ) )
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

Numerador: ventas en el contexto de la celda (producto, mes o lo que haya en el visual).

Denominador: ventas del conjunto seleccionado. Respeta los slicers (año, región, cliente…) e ignora el desglose por filas y columnas.

Resultado: cada fila se divide entre el total visible, no el total del modelo. Si el usuario filtra una región, los porcentajes vuelven a sumar 100 %.

Un matiz: el tercer argumento de DIVIDE en `% Ventas ALLSELECTED` es 0. Lo quitaría: con 0, una fila sin ventas del total muestra 0 %; con blanco (valor por defecto) la fila desaparece, que suele ser lo esperado.


![ejemplo](/docs/imagenes/ALLSELECTED1.png)

![ejemplo](/docs/imagenes/ALLSELECTED2.png)











