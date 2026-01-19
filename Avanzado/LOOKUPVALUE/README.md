## LOOKUPVALUE

LOOKUPVALUE devuelve un valor de una columna cuando encuentra una fila que cumple una o varias condiciones de búsqueda.

-> _“Dame este dato de esta tabla cuando se cumpla esta condición.”

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

-> _“Recorre esta tabla, encuentra la fila donde esta clave coincide, y tráeme ese valor.”

Es como un BUSCARV de Excel, pero en DAX.


![ejemplo](/docs/imagenes/LOOKUPVALUE.png)













