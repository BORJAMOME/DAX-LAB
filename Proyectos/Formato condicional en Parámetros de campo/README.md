## Formato condicional en parámetros de campo

Truco muy útil para Power BI que no es nada evidente y que encontré en el blog de **Javier San Juan – reportingfinanciero.com.**
Si trabajas con parámetros de campo, esto te va a ahorrar tiempo y frustración.

#### Problema
Se puede aplicar el formato condicional a las columnas o medidas, pero cuando usamos parámetros de campo en una tabla o matriz, el formato condicional no se puede aplicar directamente a las columnas del parámetro.

#### Solución

1️⃣ Coloca el parámetro de campo en la zona de Valores de la tabla o matriz.

2️⃣ Añade también la medida original sobre la que quieres aplicar el formato condicional (por ejemplo, Ventas, Unidades, etc.).

3️⃣ Aplica el formato condicional (barras de datos, color de fondo, etc.) sobre esa medida, como harías normalmente.

4️⃣ Comprueba que el formato se visualiza correctamente en la matriz.

5️⃣ Elimina la medida que añadiste en el paso 2️⃣.

El formato condicional se queda aplicado dentro del parámetro de campo, aunque la medida ya no esté visible.


![ejemplo](/docs/imagenes/ParametroFC.png)
