## Filtrar ignorando el contexto externo

En el siguiente caso, las tarjetas creadas no “obedecen” al segmentador de categoría, porque dentro de CALCULATE se redefine explícitamente el filtro sobre Color.


#### Medidas 

```
Plateado =
CALCULATE(
    [Cantidad],
    dim_Producto[Color] = "Plateado"
)
```

```
Semáforo =
CALCULATE(
    [Cantidad],
    dim_Producto[Color] IN { "Rojo", "Amarillo", "Azul" }
)
```

![ejemplo](/docs/imagenes/sincontexto.png)

![ejemplo](/docs/imagenes/sincontexto_II.png)

