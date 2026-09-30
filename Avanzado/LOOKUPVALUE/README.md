## LOOKUPVALUE

LOOKUPVALUE devuelve un valor de una columna cuando encuentra una fila que cumple una o varias condiciones de búsqueda.

En palabras: dame este dato de esta tabla donde se cumpla esta condición.

```
LOOKUPVALUE(
    <columna_resultado>,
    <columna_búsqueda>,
    <valor_búsqueda>
    [, <columna_búsqueda_2>, <valor_búsqueda_2> …]
    [, <resultado_alternativo>]
)
```


### Ejemplo

Creamos las siguientes columnas:

```
Columna RELATED = 
RELATED(DimChannel[ChannelName])
```

```
LOOKUPVALUE = 
LOOKUPVALUE(
    DimChannel_2[ChannelName],
    DimChannel_2[IndSubChannel],
    FactSales[IndSubChannel]
)
```

**Qué ocurre aquí:**

1. Para cada fila de FactSales

2. Busca en DimChannel_2

3. La fila donde:

```
DimChannel_2[IndSubChannel] = FactSales[IndSubChannel]
```

4. Y devuelve ChannelName


Es el BUSCARV de DAX.

### RELATED o LOOKUPVALUE

La captura compara las dos columnas y devuelven lo mismo. La diferencia está en cuándo usar cada una:

- **RELATED** necesita una relación en el modelo. Es más rápida y es la primera opción.
- **LOOKUPVALUE** no necesita relación. La uso cuando la clave no puede ser relación (por ejemplo, una tabla auxiliar que se busca por dos columnas) o cuando crear la relación introduciría ambigüedad.

Si hay varias filas que cumplen la condición con valores distintos, LOOKUPVALUE da error. El último argumento opcional (`resultado_alternativo`) solo cubre el caso de *no encontrar* nada.


![ejemplo](/docs/imagenes/LOOKUPVALUE.png)













