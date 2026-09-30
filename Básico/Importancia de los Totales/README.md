## Importancia de los Totales

Una resta entre dos medidas puede ser correcta en cada fila y absurda en el total. Estas medidas deciden en qué celdas el número significa algo y dejan en blanco el resto.

El caso: diferencia entre las ventas de rojo y las del color de la fila. Tiene sentido para cada color *que no sea* rojo. En la fila Rojo es cero por definición, y en el total compara rojo con la suma de todos los colores, un número que nadie ha pedido.


![ejemplo](/docs/imagenes/Totales.png)


Con las tres condiciones:

```DAX
Sin Total =
IF (
    HASONEVALUE ( dim_Producto[Color] )
        && NOT ISBLANK ( [Cantidad] )
        && NOT ( "Rojo" IN VALUES ( dim_Producto[Color] ) ),
    [Color_Rojo] - [Cantidad]
)
```

1. `HASONEVALUE` descarta el total (varios colores).
2. `NOT ISBLANK` descarta colores sin ventas.
3. `NOT ( "Rojo" IN … )` descarta la fila de rojo.

Sin control, la resta aparece en todas las celdas:

```DAX
Diferencia_Rojo = [Color_Rojo] - [Cantidad]
```

La misma medida con variables, que es como la escribo hoy: cada medida se calcula una vez y la condición se lee mejor.

```DAX
Sin Total VAR =
VAR vCantidad = [Cantidad]
VAR vRojo = [Color_Rojo]
VAR vColor = SELECTEDVALUE ( dim_Producto[Color] )
RETURN
    IF (
        NOT ISBLANK ( vColor ) && vColor <> "Rojo" && NOT ISBLANK ( vCantidad ),
        vRojo - vCantidad
    )
```

`SELECTEDVALUE` sustituye a HASONEVALUE + VALUES: devuelve el color si hay uno solo y blanco si hay varios.
