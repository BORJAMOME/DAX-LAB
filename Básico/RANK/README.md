## Rankings con RANKX

RANKX devuelve la posición de un valor dentro de una tabla, ordenada por una expresión.

```DAX
RANKX (
    <tabla>,        -- sobre qué conjunto se ordena
    <expresión>,    -- qué se ordena (ventas, cantidad…)
    [<valor>],      -- opcional, casi siempre vacío
    [<orden>],      -- DESC (por defecto) o ASC
    [<empates>]     -- SKIP (por defecto) o DENSE
)
```

La parte que decide si el ranking funciona es el primer argumento. Lo muestro con cuatro versiones sobre una tabla de tiendas de Contoso.

![ejemplo](/docs/imagenes/RANK1.png)

### Rankx I: el error típico

```DAX
Rankx I = RANKX ( DimStore, [Ventas Totales] )
```

Devuelve **1 en todas las filas**. En cada fila de la tabla, `DimStore` ya está filtrada a una sola tienda, así que RANKX compara la tienda consigo misma. La dejo aquí porque es el primer ranking que escribe todo el mundo.

### Rankx II: respetar la selección del usuario

```DAX
Rankx II = RANKX ( ALLSELECTED ( DimStore ), [Ventas Totales] )
```

ALLSELECTED quita el filtro de la fila pero conserva el de los slicers. Si el usuario marca cinco tiendas, el ranking va del 1 al 5. Es la versión que uso por defecto en tablas con segmentadores.

El inconveniente se ve en la fila de total: también devuelve 1, un número sin sentido.

### Rankx III y IV: sin valor en el total

```DAX
Rankx III =
IF (
    HASONEVALUE ( DimStore[StoreName] ),
    RANKX ( ALL ( DimStore ), [Ventas Totales] )
)
```

```DAX
Rankx IV =
IF (
    ISINSCOPE ( DimStore[StoreName] ),
    RANKX ( ALL ( DimStore ), [Ventas Totales] )
)
```

Las dos dejan el total en blanco. La diferencia:

- `HASONEVALUE` también devuelve TRUE si el usuario filtra una sola tienda en un slicer, aunque la tienda no esté en las filas del visual. En una tarjeta, eso da un ranking.
- `ISINSCOPE` solo devuelve TRUE si `StoreName` es una columna de agrupación del visual. Es lo que quiero en matrices con jerarquía.

Ojo con `ALL ( DimStore )`: ignora también los slicers, así que el ranking es siempre contra todas las tiendas. Si quiero un ranking dentro de la selección y sin total, combino `ISINSCOPE` con `ALLSELECTED`.

![ejemplo](/docs/imagenes/RANK2.png)

### Nota: la función RANK

Desde finales de 2023 existe `RANK`, de la familia de WINDOW y OFFSET. Permite desempatar por varias columnas con `ORDERBY`, algo que RANKX no hace. Para rankings simples sigo con RANKX por compatibilidad; si hay empates que importan, uso RANK.
