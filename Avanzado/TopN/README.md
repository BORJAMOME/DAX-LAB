## TOPN

TOPN devuelve una tabla con las N filas de mayor (o menor) valor según una expresión. No devuelve un número: hay que usarla dentro de CALCULATE o de un iterador.

![ejemplo](/docs/imagenes/topn.png)

### Ventas de los 3 productos que más venden

```DAX
Top 3 Productos =
VAR vTop3 =
    TOPN ( 3, ALLSELECTED ( dim_Producto[Producto] ), [Cantidad] )
RETURN
    CALCULATE ( [Cantidad], vTop3 )
```

Dos detalles que importan:

- Uso `ALLSELECTED ( dim_Producto[Producto] )` y no `'dim_Producto'`. Con la tabla entera, TOPN evalúa la medida fila a fila sobre todas las columnas de la dimensión, y en una tabla por producto el contexto de la fila la reduce a un solo producto.
- Con empates, TOPN devuelve **más** de N filas. Si el tercero y el cuarto venden lo mismo, entran los dos.

### Nombre del producto ganador

```DAX
Producto Top Ventas =
VAR vTop1 =
    TOPN ( 1, ALLSELECTED ( dim_Producto[Producto] ), [Cantidad] )
RETURN
    CONCATENATEX ( vTop1, dim_Producto[Producto], ", " )
```

CONCATENATEX en lugar de MAXX para que, si hay empate, aparezcan los dos nombres.

### Top N elegido por el usuario

Tabla desconectada para el slicer:

```DAX
Selector Top N =
DATATABLE ( "Top", INTEGER, { { 3 }, { 5 }, { 10 } } )
```

```DAX
Ventas Top Dinámico =
VAR N = SELECTEDVALUE ( 'Selector Top N'[Top], 3 )
VAR vTopN =
    TOPN ( N, ALLSELECTED ( dim_Producto[Producto] ), [Cantidad] )
RETURN
    CALCULATE ( [Cantidad], vTopN )
```

El valor por defecto (3) es uno de los que ofrece el slicer, para que la tarjeta y el slicer no se contradigan cuando no hay selección.

### Marcar las filas que están en el Top N

```DAX
Marca Top Dinámico =
VAR N = SELECTEDVALUE ( 'Selector Top N'[Top], 3 )
VAR vTopN =
    TOPN ( N, ALLSELECTED ( dim_Producto[Producto] ), [Cantidad] )
RETURN
    IF ( SELECTEDVALUE ( dim_Producto[Producto] ) IN vTopN, "●" )
```

`IN` sustituye a CONTAINS con una sintaxis más corta. La misma lógica, devolviendo un código de color, sirve para resaltar barras (lo aplico en el [widget de control de presupuestos](<../../Proyectos/Widget Control de presupuestos/>)).
