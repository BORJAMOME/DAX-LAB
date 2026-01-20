## Análisis comparativo de ventas mensuales vs promedio

Este ejemplo muestra cómo construir en Power BI con DAX un gráfico dinámico que compara las ventas mensuales frente al promedio anual, resaltando visualmente:

- meses por encima del promedio

- meses por debajo del promedio

El resultado es un gráfico fácil de leer, muy potente para análisis de rendimiento y perfecto para storytelling.

### Objetivo del análisis

¿Qué meses han rendido mejor o peor que la media del año?


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


![ejemplo](/docs/imagenes/Analisis_1.mov)


