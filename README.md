# DAX Lab

Mi cuaderno de DAX y modelado en Power BI. Cada página es un problema concreto que me he encontrado, la medida con la que lo resolví y por qué funciona, con la captura del resultado y, en los proyectos, el `.pbix` para abrirlo.

Más de 50 páginas en cuatro bloques: fundamentos de contexto de filtro, funciones avanzadas (WINDOW, OFFSET, TREATAS…), patrones que reutilizo entre informes y casos completos. Los datos son de Contoso y Adventure Works, los modelos de ejemplo de Microsoft..

## Empieza aquí

Si solo vas a mirar cuatro páginas:

- **[Orden de los filtros en CALCULATE anidado](<./Básico/Orden en los filtros/>)**: dos medidas con los mismos ingredientes dan 54.965 y 610.660. Por qué el CALCULATE interno tiene la última palabra.
- **[Contexto de filtro II](<./Básico/Contexto de filtro/Contexto de filtro II/>)**: cinco formas de escribir "solo rojo" y qué hace cada una con el filtro que ya trae la fila.
- **[WINDOW](<./Avanzado/WINDOW/>)**: acumulados, media móvil, Pareto y Top N con posiciones ABS y REL, incluido un error que cometí y cómo se ve en la captura.
- **[Ventas vs Presupuesto](<./Proyectos/Ventas vs Presupuesto/>)**: comparar tablas con granularidad distinta (transaccional frente a mensual por ciudad) con TREATAS, sin relaciones físicas. Incluye el `.pbix`.

## Índice completo

### Básico: fundamentos de contexto y filtro

- [VALUES](<./Básico/VALUES/>) · [VALUES vs ALL](<./Básico/VALUES vs ALL/>) · [ALL vs REMOVEFILTERS](<./Básico/ALL vs REMOVEFILTERS/>) · [ALLSELECTED](<./Básico/ALLSELECTED/>) · [ALLEXCEPT](<./Básico/ALLEXCEPT/>)
- [Contexto de filtro I](<./Básico/Contexto de filtro/Contexto de filtro I/>) · [Contexto de filtro II](<./Básico/Contexto de filtro/Contexto de filtro II/>)
- [Conjuntos admitidos y excluidos](<./Básico/Conjuntos admitidos y excluidos/>) · [Filtrar ignorando el contexto externo](<./Básico/Filtrar ignorando el contexto externo/>) · [Filtrar por rango usando un slicer](<./Básico/Filtrar por rango usando un slicer/>) · [Orden en los filtros](<./Básico/Orden en los filtros/>)
- [CROSSFILTER](<./Básico/CROSSFILTER/>) · [USERELATIONSHIP](<./Básico/USERELATIONSHIP/>) · [RELATED](<./Básico/RELATED/>)
- [Rankings con RANKX](<./Básico/RANK/>) · [Encabezados dinámicos con ISFILTERED](<./Básico/ISFILTERED - Encabezados dinámicos/>)
- [Corregir valores cuando falta 0](<./Básico/Corregir valores cuando falta 0/>) · [Importancia de los Totales](<./Básico/Importancia de los Totales/>) · [Indicadores de Rendimiento con color](<./Básico/Indicadores de Rendimiento con color/>)
- **Time Intelligence:** [Comparar datos hasta la fecha](<./Básico/TIME INTELLIGENCE/Casos/Comparar datos hasta la fecha/>) · [Comparar solo hasta una fecha](<./Básico/TIME INTELLIGENCE/Casos/Comparar solo hasta una fecha/>) · [Rolling year (últimos 12 meses)](<./Básico/TIME INTELLIGENCE/Casos/ROLLINYEAR/>) · [SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD](<./Básico/TIME INTELLIGENCE/Casos/SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD/>)

### Avanzado: modelado y contexto de filtro complejo

