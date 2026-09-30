## Filtrado con nombre de selección en tarjeta

Mostrar exactamente lo que el usuario ha seleccionado en una tarjeta.


```
Valores País =
CONCATENATEX(
    VALUES(DIM_Clientes[País]),
    DIM_Clientes[País],
    ", "
)
```

1. VALUES(DIM_Clientes[País]) → recoge los países seleccionados

- Si hay 1 país → tabla con 1 fila

- Si hay varios → tabla con varias filas

- Si no hay selección → todos los países

Con 40 países la tarjeta se vuelve ilegible. En producción, a partir de cierto número muestro "40 países seleccionados" en lugar de la lista.

2. CONCATENATEX(..., ", ") → concatena los valores en una cadena separada por comas

   Ej: "España, México, Argentina"


![ejemplo](/docs/imagenes/Tarjeta.png)


* Si se quiere ordenar:
  
```
CONCATENATEX(
    VALUES(DIM_Clientes[País]),
    DIM_Clientes[País],
    ", ",
    DIM_Clientes[País], ASC
)
```

### Variantes

***Mostrar primer y último valor***

```
Primer y Último País =
MIN(DIM_Clientes[País]) & " - " & MAX(DIM_Clientes[País])
```

![ejemplo](/docs/imagenes/Tarjeta_II.png)


***Incluir conteo***

```DAX
Países Seleccionados =
CONCATENATEX (
    VALUES ( DIM_Clientes[País] ),
    DIM_Clientes[País] & " (" & CALCULATE ( COUNTROWS ( DIM_Clientes ) ) & ")",
    ", ",
    DIM_Clientes[País], ASC
)
```

Número de clientes por país. `CALCULATE` convierte el país de la iteración en filtro (transición de contexto), así que no hace falta el `FILTER` con `EARLIER` de la primera versión, que hacía lo mismo recorriendo la tabla entera por cada país.

![ejemplo](/docs/imagenes/Tarjeta_III.png)



