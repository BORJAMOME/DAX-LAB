### Filtrar por rango usando un slicer

Imagina que tienes un dashboard de ventas y una matriz de productos, donde los productos están agrupados en categorías.
Tu objetivo: que el usuario pueda ver solo los productos cuya venta total esté dentro de un rango seleccionado con un slicer, sin tocar filtros manuales de Power BI.

Por ejemplo, el usuario quiere ver solo los productos que han vendido entre 500 y 1.000 unidades.

#### Paso 1: Crear tablas desconectadas para el slicer

- Tabla MinVenta: con todos los valores posibles de venta mínima

- Tabla MaxVenta: con todos los valores posibles de venta máxima

Estas tablas no están relacionadas con tu modelo. Sirven para que el usuario pueda seleccionar el rango deseado.