- [HASONEVALUE](<./Avanzado/HASONEVALUE/>) · [ISFILTERED](<./Avanzado/ISFILTERED/>) · [ISINSCOPE](<./Avanzado/ISINSCOPE/>) · [LOOKUPVALUE](<./Avanzado/LOOKUPVALUE/>)
- [TREATAS](<./Avanzado/TREATAS/>) · [Lógica condicional jerárquica con tablas virtuales](<./Avanzado/Lógica condicional jerárquica con tablas virtuales/>) · [Resolución de problemas de granularidad](<./Avanzado/Resolución de problemas de granularidad/>)
- [WINDOW](<./Avanzado/WINDOW/>) · [OFFSET](<./Avanzado/OFFSET/>) · [TopN](<./Avanzado/TopN/>)
- [Navegación según grupo de usuario](<./Avanzado/Control de acceso a páginas/>) · [Evitar que un slicer muestre opciones sin datos](<./Avanzado/Evitar que un slicer muestre opciones sin datos/>) · [Medida para controlar valores en Slicers](<./Avanzado/Medida para controlar valores en Slicers/>)

### Patrones: reutilizables entre proyectos

- **Calendario DAX:** [Distintas formas de construir una tabla de fechas](<./Patrones/Calendario DAX/Distintas formas de construir una tabla de fechas/>) · [Tabla Calendario con DAX](<./Patrones/Calendario DAX/Tabla Calendario con DAX/>) · [Calendario con M](<./Patrones/Calendario DAX/Calendario con M/>) · [Creación Calendario](<./Patrones/Calendario DAX/Creación Calendario/>) *(incluye `.pbix`)*
- [Medidas TIME INTELLIGENCE](<./Patrones/Medidas TIME INTELLIGENCE/>) · [Formateo dinámico en etiquetas](<./Patrones/Formateo dinámico en etiquetas/>) · [Filtrado con nombre de selección en tarjeta](<./Patrones/Filtrado con nombre de selección en tarjeta/>)
- [Colores condicionales](<./Patrones/Colores condicionales/>) · [Librería de colores](<./Patrones/Libreria de colores/>): mis colores estándar como medidas, listos para pegar en la vista de consultas DAX
- [Grid](<./Patrones/Grid/>): plantillas de rejilla para maquetar informes a tamaño real (16:9, carta, móvil)

### Proyectos: casos completos con `.pbix`

- [Ventas vs Presupuesto](<./Proyectos/Ventas vs Presupuesto/>): TREATAS para comparar granularidades distintas sin relación física
- [Widget Control de presupuestos](<./Proyectos/Widget Control de presupuestos/>): Top N elegido por el usuario con diagnóstico automático de concentración
- [Medidas dinámicas según jerarquía en una matriz](<./Proyectos/Medidas dinámicas según jerarquía en una matriz/>)
- [Análisis comparativo de ventas mensuales vs promedio](<./Proyectos/Análisis comparativo de ventas mensuales vs promedio/>)
- [Columnas Max y Min Resaltado](<./Proyectos/Columnas Max y Min Resaltado/>) · [Gráfico de columnas agrupadas](<./Proyectos/Gráfico de columnas agrupadas/>) · [Formato condicional en Parámetros de campo](<./Proyectos/Formato condicional en Parámetros de campo/>)

### Conceptos clave

- [Modelo en Estrella](<./docs/Conceptos clave/Modelo en Estrella/>): por qué separo hechos y dimensiones antes de escribir una sola medida

## Cómo está hecho

Cada carpeta tiene su `README.md` con el problema, las medidas y la captura del resultado; los proyectos incluyen el `.pbix`. Añado una página cada vez que resuelvo algo que sé que voy a volver a necesitar, y reviso las antiguas cuando aprendo que algo estaba mal explicado. Cuando una captura corresponde a una versión anterior de la medida, lo indico en la propia página.

## Contacto

borja.mora.mendez@gmail.com
[Portfolio](https://borjamora.es)
[LinkedIn](https://www.linkedin.com/in/borjamoramendez/)
