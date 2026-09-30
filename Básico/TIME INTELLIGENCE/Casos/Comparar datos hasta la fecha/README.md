## Comparar con el año anterior hasta la misma fecha

Pregunta de negocio: a día de hoy, ¿vamos mejor o peor que el año pasado *en el mismo punto del año*?

Ejemplo: los datos llegan hasta el 13 de marzo de 2024. Comparar el acumulado de 2024 con todo 2023 no tiene sentido; hay que comparar con el acumulado hasta el 13 de marzo de 2023.

### Última fecha con datos

```DAX
Última Fecha Hechos = MAX ( Ventas[Fecha] )
```

La fecha máxima de la tabla de hechos, no del calendario. El calendario suele llegar a final de año y daría una fecha sin ventas.

### Misma fecha un año antes

```DAX
Última Fecha Año Anterior = EDATE ( [Última Fecha Hechos], -12 )
```

EDATE mueve la fecha 12 meses y conserva el día: 13/03/2024 → 13/03/2023. Si quisiera comparar por meses cerrados, usaría `EOMONTH ( …, -12 )`, que lleva al último día del mes (31/03/2023).

### Año anterior con DATEADD

```DAX
Año Pasado DATEADD =
CALCULATE (
    [Cantidad],
    DATEADD ( Calendario_II[Fecha], -1, YEAR )
)
```

DATEADD desplaza un año las fechas que haya en el contexto. En una fila de marzo de 2024 devuelve marzo de 2023.

![ejemplo](/docs/imagenes/fecha_I.png)

### DATEADD y SAMEPERIODLASTYEAR

`SAMEPERIODLASTYEAR ( Calendario[Fecha] )` equivale a `DATEADD ( Calendario[Fecha], -1, YEAR )`. Uso SAMEPERIODLASTYEAR cuando solo necesito el año anterior porque se lee mejor, y DATEADD cuando el desplazamiento es otro (meses, trimestres, días).

Ninguna de las dos resuelve por sí sola el problema de esta página: en el total del año devuelven el año anterior *completo*. El corte en la última fecha con datos está en [Comparar solo hasta una fecha](<../Comparar solo hasta una fecha/>).
