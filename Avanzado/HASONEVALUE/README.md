## HASONEVALUE

HASONEVALUE devuelve TRUE cuando una columna tiene exactamente un valor visible en el contexto. Lo uso para saber a qué nivel de detalle se está calculando una celda y cambiar la lógica o el texto según ese nivel.

![ejemplo](/docs/imagenes/HASONEVALUE.png)

> La captura es de una primera versión que aplicaba un IVA distinto por nivel (21 % diario, 10 % mensual…). Como regla fiscal no tiene sentido; la dejé porque enseña bien cómo cambia el resultado en cada nivel. Las medidas de abajo usan el mismo mecanismo con un caso de negocio real.

```DAX
Un valor Año = HASONEVALUE ( Calendario_II[Año] )
```

```DAX
Un valor Trimestre = HASONEVALUE ( Calendario_II[Trimestre] )
```

En una matriz Año → Trimestre → Mes → Día, la fila de un trimestre tiene un año y un trimestre, pero varios meses. El total general no tiene un valor único en ninguna columna.

### Etiqueta del nivel actual

```DAX
Nivel Actual =
SWITCH (
    TRUE (),
    HASONEVALUE ( Calendario_II[Día] ),       "Día",
    HASONEVALUE ( Calendario_II[Mes] ),       "Mes",
    HASONEVALUE ( Calendario_II[Trimestre] ), "Trimestre",
    HASONEVALUE ( Calendario_II[Año] ),       "Año",
    "Total"
)
```

El orden importa: de lo más fino a lo más grueso. En una fila de día también hay un único mes y un único año, así que si preguntara primero por Año, todo sería "Año".

### Cálculo distinto según el nivel

Un caso real: en el nivel de mes quiero ver ventas; en trimestre y año, la media mensual, porque es lo que se compara entre periodos de distinta longitud.

```DAX
Ventas o Media Mensual =
IF (
    HASONEVALUE ( Calendario_II[Mes] ),
    [Cantidad],
    AVERAGEX ( VALUES ( Calendario_II[Mes] ), [Cantidad] )
)
```

### HASONEVALUE, SELECTEDVALUE e ISINSCOPE

- `SELECTEDVALUE ( col )` es `IF ( HASONEVALUE ( col ), VALUES ( col ) )`. Si necesito el valor, uso SELECTEDVALUE directamente.
- HASONEVALUE es TRUE también cuando el valor único viene de un slicer, aunque la columna no esté en el visual. Si lo que quiero saber es si la columna está en las filas de la matriz, uso [ISINSCOPE](<../ISINSCOPE/>).
