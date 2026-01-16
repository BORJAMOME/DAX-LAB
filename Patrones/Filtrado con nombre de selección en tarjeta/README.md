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

- Si no hay selección → tabla con todos los países visibles por filtro

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

```
Países Seleccionados =
CONCATENATEX(
    VALUES(DIM_Clientes[País]),
    DIM_Clientes[País] & " (" & COUNTROWS(FILTER(DIM_Clientes, DIM_Clientes[País]=EARLIER(DIM_Clientes[País]))) & ")",
    ", "
)
```

![ejemplo](/docs/imagenes/Tarjeta_III.png)



