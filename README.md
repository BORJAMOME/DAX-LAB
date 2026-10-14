# DAX Lab

He creado este repositorio para reunir ejemplos y soluciones de DAX que voy trabajando con Power BI. Incluye problemas de contexto de filtro, relaciones entre tablas, cálculos de fechas, medidas dinámicas y otros casos que me interesa tener documentados para poder consultarlos y reutilizarlos en futuros proyectos.

Actualmente reúne más de 50 páginas organizadas en cuatro categorías, con ejemplos explicados, capturas de Power BI y archivos `.pbix` en varios casos para poder abrirlos y probar las medidas directamente.

## Por dónde empezar

Si quieres ver algunos ejemplos representativos, estos son tres buenos puntos de partida:

- **[TREATAS](<./Avanzado/TREATAS/>)** — un ejemplo de cómo aplicar filtros entre tablas que no tienen una relación directa en el modelo.
- **[Ventas vs Presupuesto](<./Proyectos/Ventas vs Presupuesto/>)** — comparación de ventas detalladas con un presupuesto agrupado por meses, incluyendo el archivo `.pbix`.
- **[Distintas formas de construir una tabla de fechas](<./Patrones/Calendario DAX/Distintas formas de construir una tabla de fechas/>)** — distintas opciones para crear un calendario con Power Query, DAX o M.

## Índice de ejemplos

### Básico — contexto de filtro y funciones DAX

- [VALUES](<./Básico/VALUES/>) · [VALUES vs ALL](<./Básico/VALUES vs ALL/>) · [ALL vs REMOVEFILTERS](<./Básico/ALL vs REMOVEFILTERS/>) · [ALLSELECTED](<./Básico/ALLSELECTED/>) · [ALLEXCEPT](<./Básico/ALLEXCEPT/>)
- [Contexto de filtro I](<./Básico/Contexto de filtro/Contexto de filtro I/>) · [Contexto de filtro II](<./Básico/Contexto de filtro/Contexto de filtro II/>)
- [Conjuntos admitidos y excluidos](<./Básico/Conjuntos admitidos y excluidos/>) · [Filtrar ignorando el contexto externo](<./Básico/Filtrar ignorando el contexto externo/>) · [Filtrar por rango usando un slicer](<./Básico/Filtrar por rango usando un slicer/>) · [Orden en los filtros](<./Básico/Orden en los filtros/>)
- [CROSSFILTER](<./Básico/CROSSFILTER/>) · [USERELATIONSHIP](<./Básico/USERELATIONSHIP/>) · [RELATED](<./Básico/RELATED/>)
- [RANK](<./Básico/RANK/>) · [ISFILTERED - Encabezados dinámicos](<./Básico/ISFILTERED - Encabezados dinámicos/>)
- [Corregir valores cuando falta 0](<./Básico/Corregir valores cuando falta 0/>) · [Importancia de los Totales](<./Básico/Importancia de los Totales/>) · [Indicadores de Rendimiento con color](<./Básico/Indicadores de Rendimiento con color/>)

**Time Intelligence**

- [Comparar datos hasta la fecha](<./Básico/TIME INTELLIGENCE/Casos/Comparar datos hasta la fecha/>)
- [Comparar solo hasta una fecha](<./Básico/TIME INTELLIGENCE/Casos/Comparar solo hasta una fecha/>)
- [ROLLINYEAR](<./Básico/TIME INTELLIGENCE/Casos/ROLLINYEAR/>)
- [SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD](<./Básico/TIME INTELLIGENCE/Casos/SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD/>)

### Avanzado — modelado y filtros más complejos

