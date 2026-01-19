## Columnas Max y Min Resaltado

Este ejemplo muestra cómo resaltar visualmente el valor máximo y mínimo en un gráfico de columnas utilizando medidas DAX, tanto a nivel de color como de etiquetas.

El resultado es muy claro:

- 🟦 la barra con mayor valor se muestra en azul

- 🟥 la barra con menor valor se muestra en rojo

- ⚪ el resto de barras quedan en gris

- solo máximo y mínimo muestran etiqueta de valor


![ejemplo](/docs/imagenes/InformeMAXMin.png)


### Ventas

```
Ventas = 
SUMX(
    FactVentas,
    FactVentas[Precio] * FactVentas[Unidades]
)
```

### Color Máx/Mín

```
Color Máx/Mín = 
VAR MaxValor =
    MAXX(
        ALLSELECTED(DimProductos[Producto]),
        [Ventas]
    )
VAR MinValor =
    MINX(
        ALLSELECTED(DimProductos[Producto]),
        [Ventas]
    )
VAR ValorActual = [Ventas]
RETURN
SWITCH(
    TRUE(),
    ValorActual = MaxValor, "#066fa0",   // Azul → máximo
    ValorActual = MinValor, "#a3223e",   // Rojo → mínimo
    "#888888"                            // Gris → resto
)
```

### Etiqueta Máx/Mín

```
Etiqueta Máx/Mín = 
VAR MaxValor =
    MAXX(
        ALLSELECTED(DimProductos[Producto]),
        [Ventas]
    )
VAR MinValor =
    MINX(
        ALLSELECTED(DimProductos[Producto]),
        [Ventas]
    )
VAR ValorActual = [Ventas]
RETURN
IF(
    ValorActual = MaxValor || ValorActual = MinValor,
    ValorActual,
    BLANK()
)
```

### Formato color (etiquetas)

```
Formato color = 
VAR _Ventas = [Ventas]
VAR _Max =
    MAXX(
        ALLSELECTED(DimProductos[Producto]),
        [Ventas]
    )
VAR _MIN =
    MINX(
        ALLSELECTED(DimProductos[Producto]),
        [Ventas]
    )
RETURN
SWITCH(
    TRUE(),
    _Ventas = _Max, "#066fa0",
    _Ventas = _MIN, "#a3223e",
    "#f8f8f8"
)
```

### Máximo global ignorando el calendario

```
YM_MAX_ALL_CALENDAR = 
CALCULATE (
    MAXX(
        VALUES(CALENDARIO[Date]),
        [Ventas]
    ),
    ALL('CALENDARIO')
)
```


📂 Se incluye el archivo PBIX para que puedas probarlo, tocarlo y verlo en acción.

