## VALUES

VALUES devuelve los valores distintos visibles en el contexto actual.

- Con una columna: una tabla con los valores únicos de esa columna.
- Con una tabla: las filas visibles de la tabla.
- En los dos casos puede incluir una fila en blanco extra si la tabla de hechos tiene claves que no existen en la dimensión (la "fila desconocida" que crea Power BI).

### Como tabla calculada

```DAX
Tabla VALUES = VALUES ( DimProduct[BrandName] )
```

Una tabla con una fila por marca.

```
TieneUnaMarca = 
IF(
    HASONEVALUE( DimProduct[BrandName] ),
    "Una sola marca",
    "Varias marcas"
)
```

```
Ventas por Marca Visible = 
SUMX(
    VALUES( DimProduct[BrandName] ),
    [Ventas Totales]
)
```

![ejemplo](/docs/imagenes/VALUES1.png)


### Con la marca en las filas del visual

Mismas medidas, pero ahora `DimProduct[BrandName]` está en las filas. En cada fila hay una sola marca (`TieneUnaMarca` dice "Una sola marca") y `Ventas por Marca Visible` coincide con `[Ventas Totales]`. Solo en el total aparecen varias marcas.

`SUMX ( VALUES ( … ), [medida] )` es el patrón que uso cuando una medida no es aditiva y necesito calcularla marca a marca antes de sumar.

![ejemplo](/docs/imagenes/VALUES2.png)



