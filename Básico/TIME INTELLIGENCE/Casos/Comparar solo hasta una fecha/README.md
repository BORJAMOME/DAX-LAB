## Comparar con el año anterior solo hasta la última fecha con datos

Hoy es 13 de enero de 2024. En una tabla por meses, la fila de enero de 2024 tiene 13 días de ventas, y la columna "año anterior" en el total suma todo 2023, incluido diciembre. La comparación está sesgada a favor del año pasado.

La solución: calcular el año anterior y además cortarlo en la misma fecha del año pasado.

### Corte al día

```DAX
Año Pasado Hasta el Día =
VAR vUltimaFechaActual = MAX ( Ventas[Fecha] )
VAR vUltimaFechaAA = EDATE ( vUltimaFechaActual, -12 )
RETURN
    CALCULATE (
        [Cantidad],
        SAMEPERIODLASTYEAR ( Calendario_II[Fecha] ),
        Calendario_II[Fecha] <= vUltimaFechaAA
    )
```

1. `MAX ( Ventas[Fecha] )` encuentra el último día con ventas (13/01/2024).
2. EDATE lo lleva un año atrás (13/01/2023).
3. CALCULATE recibe dos filtros sobre `Calendario_II[Fecha]` y los intersecta: las fechas del año anterior que caen hasta el 13/01/2023.

Detalle importante: `MAX ( Ventas[Fecha] )` se evalúa en el contexto de la celda. Si el visual tiene fechas en las filas, cada fila tiene su propio máximo. Para que el corte sea siempre la última fecha global, uso `CALCULATE ( MAX ( Ventas[Fecha] ), REMOVEFILTERS ( Calendario_II ) )`.

### Corte a fin de mes

```DAX
Año Pasado Fin Último Mes =
VAR vUltimaFechaActual = MAX ( Ventas[Fecha] )
VAR vUltimaFechaAA = EOMONTH ( vUltimaFechaActual, -12 )
RETURN
    CALCULATE (
        [Cantidad],
        SAMEPERIODLASTYEAR ( Calendario_II[Fecha] ),
        Calendario_II[Fecha] <= vUltimaFechaAA
    )
```

Igual, pero corta el 31/01/2023. Lo uso cuando el negocio compara por meses completos.

### Sin corte, para comparar

```DAX
Año Pasado DATEADD =
CALCULATE (
    [Cantidad],
    DATEADD ( Calendario_II[Fecha], -1, YEAR )
)
```

![ejemplo](/docs/imagenes/fecha_II.png)

En esta captura las tres columnas coinciden porque el modelo tiene 2024 completo: la última fecha con ventas es el 31/12/2024 y el corte no elimina nada. La diferencia aparece cuando el año en curso está a medias. En ese caso, en el total de 2024 la versión DATEADD suma todo 2023 y las otras dos solo hasta el día (o el mes) equivalente.
