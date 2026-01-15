## Comparación de datos hasta la fecha

Queremos comparar lo que ha pasado hasta hoy con lo que pasó el mismo periodo del año anterior.

- [Última Fecha Hechos] → nos dice cuál es el último día con datos.

- [Última Fecha Año Anterior] → calcula el mismo día pero un año antes.

- DATEADD → mueve fechas y permite calcular ventas del año pasado hasta la misma fecha.

En pocas palabras: miramos nuestro dashboard hoy y vemos cómo vamos comparado con el año pasado, hasta la misma fecha.

Imagina el siguiente escenario:

- Hoy es 13 de Marzo de 2023 y quieres saber cuánto has vendido hasta ahora.

- También quieres ver cuánto habías vendido hasta 13 de enero de 2025, para comparar el progreso.


#### Última Fecha Hechos

```
Última Fecha Hechos = MAX(Ventas[Fecha])
```
Busca la fecha máxima disponible en la tabla de ventas.


#### Última Fecha Año Anterior
```
Última Fecha Año Anterior = EOMONTH([Última Fecha Hechos], -12)
```
- EOMONTH mueve la fecha al final del mes un año atrás.

- Útil cuando quieres comparativas mensuales “acumuladas hasta el mes”.


#### Año Pasado DATEADD
```
Año Pasado DATEADD = 
CALCULATE(
    [Cantidad],
    DATEADD(
        Calendario_II[Fecha], -1, YEAR
    )
)
```

- DATEADD desplaza las fechas una unidad temporal (en este caso, -1 año).

- CALCULATE recalcula [Cantidad] bajo el nuevo contexto de fechas.



##### Diferencias clave con otras funciones de Time Intelligence:

- SAMEPERIODLASTYEAR hace algo parecido pero solo funciona para periodos completos de calendario.

- DATEADD es más flexible: sirve para mover cualquier periodo (días, meses, trimestres, años) y comparar hasta cualquier fecha.


![ejemplo](/docs/imagenes/fecha_I.png)






