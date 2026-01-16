## Importancia de los Totales

Aquí no estás “calculando una resta”, estás protegiendo el significado del número.

Estas medidas aplican una idea clave: “No todo contexto merece un resultado.”

Por eso:

- Bloqueas totales

- Bloqueas contextos ambiguos

- Bloqueas filas irrelevantes

- Y solo devuelves valor cuando el cálculo tiene sentido


![ejemplo](/docs/imagenes/Totales.png)


```
Sin Total = 
IF(
    HASONEVALUE(dim_Producto[Color]) &&
    NOT ISBLANK([Cantidad]) &&
    NOT("Rojo") IN VALUES(dim_Producto[Color]),
    [Color_Rojo] - [Cantidad]
)
```

```
Diferencia_Rojo = [Color_Rojo] - [Cantidad]
```

```
Sin Total VAR = 
VAR vCantidad = [Cantidad] 
VAR vRojo = [Color_Rojo] 		
RETURN
    IF(
        HASONEVALUE(dim_Producto[Color]) &&
        NOT ISBLANK([Cantidad]) &&
        NOT("Rojo") IN VALUES(dim_Producto[Color]),
        vRojo - vCantidad
    )

```
