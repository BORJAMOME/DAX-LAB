## ISINSCOPE

Te permite mirar en qué nivel estás parado.

ISINSCOPE(columna) devuelve:

- TRUE → esa columna está activa en el nivel actual del visual

- FALSE → no lo está

### Lógica jerárquica 

ISINSCOPE(Calendario[Mes])

Año
 └─ Mes
     └─ Día

- Visual en Mes → TRUE

- Visual en Año → FALSE

- Visual en Día → FALSE


![ejemplo](/docs/imagenes/ISINSCOPE.png)

```
Ventas Inteligentes = 
IF(
    ISINSCOPE(dim_Producto[Producto]),
    [Cantidad],
    [Max Ventas Producto]
)
```

- En nivel Producto → muestra ventas reales

- En nivel superior (Categoría, Total) → muestra el máximo


```
Producto es Máximo Ventas = 
IF(
    ISINSCOPE(dim_Producto[Producto]),
    [Cantidad] = [Max Ventas Producto],
    FALSE()
)
```

¿Este producto es el que más vende?”

- TRUE solo en la fila ganadora

- FALSE en el resto


***Icono visual***

```
Icono Máximo = 
IF(
    [Producto es Máximo Ventas],
    "⬤",
    ""
)
```



----

### Otros usos interesanes


```
Icono Alerta =
IF(
    ISINSCOPE(dim_Producto[Producto]) &&
    [Margen] < 0,
    "⚠️",
    ""
)
```

```
Ventas Drill Inteligente =
SWITCH(
    TRUE(),
    ISINSCOPE(dim_Producto[Producto]), [Cantidad],
    ISINSCOPE(dim_Producto[Categoría]), [Ventas Categoría],
    [Ventas Totales]
)
```

```
Ranking Visible =
IF(
    ISINSCOPE(dim_Producto[Producto]),
    [Ranking Ventas],
    BLANK()
)
```

```
Variación vs Anterior =
IF(
    ISINSCOPE(Calendario[Mes]),
    [Ventas Mes] - [Ventas Mes Anterior],
    BLANK()
)
```

```
Marca Top N =
IF(
    ISINSCOPE(dim_Producto[Producto]),
    [Es Top N],
    BLANK()
)
```


***Mostrar texto explicativo dinámico***

```
Texto Contexto =
IF(
    ISINSCOPE(dim_Producto[Producto]),
    "Detalle por producto",
    "Vista agregada"
)
```












