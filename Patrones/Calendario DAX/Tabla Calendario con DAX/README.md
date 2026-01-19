## Tabla Calendario con DAX

Este calendario se ha creado mediante una nueva tabla en DAX, sin depender de Power Query, con el objetivo de disponer de una tabla de fechas completa, dinámica y reutilizable, válida para análisis temporales, comparativas, fiscalidad y festivos nacionales.

Es una solución autocontenida: al actualizar el modelo, el calendario se adapta automáticamente al año actual.

### Definición del rango temporal

```
VAR _AnioMin = YEAR(TODAY()) - 10
VAR _AnioMax = YEAR(TODAY()) + 3
```

### Generación de la tabla base

```
CALENDAR(
    DATE(_AnioMin, 1, 1),
    DATE(_AnioMax, 12, 31)
)
```


# Columnas del calendario

| Categoría          | Columna            | Descripción                                               |
| ------------------ | ------------------ | --------------------------------------------------------- |
| Base            | Date               | Fecha completa. Una fila por cada día del rango definido. |
| Base            | Año                | Año natural de la fecha.                                  |
| Base            | Mes                | Número de mes (1–12).                                     |
| Base            | Nombre del mes     | Nombre del mes en español.                                |
| Base            | Nombre Mes Ingles  | Nombre del mes en inglés.                                 |
| Base            | Día                | Día del mes (1–31).                                       |
| Base            | Nombre del día     | Nombre del día de la semana en español.                   |
| Base            | Nombre Día Ingles  | Nombre del día de la semana en inglés.                    |
| Base            | Día de la semana   | Número del día de la semana (1–7).                        |
| Identificadores | IdFecha            | Identificador de fecha en formato `YYYYMMDD`.             |
| Identificadores | IDFechaEntero      | Fecha convertida a entero. Útil para cálculos de desvío.  |
| Trimestres      | Trimestre          | Trimestre natural del año (1–4).                          |
| Trimestres      | Trimestre TX       | Trimestre en formato texto (T1, T2…).                     |
| Trimestres      | Trimestre Ingles   | Trimestre en formato inglés (Q1, Q2…).                    |
| Semanas         | Semana del año     | Número de semana del año.                                 |
| Semanas         | Semana del año ISO | Número de semana según norma ISO-8601.                    |
| Laboral         | Es Laborable       | Indica si el día es laborable o no (lunes–viernes).       |
| Fiscal          | AñoFiscal          | Año fiscal según el mes de inicio configurado.            |
| Fiscal          | MesFiscal          | Mes fiscal (1–12).                                        |
| Fiscal          | TrimestreFiscal    | Trimestre fiscal (1–4).                                   |
| Referencias     | Hoy                | Fecha actual en el momento de la actualización.           |
| Referencias     | AñoActual          | Año actual.                                               |
| Referencias     | MesActual          | Mes actual.                                               |
| Referencias     | DíaActual          | Día actual del mes.                                       |
| Desvíos         | DesvíoAño          | Diferencia en años respecto a la fecha actual.            |
| Desvíos         | DesvíoMes          | Diferencia en meses respecto al mes actual.               |
| Desvíos         | DesvíoDía          | Diferencia en días respecto a hoy.                        |
| Festivos        | Festivo            | Nombre del festivo nacional o “No Festivo”.               |




