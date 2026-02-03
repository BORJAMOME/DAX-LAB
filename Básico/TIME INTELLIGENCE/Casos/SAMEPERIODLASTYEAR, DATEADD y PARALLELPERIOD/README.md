## SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD

### Análisis de Unidades Vendidas por día laborable y comparación interanual

**Contexto**

Empresa reatil con tiendas físicas. La dirección quieren saber si se vende mejor o peor que el año pasado.
Importante: No todos los días son iguales (fines de semana y festivo distorsionan el análisis)


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

***Medidas de Time Intelligence***


#### DATEADD

```
UnidadesVendidas/día DATEADD = 
CALCULATE (
    [UnidadesVendidas / día],
    DATEADD ( dim_Calendario[Fecha], -1, YEAR )
)
```

- Desplaza el contexto de fechas exactamente 1 año atrás
- Respeta exactamente las fechas seleccionadas


####  SAMEPERIODLASTYEAR

```
UnidadesVendidas/día SAMEPERIODLASTYEAR = 
CALCULATE (
    [UnidadesVendidas / día],
    SAMEPERIODLASTYEAR ( dim_Calendario[Fecha] )
)
```

- Devuelve el mismo periodo exacto del año anterior
- Internamente se parece a DATEADD, pero con reglas más estrictas


#### PARALLELPERIOD

```
UnidadesVendidas/día PARALLELPERIOD = 
CALCULATE (
    [UnidadesVendidas / día],
    PARALLELPERIOD ( dim_Calendario[Fecha], -1, YEAR )
)
```

- Va al periodo completo equivalente
- Ignora el rango exacto de días
- Trabaja por bloques de tiempo














