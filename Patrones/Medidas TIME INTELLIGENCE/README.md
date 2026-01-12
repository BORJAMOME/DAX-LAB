### Medidas TIME INTELLIGENCE para ser reutilizadas

##### Caso:

Imagina que trabajamos en una empresa de suscripciones digitales y queremos analizar los ingresos generados por suscripciones.
Los problemas típicos que encontramos son:

 - Saber cuánto llevamos acumulado en el año (YTD).

 - Comparar con el mismo periodo del año anterior (SPLY).

 - Ver cómo van las ventas respecto al mes o trimestre pasado.

Estas medidas nos permiten responder esas preguntas de negocio sin rehacer cálculos cada vez.

#### 1. Ingresos acumulados en el año (YTD)

```
Ingresos Acumulados Año =
TOTALYTD(
    [Ingresos $],
    Calendario[Fecha]
)
```

#### 2. Ingresos mismo periodo año anterior (SPLY)

```
Ingresos SPLY =
CALCULATE(
    [Ingresos $],
    SAMEPERIODLASTYEAR(Calendario[Fecha])
)
```

#### Ingresos Periodo Anterior usando DATEADD

```
Ingresos Periodo Anterior =
CALCULATE(
    [Ingresos $],
    DATEADD(Calendario[Fecha], -12, MONTH)
)
```

#### 3. Ingresos mes pasado

```
Ingresos Mes Anterior =
CALCULATE(
    [Ingresos $],
    DATEADD(Calendario[Fecha], -1, MONTH)
)
```
#### 4. Ingresos trimestre pasado

```
Ingresos Trimestre Anterior =
CALCULATE(
    [Ingresos $],
    DATEADD(Calendario[Fecha], -1, QUARTER)
)
```

#### 5. Ingresos Últimos 30 Días

```
Ingresos Últimos 30 Días =
CALCULATE(
    [Ingresos $],
    DATESINPERIOD(Calendario[Fecha], MAX(Calendario[Fecha]), -30, DAY)
)
```

#### 6. Ingresos Últimos 90 Días

```
Ingresos Últimos 90 Días =
CALCULATE(
    [Ingresos $],
    DATESINPERIOD(Calendario[Fecha], MAX(Calendario[Fecha]), -90, DAY)
)
```

#### 7. Ingresos Año Anterior Acumulado (YTD SPLY)

```
Ingresos Acumulados Año Anterior =
CALCULATE(
    [Ingresos $],
    SAMEPERIODLASTYEAR(DATESYTD(Calendario[Fecha]))
)
```

#### 8. Ingresos Mes Actual vs Mes Anterior

```
Ingresos Mes Actual vs Anterior =
[Ingresos $] - CALCULATE([Ingresos $], DATEADD(Calendario[Fecha], -1, MONTH))
```



