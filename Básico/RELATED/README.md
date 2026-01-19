## RELATED

RELATED se utiliza para traer una columna de una tabla relacionada dentro de una fila concreta de otra tabla.

RELATED funciona cuando se cumplen estas dos condiciones:

- Existe una relación activa entre las tablas

- Estás en contexto de fila (por ejemplo, dentro de SUMX, FILTER, etc.)

```
Total Ventas RELATED = 
SUMX(
    FactSales,
    FactSales[SalesQuantity] * RELATED(DimProduct[UnitPrice])
)
```

![ejemplo](/docs/imagenes/RELATED1.png)

![ejemplo](/docs/imagenes/RELATED2.png)

