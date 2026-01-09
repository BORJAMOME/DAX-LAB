## Colores condicionales según rendimiento de ventas

Supongamos que tenemos:

- Una tabla de hechos: Ventas

- Una tabla de dimensiones: Dim_Tiendas

- Una medida de ventas: [VentasTotales]

Queremos que una tarjeta o KPI cambie de color según si la tienda tiene ventas positivas, negativas o neutras en un periodo seleccionado por el usuario.

#### 1️⃣ Medidas de color

Usamos los colores que definiste:
```
ColorNegativos = "#a81917"
ColorPositivos = "#3c8a37"
ColorNeutros = "#fbfbfb"
```
#### 2️⃣ Medida de color condicional dinámica

```
ColorVentas =
SWITCH(
    TRUE(),
    [VentasTotales] < 0, [ColorNegativos],   // Ventas negativas → rojo
    [VentasTotales] > 0, [ColorPositivos],   // Ventas positivas → verde
    [VentasTotales] = 0, [ColorNeutros]      // Ventas neutras → gris
)
```

Explicación:

SWITCH(TRUE(), ...) → Permite evaluar múltiples condiciones de forma ordenada.

Cada condición compara [VentasTotales] y devuelve un color según corresponda.

Esta medida se puede usar en formato condicional de tarjetas, matrices o gráficos.

#### 3️⃣ Ejemplo con slicer

Si quieres que el color dependa de la selección de tienda en un slicer, puedes combinarlo con SELECTEDVALUE:

```

ColorVentasTienda =
IF(
    SELECTEDVALUE(Dim_Tiendas[NombreTienda]) = "Tienda_A",
    [ColorPositivos],   // Si selecciona Tienda_A → verde
    [ColorGris]         // Si selecciona otra → gris
)
```

Uso práctico:

- Usuario selecciona “Tienda_A” → KPI se pone verde.

- Usuario selecciona otra tienda → KPI se pone gris.

Así puedes resaltar automáticamente elementos importantes según la interacción del usuario con los slicers.

#### 4️⃣ Aplicación en Power BI

Crea la medida [ColorVentas] o [ColorVentasTienda].

Ve al visual (tarjeta, matriz, gráfico).

En formato condicional → Color de fondo o de datos → Medida → selecciona tu medida de color.

