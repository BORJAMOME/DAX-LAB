## Resolución de problemas de granularidad en métricas de negocio

Imagina una empresa de suscripciones SaaS.

En la tabla de ventas guardamos demasiado detalle:

- Cliente

- Producto

- Tipo de plan

- Versión

- Extras contratados

Esto está bien para operaciones, pero el negocio no piensa así.

Idea principal: “Una oportunidad comercial es cada combinación Cliente + Producto, da igual el plan o la versión.”

Aquí aparece el problema típico:

- Los datos están demasiado detallados

- El concepto de negocio es más simple

#### Ejemplo de datos
| Cliente | Producto | Plan   |
| ------- | -------- | ------ |
| 1       | CRM      | Básico |
| 1       | CRM      | Pro    |
| 1       | ERP      | Pro    |
| 2       | CRM      | Pro    |
| 2       | BI       | Básico |
| 2       | BI       | Pro    |
| 3       | CRM      | Básico |
| 3       | ERP      | Pro    |

Si contamos filas:
→ salen 8, pero eso no responde al negocio.

Una oportunidad es:

- Cliente + Producto

Entonces:

- Cliente 1 → CRM, ERP → 2 oportunidades

- Cliente 2 → CRM, BI → 2 oportunidades

- Cliente 3 → CRM, ERP → 2 oportunidades

Total correcto: 6 oportunidades

### Cómo resolvemos esto con DAX

Creamos una tabla virtual que:

1. Elimine el detalle innecesario

2. Se quede solo con la combinación que importa al negocio

3. Cuente esas combinaciones

```
Oportunidades =
// SUMMARIZE crea una tabla virtual con: Cliente y Producto
VAR TablaVirtual =
    SUMMARIZE(
        Ventas,
        Ventas[Cliente],
        Ventas[Producto]
    )
RETURN
// COUNTROWS cuenta solo las oportunidades
COUNTROWS(TablaVirtual)

```





