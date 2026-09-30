## OFFSET

OFFSET devuelve la fila que está N posiciones antes o después de la actual en una tabla ordenada. Sirve para comparar un elemento con su vecino sin montar la lógica con fechas o rankings.

Con cinco categorías ordenadas `A B C D E`, desde la fila C:

- `OFFSET ( -1, … )` → B
- `OFFSET ( 1, … )` → D

```DAX
OFFSET (
    <desplazamiento>,       -- negativo: hacia atrás; positivo: hacia delante
    <tabla>,
    ORDERBY ( <expresión>, [ASC | DESC] )
)
```

### Categoría anterior y siguiente

```DAX
OFFSET -1 =
CALCULATE (
    [Cantidad],
    OFFSET (
        -1,
        ALLSELECTED ( dim_Producto[Categoría] ),
        ORDERBY ( dim_Producto[Categoría], ASC )
    )
)
```

```DAX
OFFSET +1 =
CALCULATE (
    [Cantidad],
    OFFSET (
        1,
        ALLSELECTED ( dim_Producto[Categoría] ),
        ORDERBY ( dim_Producto[Categoría], ASC )
    )
)
```

La primera categoría no tiene anterior y la última no tiene siguiente: devuelven blanco.

![ejemplo](/docs/imagenes/OFFSET.png)

### El ORDERBY, siempre explícito

Si omito ORDERBY, OFFSET ordena por las columnas de la tabla en orden ascendente. Aquí daría lo mismo (orden alfabético de categoría), pero en cuanto quiero "la anterior por ventas" o "el mes anterior" con un nombre de mes que se ordena mal alfabéticamente, el resultado cambia sin avisar. Lo escribo siempre.

### Caso típico: mes anterior sin Time Intelligence

```DAX
Cantidad Mes Anterior =
CALCULATE (
    [Cantidad],
    OFFSET (
        -1,
        ALLSELECTED ( dim_Calendario[AñoMes] ),
        ORDERBY ( dim_Calendario[AñoMes], ASC )
    )
)
```

Funciona con calendarios que no son estándar (fiscales, 4-4-5) donde DATEADD no encaja. Para rangos de varias filas, la herramienta es [WINDOW](<../WINDOW/>).
