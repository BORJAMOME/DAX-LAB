## Creación de un calendario paso a paso 

1. Crearlos parámetros de fecha

Power Query → Inicio > Nueva fuente > Consulta en blanco y  pegar el siguiente código para FechaInicio:

```
let     
// Paso 1: Obtener el año actual y restándole 20     
Año = Date.Year(DateTime.LocalNow()) -20,       
// Paso 2: Crear una cadena de texto que representa la fecha "01/01/Año"     
Fecha = Text.From("01/01/") & Text.From(Año),       
// Paso 3: Convertir la cadena de texto en una fecha real     
FechaInicio = Date.FromText(Fecha)

in     

FechaInicio
```

Realizamos lo mismo para FechaFin:

```
let     
// Paso 1: Obtener el año actual y sumarle 2     
Año = Date.Year(DateTime.LocalNow()) +2,       
// Paso 2: Crear una cadena de texto que representa la fecha "31/12/Año"     
Fecha = Text.From("31/12/") & Text.From(Año),       
// Paso 3: Convertir la cadena de texto en una fecha real     
FechaFin = Date.FromText(Fecha) 

in     

FechaFin
```

2. Crear el esqueleto del calendario

- Tener una tabla con una sola columna Fecha

- Que vaya de FechaInicio a FechaFin

- Totalmente dinámica

Power Query → Inicio > Nueva fuente > Consulta en blanco


