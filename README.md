# DAX Lab

Cuaderno de práctica donde documento cada patrón DAX y cada problema de modelado que resuelvo en Power BI — no una colección genérica de snippets, sino los casos concretos con los que me he tropezado y cómo los resolví, con la medida final y el porqué.

Más de 50 páginas repartidas en cuatro carpetas, la mayoría con captura del resultado en Power BI y varias con el `.pbix` real para abrir y tocar.

## Empieza aquí

Si solo vas a mirar tres cosas:

- **[TREATAS](<./Avanzado/TREATAS/>)** — cómo crear relaciones virtuales entre tablas que no están conectadas en el modelo, incluyendo selectores de periodo con tablas desconectadas.
- **[Ventas vs Presupuesto](<./Proyectos/Ventas vs Presupuesto/>)** — comparar dos tablas con granularidad distinta (transaccional vs. mensual agregada) sin forzar relaciones físicas. Incluye el `.pbix`.
- **[Distintas formas de construir una tabla de fechas](<./Patrones/Calendario DAX/Distintas formas de construir una tabla de fechas/>)** — Power Query, DAX puro o M: cuándo conviene cada una.

## Índice completo

### Básico — fundamentos de contexto y filtro

- [VALUES](<./Básico/VALUES/>) · [VALUES vs ALL](<./Básico/VALUES vs ALL/>) · [ALL vs REMOVEFILTERS](<./Básico/ALL vs REMOVEFILTERS/>) · [ALLSELECTED](<./Básico/ALLSELECTED/>) · [ALLEXCEPT](<./Básico/ALLEXCEPT/>)
- [Contexto de filtro I](<./Básico/Contexto de filtro/Contexto de filtro I/>) · [Contexto de filtro II](<./Básico/Contexto de filtro/Contexto de filtro II/>)
- [Conjuntos admitidos y excluidos](<./Básico/Conjuntos admitidos y excluidos/>) · [Filtrar ignorando el contexto externo](<./Básico/Filtrar ignorando el contexto externo/>) · [Filtrar por rango usando un slicer](<./Básico/Filtrar por rango usando un slicer/>) · [Orden en los filtros](<./Básico/Orden en los filtros/>)
- [CROSSFILTER](<./Básico/CROSSFILTER/>) · [USERELATIONSHIP](<./Básico/USERELATIONSHIP/>) · [RELATED](<./Básico/RELATED/>)
- [RANK](<./Básico/RANK/>) · [ISFILTERED - Encabezados dinámicos](<./Básico/ISFILTERED - Encabezados dinámicos/>)
- [Corregir valores cuando falta 0](<./Básico/Corregir valores cuando falta 0/>) · [Importancia de los Totales](<./Básico/Importancia de los Totales/>) · [Indicadores de Rendimiento con color](<./Básico/Indicadores de Rendimiento con color/>)
- **Time Intelligence:** [Comparar datos hasta la fecha](<./Básico/TIME INTELLIGENCE/Casos/Comparar datos hasta la fecha/>) · [Comparar solo hasta una fecha](<./Básico/TIME INTELLIGENCE/Casos/Comparar solo hasta una fecha/>) · [ROLLINYEAR](<./Básico/TIME INTELLIGENCE/Casos/ROLLINYEAR/>) · [SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD](<./Básico/TIME INTELLIGENCE/Casos/SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD/>)

### Avanzado — modelado y contexto de filtro complejo

- [HASONEVALUE](<./Avanzado/HASONEVALUE/>) · [ISFILTERED](<./Avanzado/ISFILTERED/>) · [ISINSCOPE](<./Avanzado/ISINSCOPE/>) · [LOOKUPVALUE](<./Avanzado/LOOKUPVALUE/>)
- [TREATAS](<./Avanzado/TREATAS/>) · [Lógica condicional jerárquica con tablas virtuales](<./Avanzado/Lógica condicional jerárquica con tablas virtuales/>) · [Resolución de problemas de granularidad](<./Avanzado/Resolución de problemas de granularidad/>)
- [WINDOW](<./Avanzado/WINDOW/>) · [OFFSET](<./Avanzado/OFFSET/>) · [TopN](<./Avanzado/TopN/>)
- [Control de acceso a páginas](<./Avanzado/Control de acceso a páginas/>) · [Evitar que un slicer muestre opciones sin datos](<./Avanzado/Evitar que un slicer muestre opciones sin datos/>) · [Medida para controlar valores en Slicers](<./Avanzado/Medida para controlar valores en Slicers/>)

### Patrones — reutilizables entre proyectos

- **Calendario DAX:** [Distintas formas de construir una tabla de fechas](<./Patrones/Calendario DAX/Distintas formas de construir una tabla de fechas/>) · [Tabla Calendario con DAX](<./Patrones/Calendario DAX/Tabla Calendario con DAX/>) · [Calendario con M](<./Patrones/Calendario DAX/Calendario con M/>) · [Creación Calendario](<./Patrones/Calendario DAX/Creación Calendario/>) *(incluye `.pbix`)*
- [Medidas TIME INTELLIGENCE](<./Patrones/Medidas TIME INTELLIGENCE/>) · [Formateo dinámico en etiquetas](<./Patrones/Formateo dinámico en etiquetas/>) · [Filtrado con nombre de selección en tarjeta](<./Patrones/Filtrado con nombre de selección en tarjeta/>)
- [Colores condicionales](<./Patrones/Colores condicionales/>) · [Librería de colores](<./Patrones/Libreria de colores/>) — mis HEX estándar para positivo/negativo/neutro, listos para copiar
- [Grid](<./Patrones/Grid/>) — plantillas de rejilla para maquetar informes a tamaño real (16:9, carta, móvil)

### Proyectos — casos completos con `.pbix`

- [Ventas vs Presupuesto](<./Proyectos/Ventas vs Presupuesto/>) — TREATAS para comparar granularidades distintas sin relación física
- [Widget Control de presupuestos](<./Proyectos/Widget Control de presupuestos/>)
- [Medidas dinámicas según jerarquía en una matriz](<./Proyectos/Medidas dinámicas según jerarquía en una matriz/>)
- [Análisis comparativo de ventas mensuales vs promedio](<./Proyectos/Análisis comparativo de ventas mensuales vs promedio/>)
- [Columnas Max y Min Resaltado](<./Proyectos/Columnas Max y Min Resaltado/>) · [Gráfico de columnas agrupadas](<./Proyectos/Gráfico de columnas agrupadas/>) · [Formato condicional en Parámetros de campo](<./Proyectos/Formato condicional en Parámetros de campo/>)

### Conceptos clave

- [Modelo en Estrella](<./docs/Conceptos clave/Modelo en Estrella/>) — por qué separo hechos y dimensiones antes de escribir una sola medida

## Cómo lo uso

Cada carpeta trae su propio `README.md` con el problema, la medida DAX y, cuando aplica, la captura o el `.pbix` para abrirlo directamente en Power BI Desktop. Está en construcción activa: añado una página cada vez que resuelvo algo que sé que voy a necesitar volver a consultar.

## Contacto

borja.mora.mendez@gmail.com
[Portfolio](https://borjamora.es)
[LinkedIn](https://www.linkedin.com/in/borjamoramendez/)
