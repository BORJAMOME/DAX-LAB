### Encabezados din├ímicos con ISFILTERED 

Es muy com├║n que un usuario filtre el informe (por pa├¡s, categor├¡a, canal, etc.) y luego se pregunte:
┬┐Esto que estoy viendo exactamente a qu├® se refiere?

Los encabezados din├ímicos sirven para que el propio informe se lo explique, sin que nadie tenga que interpretarlo.

#### Problema

Imagina un dashboard de ventas que usa filtros de:

- Pa├¡s
- Categor├¡a

Un director abre el informe y ve una cifra grande: Ventas = 2,3M Ôé¼.
Pregunta inmediata: ┬┐Esto es de todas las regiones o solo de alguna?

Aqu├¡ es donde entran los Encabezados din├ímicos: el t├¡tulo del gr├ífico cambia autom├íticamente seg├║n los filtros aplicados.

#### Idea principal

- Si no hay filtro, el t├¡tulo es gen├®rico.
- Si hay filtros, el t├¡tulo dice exactamente qu├® est├í seleccionado.

```
ISFILTERED Pa├¡s = 
IF(
    ISFILTERED(dim_Clientes[Pa├¡s]),
    VAR vFiltros = FILTERS(dim_Clientes[Pa├¡s])
    VAR vTexto = CONCATENATEX(vFiltros, dim_Clientes[Pa├¡s], ", ")
    RETURN
        "Pa├¡s: " & vTexto
)
```

```
ISFILTERED Categor├¡a = 
IF(
    ISFILTERED(dim_Producto[Categor├¡a]),
    VAR vFiltros = FILTERS(dim_Producto[Categor├¡a])
    VAR vTexto = CONCATENATEX(vFiltros, dim_Producto[Categor├¡a], ", ")
    RETURN
        "Categor├¡a: " & vTexto
)
```

![isfiltered](../../docs/imagenes/isfiltered_.png)
