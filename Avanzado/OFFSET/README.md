## OFFSET

OFFSET te mueve por una lista ordenada y te devuelve la fila vecina (anterior, siguiente, n posiciones).

Dado un conjunto ordenado:

[A] [B] [C] [D] [E]

- OFFSET(-1) → B

- OFFSET(1) → D

Pero DAX no adivina el orden. Tú se lo tienes que imponer.


### Como funciona la medida:

```
OFFSET(
    desplazamiento,
    tabla,
    ORDERBY(expresión, ASC | DESC)
)
```

- 1 → anterior

- +1 → siguiente

- ORDERBY → el eje real del movimiento

### Producto anterior

```
OFFSET -1 = 
    CALCULATE(
        [Cantidad], 
        OFFSET(
            -1,
            ALLSELECTED(dim_Producto[Categoría])
        )
    )
```

### Producto siguiente

```
OFFSET +1 = 
    CALCULATE(
        [Cantidad], 
        OFFSET(
            1,
            ALLSELECTED(dim_Producto[Categoría])
        )
    )
```


![ejemplo](/docs/imagenes/OFFSET.png)







