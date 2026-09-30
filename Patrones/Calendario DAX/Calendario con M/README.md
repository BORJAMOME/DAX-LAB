## Calendario con Power Query (M)

Este script en **Power Query (M)** crea un calendario completo para usar en Power BI, con información de fechas, días, semanas, trimestres y festivos de España. Lo uso como tabla de fechas por defecto cuando el modelo carga los datos con Power Query.


### Cómo usarlo

1. Copia el código M en **Power Query** (Obtener Datos → Consulta Nula → Editor Avanzado).  
2. Ajusta los años mínimo y máximo en los parámetros:
   ```m
   AnioMin = 2022
   AnioMax = 2024
   ```
3. Carga la tabla y conéctala en tu modelo de datos.

4. Relaciona la columna Fecha con tus tablas de hechos para habilitar Time Intelligence y análisis por periodos.

### Columnas generadas

| Columna            | Descripción                                                     |
| ------------------ | --------------------------------------------------------------- |
| Fecha              | Fecha completa                                                  |
| Año                | Año de la fecha                                                 |
| Mes                | Número de mes (1-12)                                            |
| Nombre del mes     | Nombre del mes en español                                       |
| MesCorto           | Nombre corto del mes (Ene, Feb…)                                |
| Día                | Día del mes                                                     |
| Nombre del día     | Nombre del día completo (Lunes, Martes…)                        |
| DiaNombreCorto     | Inicial del día (L, M, X, J, V, S, D)                           |
| IdFecha            | Formato YYYYMMDD                                                |
| Día de la semana   | Número de día de la semana (1=domingo, 7=sábado)                |
| Trimestre          | Trimestre del año (1-4)                                         |
| Trimestre TX       | Trimestre en formato "T1", "T2", …                              |
| Semana del año     | Número de semana según calendario                               |
| Semana del año ISO | Número de semana ISO (lunes como inicio)                        |
| Es Laborable       | Indica si la fecha es laborable o no                            |
| Hoy                | Fecha actual                                                    |
| AñoActual          | Año actual                                                      |
| MesActual          | Mes actual                                                      |
| DíaActual          | Día actual                                                      |
| DesvíoAño          | Diferencia de años respecto a hoy                               |
| DesvíoMes          | Diferencia de meses respecto a hoy                              |
| IDFechaEntero      | Número entero de la fecha                                       |
| DesvíoDía          | Diferencia de días respecto a hoy                               |
| Festivo            | Festivos principales de España (incluye Jueves y Viernes Santo) |




### Qué tiene de especial

- Festivos nacionales de España, con Jueves y Viernes Santo calculados a partir de la fecha de Pascua (algoritmo de Butcher). No incluye festivos autonómicos ni locales: para eso hay que añadir una tabla aparte por comunidad.

- Días laborables: Detecta fines de semana y laborables.

- Desvíos respecto a hoy: Diferencias en años, meses y días para filtros dinámicos y comparativos.

- Formato amigable: Meses y días en español, nombres cortos, identificadores numéricos y de texto.


### Consejos de uso

- Columnas de desvío (DesvíoMes, DesvíoDía) son útiles para filtros relativos, rolling periods o dashboards comparativos.

- Para dashboards internacionales, cambia "es-ES" por otro código de cultura en las funciones de fecha.


--- 

