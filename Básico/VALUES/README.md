## VALUES

VALUES devuelve “lo que hay en contexto ahora mismo”.

VALUES devuelve los valores distintos visibles en el contexto actual.

- Si le pasas una columna → tabla de valores únicos

- Si le pasas una tabla → filas visibles (aunque se repitan)

- Puede devolver BLANK() si el contexto lo exige

### Caso 1

```
Tabla VALUES = VALUES(DimProduct[BrandName])
```

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

