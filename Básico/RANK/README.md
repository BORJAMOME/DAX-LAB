## RANK

RANKX devuelve la posición de un valor dentro de un conjunto, según un criterio que defines (normalmente una medida, como ventas).

```
RANKX(
    <tabla>,  // → el conjunto sobre el que haces el ranking
    <expresión>, // → lo que estás rankeando (ventas, cantidad, puntuación…)
    [<orden>],        // → opcional, ASC o DESC
    [<tipo de orden>] // → opcional, skip o dense (cómo se tratan empates)
)
```

### RANK 1

```
Rankx I = RANKX(DimStore,[Ventas Totales])
```

- Calcula ranking dentro del contexto actual del visual

- Si el visual solo tiene algunas tiendas filtradas, el ranking se limita a esas tiendas

- No ignora filtros, ni slicers, ni nada


### RANK 2
```
Rankx II = RANKX(ALLSELECTED(DimStore),[Ventas Totales])
```

- Aquí usamos ALLSELECTED(DimStore) → ignora el desglose por fila, pero respeta lo que el usuario ha seleccionado

- Ideal para visuales con slicers: ranking dinámico basado en lo seleccionado


### Rankx III

```
Rankx III = 
IF(
    HASONEVALUE(DimStore[StoreName]),
    RANKX(ALL(DimStore),[Ventas Totales]),
    BLANK()
)
```

- Uso de HASONEVALUE → solo calcula ranking si hay una tienda en contexto, sino devuelve BLANK()

- ALL(DimStore) → ignora todos los filtros de tienda

- Perfecto para matrices o tarjetas donde solo quieres rank de una tienda visible


### Rankx IV

```
RANKX IV = 
SWITCH(
    TRUE(),
    ISINSCOPE(DimStore[StoreName]), RANKX(ALL(DimStore),[Ventas Totales])
)
```

- Usa ISINSCOPE → se activa si la columna está en el visual

- Si la columna no está en el visual, no hace nada (BLANK implícito)

- Muy útil para jerarquías dinámicas donde el ranking solo tiene sentido en ciertos niveles



![ejemplo](/docs/imagenes/RANK1.png)

![ejemplo](/docs/imagenes/RANK2.png)


