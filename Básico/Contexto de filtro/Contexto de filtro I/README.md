## Contexto de filtro

Tenemos la misma métrica (Cantidad), pero en una tarjeta vemos el total y en otra solo lo rojo.

Aquí solo cambia el CONTEXTO. 

#### Razonamiento 

- Cantidad es la pregunta: ¿Cuántas unidades se han vendido?
- Color_Rojo es la misma pregunta, pero añadiendo: …solo de productos rojos.

```
Cantidad = SUM(Ventas[Cantidades])

```


```
Color_Rojo =
CALCULATE(
    [Cantidad],
    dim_Producto[Color] = "Rojo"

```


- CALCULATE modifica el contexto de filtro.
- Añade una condición: “Solo productos cuyo color sea Rojo”


![filtro](/docs/imagenes/Contexto_Filtro.png)
