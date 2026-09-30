## Medidas de Time Intelligence reutilizables

El set de medidas temporales que copio al empezar un informe nuevo. El ejemplo es una empresa de suscripciones digitales que quiere saber:

 - Saber cuánto llevamos acumulado en el año (YTD).

 - Comparar con el mismo periodo del año anterior (SPLY).

 - Ver cómo van las ventas respecto al mes o trimestre pasado.

Requisito para que funcionen: una tabla `Calendario` marcada como tabla de fechas, con días consecutivos sin huecos y relacionada con los hechos. Sin eso, las funciones de Time Intelligence devuelven resultados erróneos sin avisar.

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

Es lo mismo que `DATEADD ( Calendario[Fecha], -1, YEAR )` o `-12, MONTH`. Me quedo con SAMEPERIODLASTYEAR porque el nombre dice lo que hace.

#### 3. Ingresos mes anterior

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
[Ingresos $] - [Ingresos Mes Anterior]
```

Reutiliza la medida del punto 3 en lugar de repetir el CALCULATE. Si mañana cambia la definición de "mes anterior", se cambia en un sitio.

#### 9. Variación % interanual

```
Ingresos Var % YoY =
DIVIDE ( [Ingresos $] - [Ingresos SPLY], [Ingresos SPLY] )
```

DIVIDE devuelve blanco si no hay año anterior, en lugar de un error de división entre cero.

#### Cuándo pasar a grupos de cálculo

Con cinco medidas base y ocho variantes temporales salen 40 medidas casi iguales. En ese punto muevo la lógica a un **grupo de cálculo** (YTD, SPLY, MoM… como elementos) y cada variante se aplica a cualquier medida. Estas medidas sueltas siguen siendo la mejor forma de empezar y de entender qué hace cada función.



