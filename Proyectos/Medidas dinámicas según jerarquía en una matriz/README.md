# Medidas dinámicas según jerarquía en una matriz

Imagina una cadena de tiendas físicas que registra todas sus ventas por **Categoría**, **Tienda** y **Mes**. Queremos un informe en matriz donde:

1. Cada celda muestre el **importe de ventas**.
2. Se indique con un **símbolo especial** cuándo un valor es el máximo de su nivel jerárquico:
   - ● para la categoría con mayor venta dentro de una tienda.
   - ★ para la tienda con mayor venta dentro de un mes.
   - ▶ para el total general.
3. Se muestre el **porcentaje de contribución** al total correspondiente:
   - Porcentaje sobre ventas de la tienda (para categorías).
   - Porcentaje sobre ventas del mes (para tiendas).

Esta técnica permite una lectura rápida de los datos, destacando visualmente los valores clave sin necesidad de filtros adicionales.


![ejemplo](/docs/imagenes/matrix.png)


## Medidas Clave

### 1. Medida base de ventas


```
01_Ventas = SUM(Almacenes[Importe])
```

### 2. Ventas por contexto

Ventas por Mes ignorando Tienda

```
01_VentasMes = CALCULATE([01_Ventas], REMOVEFILTERS(dim_Tienda[Tienda]))
```

Ventas por Tienda ignorando Categoría
```
01_VentasTienda = CALCULATE([01_Ventas], REMOVEFILTERS(dim_Categoria[Categoría]))
```

### 3. Medida dinámica según jerarquía

```
01_Barras_Tiendas = IF(
    ISINSCOPE(dim_Categoria[Categoría]),
    [01_Ventas],
    IF(ISINSCOPE(dim_Tienda[Tienda]), BLANK(),
    BLANK()
    ))
```

### 4. Medida principal con símbolos y porcentajes

```
01_Contexto_Filtro_Jerarquía = 
VAR _VentasActual = [01_Ventas]

-- Máximo por categoría dentro de la tienda
VAR _MaxCategoria =
    CALCULATE(
        MAXX(VALUES(dim_Categoria[Categoría]), [01_Ventas]),
        REMOVEFILTERS(dim_Categoria)
    )

-- Máximo por tienda dentro del mes
VAR _MaxTienda =
    CALCULATE(
        MAXX(VALUES(dim_Tienda[Tienda]), [01_Ventas]),
        REMOVEFILTERS(dim_Tienda)
    )

-- Máximo por mes
VAR _MaxMes =
    CALCULATE(
        MAXX(VALUES('01_CALENDARIO'[Mes]), [01_Ventas]),
        REMOVEFILTERS('01_CALENDARIO')
    )

-- Porcentajes sobre total
VAR _VentasTotalTienda =
    CALCULATE(
        [01_Ventas],
        REMOVEFILTERS(dim_Categoria)
    )

VAR _VentasTotalMes =
    CALCULATE(
        [01_Ventas],
        REMOVEFILTERS(dim_Tienda)
    )

VAR _PctSobreTienda = DIVIDE(_VentasActual, _VentasTotalTienda)
VAR _PctSobreMes   = DIVIDE(_VentasActual, _VentasTotalMes)

RETURN
SWITCH(
    TRUE(),

    /* Nivel Categoría */
    ISINSCOPE(dim_Categoria[Categoría]),
        IF(_VentasActual = _MaxCategoria, "● ", "") 
        & FORMAT(_VentasActual, "#,##0.00 €") 
        & " | " 
        & FORMAT(_PctSobreTienda, "0.0 %"),

    /* Nivel Tienda */
    ISINSCOPE(dim_Tienda[Tienda]),
        IF(_VentasActual = _MaxTienda, "★ ", "") 
        & FORMAT(_VentasActual, "#,##0.00 €") 
        & " | " 
        & FORMAT(_PctSobreMes, "0.0 %"),

    /* Total general */
    "▶ " & FORMAT(_VentasActual, "#,##0.00 €")
)
```


📂 Se incluye el archivo PBIX para que puedas probarlo, tocarlo y verlo en acción.