```
TablaCALENDAR = 
VAR _AnioMin = YEAR(TODAY()) - 10
VAR _AnioMax = YEAR(TODAY()) + 3
VAR _MesFiscal = 7
RETURN
ADDCOLUMNS(
    CALENDAR(
        DATE(_AnioMin, 1, 1),
        DATE(_AnioMax, 12, 31)
    ),
    "Año", YEAR([Date]),
    "Mes", MONTH([Date]),
    "Nombre del mes", FORMAT([Date], "mmmm"),
    "Nombre Mes Ingles", FORMAT([Date], "mmmm", "en-US"),
    "Día", DAY([Date]),
    "Nombre del día", FORMAT([Date], "dddd"),
    "Nombre Día Ingles", FORMAT([Date], "dddd", "en-US"),
    "IdFecha", YEAR([Date]) * 10000 + MONTH([Date]) * 100 + DAY([Date]),
    "Día de la semana", WEEKDAY([Date]),
    "Trimestre", QUARTER([Date]),
    "Trimestre TX", "T" & QUARTER([Date]),
    "Trimestre Ingles", "Q" & QUARTER([Date]),
    "Semana del año", WEEKNUM([Date]),
    "Semana del año ISO", WEEKNUM([Date], 21),
    "Es Laborable", IF(WEEKDAY([Date], 2) <= 5, "Laborable", "No laborable"),

    // Año fiscal
    "AñoFiscal", IF(MONTH([Date]) >= _MesFiscal, YEAR([Date]), YEAR([Date]) - 1),

    // Mes fiscal (1..12)
    "MesFiscal",
        VAR _m = MONTH([Date])
        RETURN MOD(_m - _MesFiscal, 12) + 1,

    // Trimestre fiscal
    "TrimestreFiscal",
        VAR _m2 = MONTH([Date])
        VAR _mesFiscal = MOD(_m2 - _MesFiscal, 12) + 1
        RETURN INT((_mesFiscal + 2) / 3),

    // Fechas de referencia
    "Hoy", TODAY(),
    "AñoActual", YEAR(TODAY()),   
    "MesActual", MONTH(TODAY()),   
    "DíaActual", DAY(TODAY()),

    // Desvíos
    "DesvíoAño", YEAR([Date]) - YEAR(TODAY()),
    "DesvíoMes", (YEAR([Date]) - YEAR(TODAY()))*12 + MONTH([Date]) - MONTH(TODAY()),
    "IDFechaEntero", INT([Date]),
    "DesvíoDía", INT([Date]) - INT(TODAY()),

    // Festivos nacionales principales
    "Festivo",
        VAR Anio = YEAR([Date])
        VAR A = MOD(Anio, 19)
        VAR B = INT(Anio / 100)
        VAR C = MOD(Anio, 100)
        VAR D = INT(B / 4)
        VAR E = MOD(B, 4)
        VAR F = INT((B + 8) / 25)
        VAR G = INT((B - F + 1) / 3)
        VAR H = MOD(19 * A + B - D - G + 15, 30)
        VAR I = INT(C / 4)
        VAR K = MOD(C, 4)
        VAR L = MOD(32 + 2 * E + 2 * I - H - K, 7)
        VAR M = INT((A + 11 * H + 22 * L) / 451)
        VAR MesPascua = INT((H + L - 7 * M + 114) / 31)
        VAR DiaPascua = MOD((H + L - 7 * M + 114), 31) + 1
        VAR ViernesSanto = DATE(Anio, MesPascua, DiaPascua) - 2
        RETURN
            SWITCH(
                TRUE(),
                [Date] = DATE(Anio, 1, 1), "Año Nuevo",
                [Date] = DATE(Anio, 1, 6), "Día de Reyes",
                [Date] = ViernesSanto - 1, "Jueves Santo",
                [Date] = ViernesSanto, "Viernes Santo",
                [Date] = DATE(Anio, 5, 1), "Día del Trabajador",
                [Date] = DATE(Anio, 8, 15), "Asunción de la Virgen",
                [Date] = DATE(Anio, 10, 12), "Fiesta Nacional de España",
                [Date] = DATE(Anio, 11, 1), "Día de Todos los Santos",
                [Date] = DATE(Anio, 12, 6), "Día de la Constitución Española",
                [Date] = DATE(Anio, 12, 8), "Inmaculada Concepción",
                [Date] = DATE(Anio, 12, 25), "Navidad",
                "No Festivo"
            )
)

```


#### Ventajas de este enfoque

- No depende de Power Query

- Se adapta automáticamente al paso del tiempo

- Ideal para modelos pequeños o medianos

- Muy fácil de portar entre proyectos

- Perfecto para aprendizaje y prototipado rápido


En modelos grandes o corporativos, este calendario suele moverse a Power Query o Dataflows por motivos de gobernanza y rendimiento.
Aun así, como tabla DAX autocontenida, este enfoque es limpio, potente y muy didáctico.


