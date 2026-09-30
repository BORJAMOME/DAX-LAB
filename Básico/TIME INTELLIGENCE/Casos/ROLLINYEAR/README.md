## Rolling year: los últimos 12 meses

Pregunta de negocio: ¿cuántas unidades vendemos al día, de media, *ahora*? No en el año natural ni desde enero, sino en el último año completo contado hacia atrás.

Comparar noviembre de 2023 con todo 2022 mezcla estacionalidad y cambios de negocio. Una ventana móvil de 12 meses da una cifra que se actualiza sola cada mes y siempre abarca un ciclo completo.

- En noviembre de 2023: del 1/12/2022 al 30/11/2023.
- En junio de 2024: del 1/7/2023 al 30/6/2024.

No hay una función ROLLINGYEAR en DAX; el patrón se construye con DATESINPERIOD.

```DAX
UnidadesVendidas/día Rolling 12M =
CALCULATE (
    [UnidadesVendidas / día],
    DATESINPERIOD (
        dim_Calendario[Fecha],
        MAX ( dim_Calendario[Fecha] ),
        -12,
        MONTH
    )
)
```

1. `MAX ( dim_Calendario[Fecha] )` da el último día del contexto. En la fila de noviembre de 2023, el 30/11/2023.
2. DATESINPERIOD devuelve los 12 meses que terminan ese día.
3. `[UnidadesVendidas / día]` divide unidades entre días laborables de ese periodo (la medida base está en [SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD](<../SAMEPERIODLASTYEAR, DATEADD y PARALLELPERIOD/>)).

![ejemplo](/docs/imagenes/TIRY.png)

Dos cosas a vigilar:

- Los primeros 11 meses del calendario no tienen un año completo detrás, así que la media usa menos meses. Si eso confunde, devuelvo blanco cuando no hay 12 meses de datos.
- Si el calendario llega hasta diciembre pero las ventas se quedan en octubre, noviembre y diciembre muestran una media que no incluye meses reales. Limito el calendario a la última fecha con datos o filtro esas filas.
