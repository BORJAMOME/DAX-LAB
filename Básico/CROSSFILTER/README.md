## CROSSFILTER

CROSSFILTER cambia la dirección en que se propaga el filtro por una relación, solo durante una medida.

Por defecto, en una relación 1:N el filtro va de la tabla "uno" a la "muchos": de `dim_Clientes` a `Ventas`, de `dim_Producto` a `Ventas`. Filtrar un color reduce las ventas, pero **no** reduce la lista de clientes, porque el filtro no sube de `Ventas` a `dim_Clientes`.

Eso es un problema cuando la pregunta es "¿cuántos clientes han comprado productos rojos?".

```DAX
CROSSFILTER ( <columna1>, <columna2>, <dirección> )
```

Direcciones posibles:

- **OneWay**: la dirección normal de la relación.
- **Both**: el filtro se propaga en los dos sentidos.
- **None**: la relación se ignora.

(Hay dos variantes más, `OneWay_LeftFiltersRight` y `OneWay_RightFiltersLeft`, para forzar un sentido concreto.)

### Las medidas

```DAX
Clientes-Colores =
CALCULATE (
    [Total_Clientes],
    CROSSFILTER ( dim_Clientes[IdCliente], Ventas[IdCliente], BOTH )
)
```

Con Both, el color filtra Ventas y Ventas filtra Clientes: cuenta solo los clientes que compraron ese color.

```DAX
Clientes-Reino Unido =
CALCULATE (
    COUNT ( dim_Clientes[IdCliente] ),
    dim_Clientes[País] = "Reino Unido"
)
```

```DAX
Reino Unido-Colores =
CALCULATE (
    [Clientes-Reino Unido],
    CROSSFILTER ( dim_Clientes[IdCliente], Ventas[IdCliente], BOTH )
)
```

Clientes de Reino Unido que compraron el color de la fila.

| Medida | Qué cuenta |
|---|---|
| `Total_Clientes` | Todos los clientes; el color no le afecta |
| `Clientes-Colores` | Clientes que compraron ese color |
| `Clientes-Reino Unido` | Clientes de Reino Unido; el color no le afecta |
| `Reino Unido-Colores` | Clientes de Reino Unido que compraron ese color |

![ejemplo](/docs/imagenes/crossfilter.png)

A la derecha, las mismas medidas sin CROSSFILTER (las etiquetadas "sin magia"): el color no cambia nada y todas las filas repiten 18.484 y 1.913. La columna `Compradores` de la izquierda es `DISTINCTCOUNT ( Ventas[IdCliente] )` y coincide con `Clientes-Colores`, lo que confirma la alternativa del final.

### Por qué en la medida y no en el modelo

Podría marcar la relación como bidireccional en el modelo y ahorrarme CROSSFILTER. No lo hago: una relación bidireccional afecta a todas las medidas y visuales, puede crear ambigüedad si hay más de un camino entre tablas y hace el modelo más lento. Con CROSSFILTER, el comportamiento bidireccional se limita a la medida que lo necesita.

Una alternativa sin CROSSFILTER es contar clientes desde la tabla de hechos: `DISTINCTCOUNT ( Ventas[IdCliente] )` ya responde "clientes que compraron", porque Ventas sí recibe el filtro de color.
