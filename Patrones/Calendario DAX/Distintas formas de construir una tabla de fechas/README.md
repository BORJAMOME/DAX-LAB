## Creación de calendarios en DAX: distintas formas de construir una tabla de fechas

En Power BI no existe una única forma “correcta” de crear una tabla calendario.
Dependiendo del contexto, del tamaño del modelo y del objetivo del análisis, hay varios enfoques válidos, cada uno con sus ventajas y limitaciones.

Aquí recojo cinco formas de crear una tabla de fechas en DAX, de la más simple a la más dinámica. Las versiones en Power Query están en [Calendario con M](<../Calendario con M/>) y [Creación Calendario](<../Creación Calendario/>).


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

### Cuál uso y cuándo

- **Power Query / Dataflow** cuando el calendario se comparte entre informes o el modelo es grande: se calcula una vez en el refresco y queda documentado en un solo sitio.
- **DAX con CALENDAR** para prototipos y modelos pequeños, o cuando necesito que el rango dependa de las propias tablas del modelo.
- **CALENDARAUTO**, casi nunca: busca todas las columnas de fecha del modelo, incluidas fechas de nacimiento o de alta, y el rango se dispara.
- Las versiones con `NOW()` o `TODAY()` se recalculan en cada refresco. Útil para índices relativos ("hace 3 meses"), pero hay que tenerlo en cuenta si el informe se refresca con poca frecuencia.

En todos los casos, después de crearla: **marcar como tabla de fechas** y relacionarla con los hechos por la columna de fecha.
