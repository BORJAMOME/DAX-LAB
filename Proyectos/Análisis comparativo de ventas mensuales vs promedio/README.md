## Análisis comparativo de ventas mensuales vs promedio

Este ejemplo muestra cómo construir en Power BI con DAX un gráfico dinámico que compara las ventas mensuales frente al promedio anual, resaltando visualmente:

- meses por encima del promedio

- meses por debajo del promedio

La pregunta que responde: qué meses han rendido mejor o peor que la media del año.

![ejemplo](/docs/imagenes/analisis_1.png)




### Medidas


**Ventas**

```
Ventas =
SUMX(
    FactVentas,
    FactVentas[Precio] * FactVentas[Unidades]
)
```

**Promedio de ventas**

```
PromedioVentas =
CALCULATE(
    AVERAGEX(
        VALUES(CALENDARIO[Mes]),
        [Ventas]
    ),
    ALLEXCEPT(CALENDARIO, CALENDARIO[Año])
)
```

El gráfico es de columnas apiladas con dos series: la de abajo llega hasta la media (o hasta la venta, si el mes queda por debajo) y la de arriba solo existe en los meses que la superan. Con colores distintos, el exceso sobre la media se ve sin leer números.

**Barra superior**

```
Barra superior =
IF(
    [Ventas] > [PromedioVentas],
    [Ventas] - [PromedioVentas],
    BLANK()
)
```

**Barra inferior**

```
Barra Inferior =
MIN(
    [Ventas],
    [PromedioVentas]
)


```



El `.pbix` está en esta misma carpeta.