- [HASONEVALUE](<./Avanzado/HASONEVALUE/>) · [ISFILTERED](<./Avanzado/ISFILTERED/>) · [ISINSCOPE](<./Avanzado/ISINSCOPE/>) · [LOOKUPVALUE](<./Avanzado/LOOKUPVALUE/>)
- [TREATAS](<./Avanzado/TREATAS/>) · [Lógica condicional jerárquica con tablas virtuales](<./Avanzado/Lógica condicional jerárquica con tablas virtuales/>) · [Resolución de problemas de granularidad](<./Avanzado/Resolución de problemas de granularidad/>)
- [WINDOW](<./Avanzado/WINDOW/>) · [OFFSET](<./Avanzado/OFFSET/>) · [TopN](<./Avanzado/TopN/>)
- [Control de acceso a páginas](<./Avanzado/Control de acceso a páginas/>) · [Evitar que un slicer muestre opciones sin datos](<./Avanzado/Evitar que un slicer muestre opciones sin datos/>) · [Medida para controlar valores en Slicers](<./Avanzado/Medida para controlar valores en Slicers/>)

### Patrones reutilizables

**Calendarios**

- [Distintas formas de construir una tabla de fechas](<./Patrones/Calendario DAX/Distintas formas de construir una tabla de fechas/>)
- [Tabla Calendario con DAX](<./Patrones/Calendario DAX/Tabla Calendario con DAX/>)
- [Calendario con M](<./Patrones/Calendario DAX/Calendario con M/>)
- [Creación Calendario](<./Patrones/Calendario DAX/Creación Calendario/>) — incluye `.pbix`.

**Medidas y visualización**

- [Medidas TIME INTELLIGENCE](<./Patrones/Medidas TIME INTELLIGENCE/>)
- [Formateo dinámico en etiquetas](<./Patrones/Formateo dinámico en etiquetas/>)
- [Filtrado con nombre de selección en tarjeta](<./Patrones/Filtrado con nombre de selección en tarjeta/>)
- [Colores condicionales](<./Patrones/Colores condicionales/>)
- [Librería de colores](<./Patrones/Libreria de colores/>) — códigos HEX para estados positivos, negativos y neutros.
- [Grid](<./Patrones/Grid/>) — plantillas de rejilla para organizar informes en formatos 16:9, carta y móvil.

### Proyectos completos

En esta sección reúno ejemplos que van más allá de una medida aislada e incluyen archivos `.pbix` para explorar su funcionamiento en Power BI Desktop.

- [Ventas vs Presupuesto](<./Proyectos/Ventas vs Presupuesto/>) — comparación de datos con distinta granularidad mediante TREATAS.
- [Widget Control de presupuestos](<./Proyectos/Widget Control de presupuestos/>)
- [Medidas dinámicas según jerarquía en una matriz](<./Proyectos/Medidas dinámicas según jerarquía en una matriz/>)
- [Análisis comparativo de ventas mensuales vs promedio](<./Proyectos/Análisis comparativo de ventas mensuales vs promedio/>)
- [Columnas Max y Min Resaltado](<./Proyectos/Columnas Max y Min Resaltado/>)
- [Gráfico de columnas agrupadas](<./Proyectos/Gráfico de columnas agrupadas/>)
- [Formato condicional en Parámetros de campo](<./Proyectos/Formato condicional en Parámetros de campo/>)

### Conceptos de modelado

- [Modelo en Estrella](<./docs/Conceptos clave/Modelo en Estrella/>) — cómo organizo las tablas de hechos y dimensiones para trabajar con un modelo de Power BI.

## Cómo está organizado

Cada carpeta tiene su propio `README.md`, donde explico el caso, la medida DAX y el resultado. Cuando corresponde, también incluyo capturas o un archivo `.pbix` para probar el ejemplo directamente en Power BI Desktop.

Sigo ampliando el repositorio a medida que trabajo con nuevos casos. La idea es tener un lugar al que volver cuando necesito revisar una función, recuperar una solución o comprobar cómo resolví un problema parecido anteriormente.

## Contacto

- **Correo:** borja.mora.mendez@gmail.com
- **Portfolio:** [borjamora.es](https://borjamora.es)
- **LinkedIn:** [Borja Mora Méndez](https://www.linkedin.com/in/borjamoramendez/)
