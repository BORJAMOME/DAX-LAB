## Comparar sólo hasta la fecha

Queremos comparar el año pasado pero solo hasta el mismo punto en el tiempo que tenemos datos este año.

- Evitamos que la comparación incluya meses futuros del año anterior que todavía no tenemos en el año actual.

Imagina el siguiente caso:

Hoy es 13 de enero de 2024.

- Quieres ver cuántas unidades vendiste hasta ahora, pero también cuántas vendiste en el mismo periodo del año pasado.

No tendría sentido sumar diciembre de 2023 en tu comparativa, porque todavía no ha llegado diciembre de 2024.

### Año pasado Fin último Mes
```
Año pasado Fin último Mes = 
	VAR vUltimaFechaActual = MAX(Ventas[Fecha]) 
	VAR vUltimaFecha = EOMONTH(vUltimaFechaActual,-12) 
	RETURN
		CALCULATE([Cantidad], 
			SAMEPERIODLASTYEAR(
				Calendario_II[Fecha]
				), 
			Calendario_II[Fecha] <= vUltimaFecha
		)
```

### Año Pasado DATEADD
```
Año Pasado DATEADD = 
	CALCULATE(
		[Cantidad],
		DATEADD(
			Calendario_II[Fecha], -1, YEAR
		)
```

### Año pasado hasta el día
```
Año pasado hasta el día = 
	VAR vUltimaFechaActual = MAX(Ventas[Fecha])
	// VAR vUltimaFecha = EDATE(LASTDATE(Ventas[Fecha]),-12) 
    	VAR vUltimaFecha = EDATE(vUltimaFechaActual,-12) 
		CALCULATE([Cantidad], 
			SAMEPERIODLASTYEAR(
				Calendario_II[Fecha]
				), 
		Calendario_II[Fecha] <= vUltimaFecha
	)
```

![ejemplo](/docs/imagenes/fecha_II.png)





