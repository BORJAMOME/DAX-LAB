## SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD

### Análisis de Unidades Vendidas por día laborable y comparación interanual

Una cadena de tiendas físicas quiere saber si vende mejor o peor que el año pasado. Comparar unidades totales engaña: un mes con cinco sábados o con Semana Santa no es comparable con el mismo mes del año anterior. Por eso normalizo por día laborable antes de comparar.


#### 1.Contar días laborables

```
Laborable = 
CALCULATE(
    DISTINCTCOUNT(dim_Calendario[Fecha]),
    dim_Calendario[Es Laborable] = "Laborable"
)
```

#### 2.Unidades vendidas totales

```
UnidadesVendidas = 
SUM(fact_ventas[Cantidad])
```

#### 3.Unidades vendidas por día laborable

```
UnidadesVendidas / día = 
DIVIDE([UnidadesVendidas], [Laborable], 0)
```

### Medidas de Time Intelligence


#### DATEADD

```
UnidadesVendidas/día DATEADD = 
CALCULATE (
    [UnidadesVendidas / día],
    DATEADD ( dim_Calendario[Fecha], -1, YEAR )
)
```

- Desplaza las fechas del contexto un año atrás, día a día.
- Si en el contexto hay del 1 al 15 de marzo, devuelve del 1 al 15 de marzo del año anterior.


####  SAMEPERIODLASTYEAR

```
UnidadesVendidas/día SAMEPERIODLASTYEAR = 
CALCULATE (
    [UnidadesVendidas / día],
    SAMEPERIODLASTYEAR ( dim_Calendario[Fecha] )
)
```

- Es exactamente `DATEADD ( …, -1, YEAR )` con otro nombre.
- La uso cuando solo necesito el año anterior, porque se lee mejor.


#### PARALLELPERIOD

```
UnidadesVendidas/día PARALLELPERIOD = 
CALCULATE (
    [UnidadesVendidas / día],
    PARALLELPERIOD ( dim_Calendario[Fecha], -1, YEAR )
)
```

- Desplaza y **amplía** al periodo completo: si en el contexto hay del 1 al 15 de marzo, devuelve el año anterior *entero*.
- Útil para comparar contra el total del año anterior, no contra el mismo tramo.

La diferencia solo se nota cuando el contexto no es un año completo. En una fila de mes, DATEADD y SAMEPERIODLASTYEAR devuelven el mismo mes del año anterior; PARALLELPERIOD con YEAR devuelve el año anterior completo en todas las filas.



![ejemplo](/docs/imagenes/TI.png)

En la captura se ve claro: en cada mes de 2023, SAMEPERIODLASTYEAR y DATEADD devuelven el mismo mes de 2022 (enero: 5.662,67), mientras PARALLELPERIOD repite 4.090,22, la media de todo 2022, en las doce filas.