```
let     
// 1. Calcular la cantidad de días entre FechaInicio y FechaFin, sumando 1 para incluir ambos días     
DuracionDias = Duration.Days(FechaFin - FechaInicio) + 1,       
// 2. Crear una lista de fechas desde FechaInicio hasta FechaFin con un incremento de 1 día     
ListaFechas = List.Dates(FechaInicio, DuracionDias, #duration(1, 0, 0, 0)),         
// 3. Convertir la lista de fechas en una tabla     
TablaFechas = Table.FromList(ListaFechas, Splitter.SplitByNothing(), null, null, ExtraValues.Error),         
// 4. Cambiar el tipo de dato de la columna a tipo fecha     
TablaFechasConTipoFecha = Table.TransformColumnTypes(TablaFechas, {{"Column1", type date}}),
  #"Columnas con nombre cambiado" = Table.RenameColumns(TablaFechasConTipoFecha, {{"Column1", "Fecha"}}),
  #"Año insertado" = Table.AddColumn(#"Columnas con nombre cambiado", "Año", each Date.Year([Fecha]), Int64.Type),
  #"Mes insertado" = Table.AddColumn(#"Año insertado", "Mes", each Date.Month([Fecha]), type nullable number),
  #"Nombre del mes insertado" = Table.AddColumn(#"Mes insertado", "Nombre del mes", each Date.MonthName([Fecha]), type nullable text),
  #"Día insertado" = Table.AddColumn(#"Nombre del mes insertado", "Día", each Date.Day([Fecha]), Int64.Type),
  #"Personalizado agregado" = Table.TransformColumnTypes(Table.AddColumn(#"Día insertado", "IdFecha", each [Año]*10000 + [Mes]*100 + [Día]), {{"IdFecha", Int64.Type}}),
  #"Personalizado agregado 1" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado", "IdFechaEntero", each [Fecha]), {{"IdFechaEntero", Int64.Type}}),
  #"Personalizado agregado 2" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 1", "Hoy", each Number.RoundDown (Number.From(DateTime.LocalNow()))), {{"Hoy", Int64.Type}}),
  #"Personalizado agregado 3" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 2", "DesvioDia", each [IdFechaEntero] - [Hoy]), {{"DesvioDia", Int64.Type}}),
  #"Día de la semana insertado" = Table.AddColumn(#"Personalizado agregado 3", "Día de la semana", each Date.DayOfWeek([Fecha]) + 1 , Int64.Type),
  #"Nombre del día insertado" = Table.AddColumn(#"Día de la semana insertado", "Nombre del día", each Date.DayOfWeekName([Fecha]), type nullable text),
  #"Columnas reordenadas" = Table.ReorderColumns(#"Nombre del día insertado", {"Fecha", "Año", "Mes", "Nombre del mes", "Nombre del día", "Día", "IdFecha", "IdFechaEntero", "Hoy", "DesvioDia", "Día de la semana"}),
  #"Columna condicional insertada" = Table.AddColumn(#"Columnas reordenadas", "FinDeSemana", each if [Día de la semana] >= 6 then "Fin de semana" else "Laborable", type text),
  #"Trimestre insertado" = Table.AddColumn(#"Columna condicional insertada", "Trimestre", each Date.QuarterOfYear([Fecha]), Int64.Type),
  #"Personalizado agregado 4" = Table.TransformColumnTypes(Table.AddColumn(#"Trimestre insertado", "TrimestreLetra", each "T" & Text.From([Trimestre])), {{"TrimestreLetra", type text}}),
  #"Personalizado agregado 5" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 4", "AñoActual", each Date.Year(DateTime.LocalNow())), {{"AñoActual", Int64.Type}}),
  #"Personalizado agregado 6" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 5", "DesvioAño", each [Año] - [AñoActual]), {{"DesvioAño", Int64.Type}}),
  #"Personalizado agregado 7" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 6", "MesActual", each Date.Month(DateTime.LocalNow())), {{"MesActual", Int64.Type}}),
  #"Personalizado agregado 8" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 7", "DesvioMes", each [Mes]-[MesActual]+([DesvioAño]*12)), {{"DesvioMes", Int64.Type}}),
  #"Semana del año insertada" = Table.AddColumn(#"Personalizado agregado 8", "Semana del año", each Date.WeekOfYear([Fecha]), type nullable number),
  #"Personalizado agregado 9" = Table.TransformColumnTypes(Table.AddColumn(#"Semana del año insertada", "SemanaISO", each let     
// Calcular la semana para la fecha dada     
         SemanaCalculada = Number.RoundDown((Date.DayOfYear([Fecha]) - 
(Date.DayOfWeek([Fecha], Day.Monday) + 1) + 10) / 7),         

// Calcular la semana para el último día del año anterior     

UltimaSemanaAñoAnterior = 
Number.RoundDown((Date.DayOfYear(#date(Date.Year([Fecha]) - 1, 12, 31)) - 
(Date.DayOfWeek(#date(Date.Year([Fecha]) - 1, 12, 31), Day.Monday) + 1) + 10) / 7),         

// Comprobar si la fecha está en la semana 0 (pertenece a la última semana del año anterior)     
EsSemanaCero = Number.RoundDown((([Día] - [Día de la semana]) + 10) / 7) = 0,         

// Comprobar si es la semana 53 y si el último día del año es un lunes o martes (es decir, tiene menos de 4 días)    
EsSemana53YFinDeAño = (SemanaCalculada = 53) and 
(Date.DayOfWeek(#date(Date.Year([Fecha]), 12, 31), Day.Monday) + 1 < 4) 

in     
if EsSemanaCero then         
UltimaSemanaAñoAnterior     
else if EsSemana53YFinDeAño then         
1     
else         
SemanaCalculada), {{"SemanaISO", Int64.Type}}),
  #"Personalizado agregado 10" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 9", "AñoFiscal", each if [Mes] < InicioMesFiscal then [Año]-1 else [Año]), {{"AñoFiscal", Int64.Type}}),
  #"Filas ordenadas" = Table.Sort(#"Personalizado agregado 10", {{"Fecha", Order.Descending}}),
  #"Personalizado agregado 11" = Table.TransformColumnTypes(Table.AddColumn(#"Filas ordenadas", "MesFiscal", each if [Mes] >= InicioMesFiscal then [Mes] - InicioMesFiscal else [Mes] + (12-InicioMesFiscal+1)), {{"MesFiscal", Int64.Type}}),
  #"Personalizado agregado 12" = Table.TransformColumnTypes(Table.AddColumn(#"Personalizado agregado 11", "TrimestreFiscal", each Number.RoundUp([MesFiscal])/3), {{"TrimestreFiscal", Int64.Type}}),
  #"Consultas combinadas" = Table.NestedJoin(#"Personalizado agregado 12", {"Fecha"}, Festivos, {"Fecha"}, "Consulta (2)", JoinKind.LeftOuter),
  #"Consulta (2) expandido" = Table.ExpandTableColumn(#"Consultas combinadas", "Consulta (2)", {"Festivo"}, {"Festivo"})   

in       

#"Consulta (2) expandido"
```

3. Creación del mes fiscal

Inicio > Administrar parámetros > Nuevo parámetro 




