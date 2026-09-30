## Orden de los filtros en CALCULATE anidado

Es uno de los casos que más me costó entender, porque la intuición dice justo lo contrario. La regla:

**En un CALCULATE anidado, el externo se evalúa primero y el interno tiene la última palabra.** El CALCULATE de fuera prepara un contexto de filtro; el de dentro parte de ese contexto y puede sobrescribirlo.

Lo compruebo con dos medidas que usan los mismos ingredientes (Francia y ALL sobre País) en orden inverso.

![ejemplo](/docs/imagenes/OrdenFiltros.png)

### ALL País → devuelve solo Francia

```DAX
ALL País =
CALCULATE (
    CALCULATE (
        [Cantidad],
        KEEPFILTERS ( dim_Clientes[País] = "Francia" )
    ),
    ALL ( dim_Clientes[País] )
)
```

1. El CALCULATE externo quita el filtro de país de la fila (Alemania, Australia…).
2. El interno parte de "todos los países" y aplica Francia con KEEPFILTERS: la intersección de *todos* con *Francia* es Francia.

Resultado: **54.965 en todas las filas**, que es la cantidad de Francia.

### País ALL → devuelve el total

```DAX
País ALL =
CALCULATE (
    CALCULATE (
        [Cantidad],
        ALL ( dim_Clientes[País] )
    ),
    KEEPFILTERS ( dim_Clientes[País] = "Francia" )
)
```

1. El externo intersecta el país de la fila con Francia (en la fila Alemania, eso queda vacío).
2. El interno ejecuta ALL sobre País y borra todo lo anterior, incluido Francia.

Resultado: **610.660 en todas las filas**, el total de todos los países.

### Lo que me llevo

- Si una medida da un número que no esperas, lee los CALCULATE de fuera hacia dentro: el último filtro sobre una columna es el del CALCULATE más interno.
- KEEPFILTERS no "protege" un filtro de lo que venga después; solo cambia cómo se combina con el contexto que ya existe (intersección en lugar de sobrescritura).
- En la práctica evito anidar CALCULATE en la misma medida. Cuando pasa, suele ser porque una medida llama a otra que ya lleva su propio CALCULATE, y ahí es donde aparecen las sorpresas.
