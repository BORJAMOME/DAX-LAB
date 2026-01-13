## ALLEXCEPT

ALLEXCEPT es como decirle a DAX:
"Ignora todos los filtros de la tabla, excepto los que me interesan".

Es útil cuando quieres calcular totales o promedios dentro de un grupo específico, sin que otros filtros te distorsionen el resultado.

![ejemplo](/docs/imagenes/ALLEXCEPT.png)

Imagínate que eres un gerente de ventas y tienes una tabla enorme de transacciones: colores de productos, cantidad vendida, país del cliente…

  - Si quieres saber cuántas unidades se vendieron por país, no te importa qué color ni qué tienda; solo quieres que el país permanezca.

  - ALLEXCEPT actúa como un guardián: borra todos los filtros que estén encima, pero deja intacto aquel que tú elijas, en este caso el País.

```
ALLEXCEPT_País = CALCULATE ( 
                  [Cantidad], 
                  ALLEXCEPT ( Ventas, DIM_Clientes[País] ) 
                  )
```

- CALCULATE recalcula la medida [Cantidad] bajo un nuevo contexto.

- ALLEXCEPT elimina todos los filtros aplicados sobre la tabla Ventas, menos el filtro de país.

### Diferencia clave:

- ALL(Ventas) eliminaría todos los filtros, país incluido.

- ALLEXCEPT mantiene los que tú elijas, y eso es muy útil cuando quieres totales por grupo específico, ignorando otros atributos como color o producto.


### Cuándo no usarlo:

- Si solo quieres ignorar un filtro aislado, a veces ALL + FILTER puede ser más directo.

- Con tablas muy grandes, usar ALLEXCEPT con muchas columnas puede afectar performance.