```powerquery

let
    // Parámetros
    AnioMin = 2022,
    AnioMax = 2024,

    // Tabla base de fechas
    Source = List.Dates(
        #date(AnioMin, 1, 1),
        Duration.Days(#date(AnioMax, 12, 31) - #date(AnioMin, 1, 1)) + 1,
        #duration(1,0,0,0)
    ),
    Tbl = Table.FromList(Source, Splitter.SplitByNothing(), {"Fecha"}),

    // Cálculos básicos
    AddYear = Table.AddColumn(Tbl, "Año", each Date.Year([Fecha]), Int64.Type),
    AddMonth = Table.AddColumn(AddYear, "Mes", each Date.Month([Fecha]), Int64.Type),
    AddMonthName = Table.AddColumn(AddMonth, "Nombre del mes", each Date.ToText([Fecha], "MMMM", "es-ES")),
    AddMonthShort = Table.AddColumn(AddMonthName, "MesCorto", each Date.ToText([Fecha], "MMM", "es-ES")),
    AddDay = Table.AddColumn(AddMonthShort, "Día", each Date.Day([Fecha]), Int64.Type),
    AddDayName = Table.AddColumn(AddDay, "Nombre del día", each Date.ToText([Fecha], "dddd", "es-ES")),

    // NUEVA columna: DíaNombreCorto L M X J V S D
    AddDayNameShort = Table.AddColumn(AddDayName, "DiaNombreCorto", each
        let
            // Día de la semana con lunes=0
            dw = Date.DayOfWeek([Fecha], Day.Monday),
            lista = {"L","M","X","J","V","S","D"}
        in
            lista{dw}
    , type text),

    AddIdFecha = Table.AddColumn(AddDayNameShort, "IdFecha", each Date.Year([Fecha])*10000 + Date.Month([Fecha])*100 + Date.Day([Fecha]), Int64.Type),
    AddWeekday = Table.AddColumn(AddIdFecha, "Día de la semana", each Date.DayOfWeek([Fecha])+1, Int64.Type),
    AddQuarter = Table.AddColumn(AddWeekday, "Trimestre", each Date.QuarterOfYear([Fecha]), Int64.Type),
    AddQuarterTX = Table.AddColumn(AddQuarter, "Trimestre TX", each "T" & Number.ToText(Date.QuarterOfYear([Fecha]))),
    AddWeekNum = Table.AddColumn(AddQuarterTX, "Semana del año", each Date.WeekOfYear([Fecha]), Int64.Type),
    AddWeekNumISO = Table.AddColumn(AddWeekNum, "Semana del año ISO", each Date.WeekOfYear([Fecha],Day.Monday), Int64.Type),
    AddLaborable = Table.AddColumn(AddWeekNumISO, "Es Laborable", each if Date.DayOfWeek([Fecha], Day.Monday) < 5 then "Laborable" else "No laborable"),

    // Fechas actuales
    Hoy = DateTime.Date(DateTime.LocalNow()),
    AddHoy = Table.AddColumn(AddLaborable, "Hoy", each Hoy, type date),
    AddAnioActual = Table.AddColumn(AddHoy, "AñoActual", each Date.Year(Hoy), Int64.Type),
    AddMesActual = Table.AddColumn(AddAnioActual, "MesActual", each Date.Month(Hoy), Int64.Type),
    AddDiaActual = Table.AddColumn(AddMesActual, "DíaActual", each Date.Day(Hoy), Int64.Type),

    // Desvíos
    AddDesvAnio = Table.AddColumn(AddDiaActual, "DesvíoAño", each Date.Year([Fecha]) - Date.Year(Hoy), Int64.Type),
    AddDesvMes = Table.AddColumn(AddDesvAnio, "DesvíoMes", each (Date.Year([Fecha]) - Date.Year(Hoy))*12 + Date.Month([Fecha]) - Date.Month(Hoy), Int64.Type),
    AddIDFechaEntero = Table.AddColumn(AddDesvMes, "IDFechaEntero", each Number.From([Fecha]), Int64.Type),
    AddDesvDia = Table.AddColumn(AddIDFechaEntero, "DesvíoDía", each Number.From([Fecha]) - Number.From(Hoy), Int64.Type),

    // Función para calcular Viernes Santo
    fnViernesSanto = (anio as number) as date =>
        let
            A = Number.Mod(anio,19),
            B = Number.IntegerDivide(anio,100),
            C = Number.Mod(anio,100),
            D = Number.IntegerDivide(B,4),
            E = Number.Mod(B,4),
            F = Number.IntegerDivide(B+8,25),
            G = Number.IntegerDivide(B-F+1,3),
            H = Number.Mod(19*A+B-D-G+15,30),
            I = Number.IntegerDivide(C,4),
            K = Number.Mod(C,4),
            L = Number.Mod(32+2*E+2*I-H-K,7),
            M = Number.IntegerDivide(A+11*H+22*L,451),
            MesPascua = Number.IntegerDivide(H+L-7*M+114,31),
            DiaPascua = Number.Mod(H+L-7*M+114,31)+1,
            Pascua = #date(anio, MesPascua, DiaPascua),
            ViernesSanto = Date.AddDays(Pascua,-2)
        in
            ViernesSanto,

    // Columna Festivo
    AddFestivo = Table.AddColumn(AddDesvDia, "Festivo", each 
        let 
            anio = Date.Year([Fecha]),
            viernes = fnViernesSanto(anio)
        in
            if [Fecha] = #date(anio,1,1) then "Año Nuevo" else
            if [Fecha] = #date(anio,1,6) then "Día de Reyes" else
            if [Fecha] = Date.AddDays(viernes,-1) then "Jueves Santo" else
            if [Fecha] = viernes then "Viernes Santo" else
            if [Fecha] = #date(anio,5,1) then "Día del Trabajador" else
            if [Fecha] = #date(anio,8,15) then "Asunción de la Virgen" else
            if [Fecha] = #date(anio,10,12) then "Fiesta Nacional de España" else
            if [Fecha] = #date(anio,11,1) then "Día de Todos los Santos" else
            if [Fecha] = #date(anio,12,6) then "Día de la Constitución Española" else
            if [Fecha] = #date(anio,12,8) then "Inmaculada Concepción" else
            if [Fecha] = #date(anio,12,25) then "Navidad" else
            "No Festivo"
        , type text),

    // Asignar tipos
    CambiarTipos = Table.TransformColumnTypes(
        AddFestivo,
        {
            {"Fecha", type date},
            {"Año", Int64.Type},
            {"Mes", Int64.Type},
            {"Nombre del mes", type text},
            {"MesCorto", type text},
            {"Día", Int64.Type},
            {"Nombre del día", type text},
            {"DiaNombreCorto", type text},
            {"IdFecha", Int64.Type},
            {"Día de la semana", Int64.Type},
            {"Trimestre", Int64.Type},
            {"Trimestre TX", type text},
            {"Semana del año", Int64.Type},
            {"Semana del año ISO", Int64.Type},
            {"Es Laborable", type text},
            {"Hoy", type date},
            {"AñoActual", Int64.Type},
            {"MesActual", Int64.Type},
            {"DíaActual", Int64.Type},
            {"DesvíoAño", Int64.Type},
            {"DesvíoMes", Int64.Type},
            {"IDFechaEntero", Int64.Type},
            {"DesvíoDía", Int64.Type},
            {"Festivo", type text}
        }
    ),
  #"Valor reemplazado" = Table.ReplaceValue(CambiarTipos, ".", "", Replacer.ReplaceText, {"MesCorto"}),
  #"Mayúsculas aplicadas en cada palabra" = Table.TransformColumns(#"Valor reemplazado", {{"MesCorto", each Text.Proper(_), type nullable text}})
in
    #"Mayúsculas aplicadas en cada palabra"

```
