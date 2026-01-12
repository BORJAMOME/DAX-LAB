## Indicadores de Rendimiento con color

En muchos negocios, los departamentos financieros quieren ver rápidamente qué clientes están cumpliendo con sus pagos y cuáles no, sin tener que mirar números exactos. Un semáforo es perfecto para esto: verde = bien, naranja = alerta, rojo = mal.

#### Ejemplo:
Supongamos que tenemos un modelo de clientes y una medida [Días Pendientes de Pago] que indica cuántos días lleva un cliente sin pagar. Queremos crear un indicador tipo semáforo para que, al ver la matriz de clientes, podamos identificar rápidamente:

- Verde: cliente al día o casi al día

- Naranja: cliente con retraso moderado

- Rojo: cliente con retraso crítico


```
Cobro Cliente =
IF(
    ISINSCOPE(dim_clients[Client_Name]),   -- Solo en filas de cliente, no totales
    SWITCH(
        TRUE(),
        [Días Pendientes de Pago] <= 5, "🟢",   -- Al día o casi al día
        [Días Pendientes de Pago] <= 15, "🟠",  -- Retraso moderado
        "🔴"                                     -- Retraso crítico
    ),
    BLANK()  -- No mostrar nada en totales
)
```

1. ISINSCOPE(dim_clients[Client_Name]) → nos aseguramos de que el semáforo solo aparezca por cliente, no en el total general.

2. SWITCH(TRUE(), ...) → evaluamos la medida [Días Pendientes de Pago] en orden de prioridad: primero verde, luego naranja, si no rojo.

3. BLANK() → evita que el total se muestre con un semáforo, lo cual podría confundir.


--- 

## Manera dinámica


Supongamos que ya tenemos:

```
Días Pendientes de Pago =
AVERAGE(fact_payments[Days_Pending])
```

- Mide el promedio de días de retraso por cliente.

- Se calcula sobre la tabla de pagos (fact_payments).


#### Medida dinámica de límites
Podemos calcular límites basados en percentiles históricos para identificar clientes críticos:

```
Limite Verde =
PERCENTILEX.INC(
    ALL(dim_clients),
    [Días Pendientes de Pago],
    0.50   -- mediana de todos los clientes
)

Limite Naranja =
PERCENTILEX.INC(
    ALL(dim_clients),
    [Días Pendientes de Pago],
    0.80   -- percentil 80
)
```

- Verde: clientes por debajo de la mediana → comportamiento normal

- Naranja: clientes entre mediana y percentil 80 → alerta moderada

- Rojo: por encima del percentil 80 → críticos


#### Indicador dinámico

```
Semáforo Cobro Dinámico =
IF(
    ISINSCOPE(dim_clients[Client_Name]),  -- Solo en filas de cliente
    SWITCH(
        TRUE(),
        [Días Pendientes de Pago] <= [Limite Verde], "🟢",
        [Días Pendientes de Pago] <= [Limite Naranja], "🟠",
        "🔴"
    ),
    BLANK()  -- Evitar semáforo en totales
)
```

- Usa los límites calculados dinámicamente.

- Se ajusta automáticamente si aparecen clientes nuevos o cambian los retrasos.

- Solo aparece por cliente, no en totales.


