## Creación de un calendario paso a paso 



### 1. Crearlos parámetros de fecha

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

### 2. Crear el esqueleto del calendario

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

### 3. Creación del mes fiscal

Inicio > Administrar parámetros > Nuevo parámetro 

![ejemplo](/docs/imagenes/Para1.png)

![ejemplo](/docs/imagenes/Para2.png)


### 4. Consulta para festivos 


```
let   
// Definir la fecha de inicio y la fecha de fin para el calendario   
FechaInicio = FechaInicio,  // Fecha inicial del rango de calendario   
FechaFin = FechaFin,        // Fecha final del rango de calendario     
// Generar una lista de fechas desde FechaInicio hasta FechaFin   
ListaFechas = List.Dates(FechaInicio, Duration.Days(FechaFin - FechaInicio) + 1, #duration(1, 0, 0, 0)),     
// Crear una lista de fechas que abarca desde la FechaInicio hasta la FechaFin, sumando un día adicional para incluir la última fecha.     

// Convertir la lista en una tabla con una columna llamada "Fecha"   
TablaCalendario = Table.FromList(ListaFechas, Splitter.SplitByNothing(), {"Fecha"}),    

 // Convierte la lista de fechas en una tabla de una sola columna llamada "Fecha".     
 
 // Función para calcular el Viernes Santo (2 días antes de la Pascua)   
 CalcularViernesSanto = (año as number) =>         
 let             
 A = Number.Mod(año, 19),             
 B = Number.IntegerDivide(año, 100),             
 C = Number.Mod(año, 100),             
 D = Number.IntegerDivide(B, 4),             
 E = Number.Mod(B, 4),             
 F = Number.IntegerDivide((B + 8), 25),             
 G = Number.IntegerDivide((B - F + 1), 3),             
 H = Number.Mod((19 * A + B - D - G + 15), 30),             
 I = Number.IntegerDivide(C, 4),             
 K = Number.Mod(C, 4),             
 L = Number.Mod((32 + 2 * E + 2 * I - H - K), 7),             
 M = Number.IntegerDivide((A + 11 * H + 22 * L), 451),             
 MesPascua = Number.IntegerDivide((H + L - 7 * M + 114), 31),             
 DiaPascua = Number.Mod((H + L - 7 * M + 114), 31) + 1,
 DomingoPascua = #date(año, MesPascua, DiaPascua),            
 ViernesSanto = Date.AddDays(DomingoPascua, -2)         
in             
 ViernesSanto,

// Define una función llamada CalcularViernesSanto, que calcula el Viernes Santo (dos días antes del Domingo de Pascua) para un año dado usando un método algorítmico.     
// Crear una tabla con los festivos nacionales comunes y dinámicos     

FestivosNacionales = (año as number) =>         
  let             
    BaseFestivos = {                 
    [Fecha = #date(año, 1, 1), Festivo = "Año Nuevo"],                 
    [Fecha = #date(año, 1, 6), Festivo = "Día de Reyes"],                 
    [Fecha = CalcularViernesSanto(año), Festivo = "Viernes Santo"],                 
    [Fecha = #date(año, 5, 1), Festivo = "Día del Trabajador"],                 
    [Fecha = #date(año, 8, 15), Festivo = "Asunción de la Virgen"],                 
    [Fecha = #date(año, 10, 12), Festivo = "Fiesta Nacional de España"],                 
    [Fecha = #date(año, 11, 1), Festivo = "Día de Todos los Santos"],
    [Fecha = #date(año, 12, 6), Festivo = "Día de la Constitución Española"],                 
    [Fecha = #date(año, 12, 8), Festivo = "Inmaculada Concepción"],                 
    [Fecha = #date(año, 12, 25), Festivo = "Navidad"]             
    },             
    TablaFestivos = Table.FromRecords(BaseFestivos)         
  in             
    TablaFestivos,

// Define una función llamada FestivosNacionales, que recibe un año como parámetro y devuelve una tabla con los días festivos nacionales de ese año.     
// Los festivos incluyen tanto fechas fijas como Viernes Santo, que es calculado dinámicamente con la función anterior.     
// Generar una lista de años a partir del año de FechaInicio hasta el año de FechaFin     
ListaAños = {Date.Year(FechaInicio)..Date.Year(FechaFin)},     
// Crea una lista de años desde el año de FechaInicio hasta el año de FechaFin.     
TablaFestivosTodosAños = Table.Combine(List.Transform(ListaAños, each FestivosNacionales(_))),     
// Aplica la función FestivosNacionales a cada año de ListaAños para crear una lista de tablas de festivos anuales.     
// Luego, combina estas tablas en una única tabla que contiene todos los festivos de todos los años en el rango.     
// Fusionar la tabla de festivos con la tabla calendario     
TablaConFestivos = Table.NestedJoin(TablaCalendario, "Fecha", 
TablaFestivosTodosAños, "Fecha", "Festivo", JoinKind.LeftOuter),     
// Realiza una combinación (join) izquierda entre la TablaCalendario y la TablaFestivosTodosAños en la columna "Fecha".     
// Esto añade una columna "Festivo" que contiene el nombre del festivo si la fecha coincide con un día festivo; de lo contrario, estará en blanco (null).     
// Expandir la columna de festivos y reemplazar null por "No Festivo"     
TablaConFestivosExpandida = Table.ExpandTableColumn(TablaConFestivos, 
"Festivo", {"Festivo"}, {"Festivo"}),     
// Expande la columna de "Festivo" para mostrar directamente el nombre del festivo en la tabla de calendario.     
TablaFinal = Table.ReplaceValue(TablaConFestivosExpandida, null, "No Festivo", 
Replacer.ReplaceValue, {"Festivo"}),     
// Reemplaza los valores nulos en la columna "Festivo" por "No Festivo", de manera que las fechas no festivas se etiqueten explícitamente como "No Festivo".
#"Transformar columnas" = Table.TransformColumnTypes(TablaFinal, {{"Fecha", type text}, {"Festivo", type text}}),     
// Cambia el tipo de datos de las columnas "Fecha" y "Festivo" a texto.     
#"Reemplazar errores" = Table.ReplaceErrorValues(#"Transformar columnas", {{"Fecha", null}, {"Festivo", null}})     
// Reemplaza posibles errores en las columnas "Fecha" y "Festivo" con valores nulos, asegurando que la tabla no contenga errores.   
  ,
  #"Tipo de columna cambiado" = Table.TransformColumnTypes(#"Reemplazar errores", {{"Fecha", type date}}),
  #"Filas ordenadas" = Table.Sort(#"Tipo de columna cambiado", {{"Fecha", Order.Descending}})
in   
#"Filas ordenadas"
```


![ejemplo](/docs/imagenes/Calendario.png)




