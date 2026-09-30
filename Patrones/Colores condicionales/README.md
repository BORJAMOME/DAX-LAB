## Colores condicionales según rendimiento de ventas

Supongamos que tenemos:

- Una tabla de hechos: Ventas

- Una tabla de dimensiones: Dim_Tiendas

- Una medida de ventas: [VentasTotales]

Queremos que una tarjeta o KPI cambie de color según si la tienda tiene ventas positivas, negativas o neutras en un periodo seleccionado por el usuario.

#### 1. Medidas de color

Los colores salen de mi [librería de colores](<../Libreria de colores/>). Tenerlos como medidas permite cambiar la paleta en un solo sitio:
```
ColorNegativos = "#a81917"
ColorPositivos = "#3c8a37"
ColorNeutros = "#fbfbfb"
```
#### 2. Medida de color condicional

```
ColorVentas =
SWITCH(
    TRUE(),
    [VentasTotales] < 0, [ColorNegativos],   // negativas → rojo
    [VentasTotales] > 0, [ColorPositivos],   // positivas → verde
    [ColorNeutros]                           // cero o blanco → neutro
)
```

SWITCH ( TRUE (), … ) evalúa las condiciones en orden y devuelve el primer resultado que cumple. El último valor, sin condición, recoge todo lo demás, incluido el blanco. En la primera versión tenía `[VentasTotales] = 0` como última condición y las tiendas sin ventas se quedaban sin color.

En un modelo real guardaría `[VentasTotales]` en una variable para no evaluarla dos veces.

#### 3. Resaltar la selección del slicer

Si quieres que el color dependa de la selección de tienda en un slicer, puedes combinarlo con SELECTEDVALUE:

```

ColorVentasTienda =
IF(
    SELECTEDVALUE(Dim_Tiendas[NombreTienda]) = "Tienda_A",
    [ColorPositivos],   // Si selecciona Tienda_A → verde
    [ColorGris1]        // Si selecciona otra → gris
)
```

Uso práctico:

- Usuario selecciona “Tienda_A” → KPI se pone verde.

- Usuario selecciona otra tienda → KPI se pone gris.

Así puedes resaltar automáticamente elementos importantes según la interacción del usuario con los slicers.

#### 4. Aplicación en Power BI

Crea la medida [ColorVentas] o [ColorVentasTienda].

Ve al visual (tarjeta, matriz, gráfico).

En formato condicional → Color de fondo o de datos → Medida → selecciona tu medida de color.

