## WINDOW

WINDOW crea una ventana de filas alrededor de la fila actual.

- No devuelve un número → devuelve una tabla

- Se usa dentro de CALCULATE

- Sirve para:

  - Acumulados

  - Medias móviles

  - Comparaciones avanzadas

  - Análisis progresivos

--> ***OFFSET*** mira una fila
--> ***WINDOW*** mira un bloque de filas


### Estructura de la medida:

```
WINDOW(
    start_offset,
    end_offset,
    tabla,
    ORDERBY(expresión, ASC | DESC)
)
```

- ***start_offset*** → desde dónde empiezas

- ***end_offset*** → hasta dónde llegas

- ***0*** → fila actual

- ***Negativo*** → hacia atrás

- ***Positivo*** → hacia adelante

### Ejemplo:

```
Ventas Acumuladas WINDOWS = 
CALCULATE(
    [Cantidad],
    WINDOW(
        -1000,
        0,
        ALL(dim_Producto[Color]),
        ORDERBY([Cantidad], DESC)
    )
)
```

¿Por qué -1000?

- Es un “desde el inicio”
- DAX corta en el primer registro real


![ejemplo](/docs/imagenes/WINDOW1.png)




--- 
### Caso: media móvil (3 elementos)

```
Media Móvil 3 =
CALCULATE(
    AVERAGEX(
        WINDOW(
            -2,
            0,
            ALLSELECTED(dim_Calendario[AñoMes]),
            ORDERBY(dim_Calendario[AñoMes], ASC)
        ),
        [Cantidad]
    )
)
```


### Caso: ventas acumuladas de los Top 5 productos (WINDOW + Top N)

```
Ventas Acumuladas Top 5 =
CALCULATE(
    [Cantidad],
    WINDOW(
        0,
        4,
        ALL(dim_Producto[Producto]),
        ORDERBY([Cantidad], DESC)
    )
)
```

- Ordena productos por ventas

- Selecciona las 5 primeras filas

- Calcula ventas solo para ese grupo

![ejemplo](/docs/imagenes/WINDOW2.png)



### Caso: % acumulado por ranking (WINDOW + % acumulado (Pareto))

```
% Acumulado Ventas =
VAR VentasAcumuladas =
    CALCULATE(
        [Cantidad],
        WINDOW(
            -1000,
            0,
            ALL(dim_Producto[Producto]),
            ORDERBY([Cantidad], DESC)
        )
    )

VAR VentasTotales =
    CALCULATE(
        [Cantidad],
        ALL(dim_Producto)
    )

RETURN
    DIVIDE(VentasAcumuladas, VentasTotales)
```

- Primer producto → % pequeño

- A medida que bajas → % crece

- Detectas dónde se alcanza el 80%


![ejemplo](/docs/imagenes/WINDOW3.png)



### Caso: WINDOW + Ranking dinámico

```
Ranking Dinámico =
RANKX(
    ALLSELECTED(dim_Producto[Producto]),
    [Cantidad],
    ,
    DESC,
    DENSE
)
```

Comparar contra los 3 anteriores.

```
Ventas Últimos 3 en Ranking =
CALCULATE(
    [Cantidad],
    WINDOW(
        -3,
        -1,
        ALLSELECTED(dim_Producto[Producto]),
        ORDERBY([Cantidad], DESC)
    )
)
```

--> Dame las ventas de los 3 productos que están justo por encima en el ranking

![ejemplo](/docs/imagenes/WINDOW4.png)










