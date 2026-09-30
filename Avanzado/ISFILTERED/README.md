## ISFILTERED

ISFILTERED devuelve TRUE cuando una columna tiene un filtro **directo**: un slicer, un filtro de visual, de página o de informe sobre esa misma columna.

Lo que no detecta es el filtro que llega por una relación. Si el usuario filtra `dim_Producto[Categoría]`, la columna `dim_Producto[Producto]` queda reducida, pero `ISFILTERED ( dim_Producto[Producto] )` sigue devolviendo FALSE. Para eso está `ISCROSSFILTERED`.

![ejemplo](/docs/imagenes/ISFILTERED.png)

### Texto con la selección activa

Lo uso sobre todo para títulos y subtítulos que dicen qué se está viendo:

```DAX
Contexto Selección =
VAR vCategoria =
    IF (
        ISFILTERED ( dim_Producto[Categoría] ),
        "Categoría: "
            & CONCATENATEX ( VALUES ( dim_Producto[Categoría] ), dim_Producto[Categoría], ", " )
    )
VAR vPais =
    IF (
        ISFILTERED ( dim_Clientes[País] ),
        "País: "
            & CONCATENATEX ( VALUES ( dim_Clientes[País] ), dim_Clientes[País], ", " )
    )
RETURN
    SWITCH (
        TRUE (),
        NOT ISBLANK ( vCategoria ) && NOT ISBLANK ( vPais ), vCategoria & " | " & vPais,
        COALESCE ( vCategoria, vPais, "Todas las categorías y países" )
    )
```

1. ISFILTERED decide si hay algo que contar.
2. VALUES recoge los valores visibles y CONCATENATEX los junta en texto.
3. El SWITCH final evita el separador colgando cuando solo hay uno de los dos filtros.

Sobre `FILTERS` frente a `VALUES`: FILTERS devuelve los valores del filtro directo, VALUES los valores visibles tras todos los filtros. Para un título, VALUES suele ser lo que el usuario espera ver.

La aplicación completa en un informe está en [Encabezados dinámicos con ISFILTERED](<../../Básico/ISFILTERED - Encabezados dinámicos/>).
