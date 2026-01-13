## ALL vs REMOVEFILTERS en DAX

Si alguna vez te has preguntado cómo ignorar filtros en Power BI, ALL y REMOVEFILTERS son tus herramientas mágicas. La diferencia clave es:

  - ALL ignora filtros y puede redefinir el contexto completo de la columna o tabla.

  - REMOVEFILTERS quita filtros existentes pero respeta filtros que tú apliques en la misma medida.


![ejemplo](/docs/imagenes/ALLREMOVE.png)


Imagina que estás en tu tienda de ropa y quieres ver cuántas camisetas se han vendido en total, sin importar el color que haya seleccionado tu cliente en el dashboard.

  - ALL es como decir: “Ignora por completo lo que el cliente ha elegido, mira todas las camisetas”.
    - Quita todos los filtros de esa columna (o tabla) para ese cálculo. Redefine el contexto.

  - REMOVEFILTERS es más sutil: “Olvida los filtros que vienen de la página, pero respeta lo que yo te diga en este cálculo”.
    - Elimina filtros que vinieran de slicers o del informe, pero permite filtros que tú apliques dentro de CALCULATE.


### Explicando las medidas:

```
CALCULATE(
  [Cantidad],
  REMOVEFILTERS(dim_Producto[Color])
)
```
 - Quita filtros que vinieran de la página o de slicers sobre colores.

 - Si dentro de CALCULATE no aplicas un filtro nuevo, devuelve el total de todos los colores.


#### Todos_Colores_ALL
```
CALCULATE(
  [Cantidad],
  ALL(dim_Producto[Color])
)
```
- Quita todos los filtros sobre la columna Color, incluidos los que tú puedas aplicar en CALCULATE.

- Resultado similar a REMOVEFILTERS si no hay filtros internos, pero más rígido: ignora todo lo externo e interno.


#### Color_Rojo
```
CALCULATE(
  [Cantidad],
  dim_Producto[Color] = "Rojo"
)
```
- Filtra el total de ventas solo para color rojo.

- Respeta otros filtros del dashboard (fecha, tienda, etc.).


#### Color_Rojo_REMOVEFILTERS
```
CALCULATE(
  [Cantidad],
  dim_Producto[Color] = "Rojo",
  REMOVEFILTERS(dim_Producto[Color])
)
```
- Primero quita cualquier filtro externo sobre Color.

- Después aplica el filtro interno Color = Rojo.

- Resultado: siempre obtienes las ventas de rojo, ignorando cualquier selección que haya hecho el usuario.

