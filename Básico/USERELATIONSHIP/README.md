## USERELATIONSHIP

USERELATIONSHIP te permite activar temporalmente una relación inactiva entre dos tablas para un cálculo específico, sin tocar el modelo original.

Imagínate que tienes dos caminos posibles entre tu tabla de ventas y tu tabla de fechas:

- Camino 1: Fecha de venta → normalmente activo.

- Camino 2: Fecha de envío → normalmente inactivo.

Quieres calcular ventas por fecha de envío, pero no quieres cambiar la relación que ya usas para otras medidas.

USERELATIONSHIP es como un desvío temporal: activas solo ese camino para esta medida, y luego todo vuelve a la normalidad.

#### Ejemplo:

```
Ventas por Fecha de Envío =
CALCULATE(
    [Cantidad],
    USERELATIONSHIP(Ventas[FechaEnvio], Dim_Fecha[Fecha])
)
```

- Si usas la relación activa normal, se filtra por FechaVenta.

- Con USERELATIONSHIP, esa medida ignora FechaVenta y usa FechaEnvio solo aquí.


| ID | Cantidad | FechaVenta | FechaEnvio |
| -- | -------- | ---------- | ---------- |
| 1  | 10       | 01/01/2026 | 03/01/2026 |
| 2  | 5        | 01/01/2026 | 04/01/2026 |


Tabla Dim_Fecha con todas las fechas del año.

- Medida normal (relación activa por FechaVenta): suma las ventas según la fecha de venta.

- Medida con USERELATIONSHIP: suma las ventas según la fecha de envío.

Resultado: puedes ver cuántas unidades fueron enviadas cada día, sin tocar tu cálculo habitual de ventas por fecha de venta.



