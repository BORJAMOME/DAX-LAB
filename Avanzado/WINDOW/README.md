## WINDOW

WINDOW devuelve un bloque de filas de una tabla ordenada. OFFSET mira una fila vecina; WINDOW mira un rango. Con eso salen acumulados, medias móviles y Pareto sin trucos con fechas.

```DAX
WINDOW (
    <desde>, [<ABS | REL>],
    <hasta>, [<ABS | REL>],
    <tabla>,
    ORDERBY ( <expresión>, [ASC | DESC] )
)
```

La clave está en ABS y REL:

- **REL** (por defecto): posición relativa a la fila actual. `0` es la fila actual, `-1` la anterior.
- **ABS**: posición absoluta en la tabla ordenada. `1` es la primera fila, `-1` la última.

![ejemplo](/docs/imagenes/WINDOW1.png)

### Acumulado desde el primero

```DAX
Ventas Acumuladas =
CALCULATE (
    [Cantidad],
    WINDOW (
        1, ABS,
        0, REL,
        ALLSELECTED ( dim_Producto[Color] ),
        ORDERBY ( [Cantidad], DESC )
    )
)
```

Desde la primera fila (`1, ABS`) hasta la actual (`0, REL`). Antes escribía `-1000, 0` para decir "desde el principio"; funciona porque WINDOW recorta, pero `1, ABS` dice lo mismo sin número mágico y no se rompe si un día hay más de mil filas.

### Media móvil de 3 meses

```DAX
Media Móvil 3 =
AVERAGEX (
    WINDOW (
        -2, REL,
        0, REL,
        ALLSELECTED ( dim_Calendario[AñoMes] ),
        ORDERBY ( dim_Calendario[AñoMes], ASC )
    ),
    [Cantidad]
)
```

WINDOW devuelve los tres meses (los dos anteriores y el actual) y AVERAGEX promedia la medida en cada uno. En los dos primeros meses la ventana es más corta y la media se hace con los que haya.

### Top 5 fijo

```DAX
Ventas Top 5 =
CALCULATE (
    [Cantidad],
    WINDOW (
        1, ABS,
        5, ABS,
        ALL ( dim_Producto[Producto] ),
        ORDERBY ( [Cantidad], DESC )
    )
)
```

Posiciones 1 a 5 en absoluto: el mismo valor en todas las filas, la suma de los cinco productos que más venden.

![ejemplo](/docs/imagenes/WINDOW2.png)

> La captura corresponde a una versión anterior escrita con `WINDOW ( 0, 4, … )`. Con posiciones relativas, eso suma **la fila actual y las cuatro siguientes**, no el Top 5: por eso el valor cambia en cada fila. Es un error fácil de cometer y lo dejo documentado.

### Pareto: % acumulado

```DAX
% Acumulado Ventas =
VAR VentasAcumuladas =
    CALCULATE (
        [Cantidad],
        WINDOW (
            1, ABS,
            0, REL,
            ALLSELECTED ( dim_Producto[Producto] ),
            ORDERBY ( [Cantidad], DESC )
        )
    )
VAR VentasTotales =
    CALCULATE ( [Cantidad], ALLSELECTED ( dim_Producto[Producto] ) )
RETURN
    DIVIDE ( VentasAcumuladas, VentasTotales )
```

El primer producto tiene un % pequeño y va creciendo hasta el 100 %. La fila donde se cruza el 80 % marca cuántos productos concentran la mayor parte de la venta.

![ejemplo](/docs/imagenes/WINDOW3.png)

### Los tres de arriba en el ranking

```DAX
Ventas 3 Anteriores en Ranking =
CALCULATE (
    [Cantidad],
    WINDOW (
        -3, REL,
        -1, REL,
        ALLSELECTED ( dim_Producto[Producto] ),
        ORDERBY ( [Cantidad], DESC )
    )
)
```

Suma los tres productos que están justo por encima del actual. Sirve para ver cuánto le falta a un producto para subir posiciones.

![ejemplo](/docs/imagenes/WINDOW4.png)

### Cosas a vigilar

- Si hay empates en la expresión de ORDERBY, WINDOW puede dar error por orden ambiguo. Añadir una segunda columna al ORDERBY (el propio nombre del producto) lo resuelve.
- WINDOW no necesita estar dentro de CALCULATE: es una función de tabla y se puede usar en cualquier iterador, como en la media móvil.
