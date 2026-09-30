## Gráfico de columnas agrupadas

Este proyecto muestra cómo crear un **gráfico de columnas agrupadas** en Power BI para comparar series de datos del **año actual (CY)** y **año anterior (PY)**, destacando variaciones positivas y negativas mediante etiquetas dinámicas.  

El caso es contable: comparar el saldo final de cada cuenta entre un año y el anterior.


![ejemplo](/docs/imagenes/ColumnasAgrupadas.png)



## Objetivo

- Comparar el **saldo final** de cada cuenta entre el año actual y el año anterior.
- Resaltar de forma visual las **variaciones positivas y negativas** mediante etiquetas dinámicas.
- Preparar los datos para un **gráfico de columnas agrupadas**, donde cada columna representa CY y PY, y las etiquetas indican la magnitud de la variación.

## Medidas DAX

### 1. Saldo de cada línea
```DAX
02_Salto_Linea = SUM(Cuentas[Saldo])
```

El saldo es una medida semiaditiva: no se suma a lo largo del tiempo. El saldo del año es el de la última línea, no la suma de todas.

### 2. Saldo final del año actual (CY)
```
02_Saldo Final =
VAR final = MAX(Cuentas[Id])
RETURN
CALCULATE(
    [02_Salto_Linea],
    Cuentas[Id] = final
)
```

### 3. Saldo final del año anterior (PY)
```
02_Saldo_Final_PY =
CALCULATE([02_Saldo Final],
SAMEPERIODLASTYEAR('01_CALENDARIO'[Date])
)
```

### 4. Variación positiva y negativa
```
02_Variacion_Positiva = IF([02_Saldo Final] > [02_Saldo_Final_PY], [02_Saldo Final])
02_Variacion_Negatva = IF([02_Saldo Final] < [02_Saldo_Final_PY], [02_Saldo_Final_PY])
```

### 5. Etiquetas de variación
```
02_Etiqueta_Variacion_CY =
IF([02_Variacion_Positiva] > [02_Variacion_Negatva],
    [02_Saldo Final] - [02_Saldo_Final_PY],
    BLANK()
)

02_Etiqueta_Variacion_PY =
IF([02_Variacion_Positiva] < [02_Variacion_Negatva],
    [02_Saldo Final] - [02_Saldo_Final_PY],
    BLANK()
)
```

El `.pbix` está en esta misma carpeta.
