## Creación de calendarios en DAX: distintas formas de construir una tabla de fechas

En Power BI no existe una única forma “correcta” de crear una tabla calendario.
Dependiendo del contexto, del tamaño del modelo y del objetivo del análisis, hay varios enfoques válidos, cada uno con sus ventajas y limitaciones.

En este repositorio muestro distintas maneras de crear una tabla de fechas en DAX, partiendo de ejemplos sencillos y avanzando hacia escenarios más dinámicos.


Todas las soluciones parten del mismo concepto:

1. Generar un rango de fechas (CALENDAR o CALENDARAUTO)

2. Iterar por cada día

3. Enriquecer cada fecha con columnas útiles para el análisis

La diferencia está en cómo se genera el rango y qué información se añade.


### Calendario básico con CALENDAR + GENERATE

```
Calendario_1 = 
GENERATE (
    CALENDAR ( DATE ( 2017, 1, 1 ), DATE ( 2017, 12, 31 ) ),
    VAR FechaActual = [Date]
    RETURN ROW (
        "Día", DAY(FechaActual),
        "Mes", MONTH(FechaActual),
        "Año", YEAR(FechaActual)
    )
)
```

### Calendario básico con ADDCOLUMNS (forma más limpia)

```
Calendario_2 = 
ADDCOLUMNS(
    CALENDAR( DATE(2017, 1, 1), DATE(2017, 12, 31) ),
    "Día", DAY([Date]),
    "Mes", MONTH([Date]),
    "Año", YEAR([Date])
)
```

### Calendario automático con CALENDARAUTO

```
Calendario_3 = 
GENERATE (
    CALENDARAUTO(),
    VAR FechaActual = [Date]
    RETURN ROW (
        "Día", DAY(FechaActual),
        "Mes", MONTH(FechaActual),
        "Año", YEAR(FechaActual)
    )
)
```

### Calendario dinámico basado en la fecha actual (NOW())

```
Calendario_4 = 
GENERATE (
    CALENDAR(
        DATE( YEAR(NOW()) - 1, MONTH(NOW()), DAY(NOW()) ),
        NOW()
    ),
    VAR FechaActual = [Date]
    RETURN ROW (
        "Día", DAY(FechaActual),
        "Mes", MONTH(FechaActual),
        "Año", YEAR(FechaActual)
    )
)
```

### Calendario avanzado con índices temporales

```
Calendario_5 = 
GENERATE (
    CALENDAR(
        DATE( YEAR(TODAY()) - 2, MONTH(TODAY()), DAY(TODAY()) ),
        TODAY()
    ),
    VAR FechaActual = [Date]
    VAR AñoActual = YEAR(TODAY())
    VAR MesActual = MONTH(TODAY())
    VAR IndiceDía = DATEDIFF(FechaActual, TODAY(), DAY) * -1
    VAR InicioSemana = 1
    VAR NumeroDiaHoy = WEEKDAY(TODAY())
    VAR IndiceSemana =
        INT(
            ROUNDDOWN(
                (IndiceDía +
                -1 * IF(
                    NumeroDiaHoy + InicioSemana <= 6,
                    NumeroDiaHoy + InicioSemana,
                    NumeroDiaHoy + InicioSemana - 7
                )) / 7,
                0
            )
        )
    RETURN ROW (
        "Día", DAY(FechaActual),
        "Mes", MONTH(FechaActual),
        "Año", YEAR(FechaActual),
        "Índice Día", IndiceDía,
        "Índice Semana", IndiceSemana,
        "Número Día Semana", InicioSemana,
        "Índice Mes", INT((YEAR(FechaActual) - AñoActual) * 12 + MONTH(FechaActual) - MesActual),
        "Índice Año", YEAR(FechaActual) - AñoActual
    )
)
```

