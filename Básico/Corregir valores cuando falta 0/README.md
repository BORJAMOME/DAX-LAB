## Mostrar 0 en lugar de blanco

Cuando un producto no tiene ventas en un periodo, la medida devuelve BLANK y Power BI esconde la fila. A veces eso es lo que quiero; otras, el usuario necesita ver que ese producto vendió cero.

![ejemplo](/docs/imagenes/cero.png)

```DAX
Blanco Cantidad = IF ( ISBLANK ( [Cantidad] ), 0, [Cantidad] )
```

```DAX
Suma Cero = [Cantidad] + 0
```

```DAX
Cantidad o Cero = COALESCE ( [Cantidad], 0 )
```

Las tres dan el mismo resultado. Uso COALESCE: es la más legible y evalúa la medida una sola vez.

Cuidado: al devolver 0, la matriz muestra **todas** las combinaciones de filas, incluidas las que no existen (todos los productos por todos los meses). En tablas grandes el visual se vuelve lento y lleno de ceros. El caso más habitual son los meses futuros del calendario, que se llenan de ceros. Los corto en la última fecha con ventas:

```DAX
Cantidad o Cero Acotada =
VAR vUltimaVenta = CALCULATE ( MAX ( Ventas[Fecha] ), REMOVEFILTERS () )
RETURN
    IF ( MIN ( dim_Calendario[Fecha] ) <= vUltimaVenta, COALESCE ( [Cantidad], 0 ) )
```


