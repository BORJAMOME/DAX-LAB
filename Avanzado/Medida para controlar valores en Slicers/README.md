## Medida para controlar valores en Slicers según lógica de la visualización

Imagina que trabajamos en un departamento de marketing y queremos analizar campañas de email.
Tenemos una matriz con el siguiente formato:

| Campaña   | Aperturas | Emails enviados |
| --------- | --------- | --------------- |
| Campaña A | 500       | 1,000           |
| Campaña B | BLANK     | 800             |
| Campaña C | 750       | 1,200           |



#### Problema:
Queremos ver solo las campañas que han tenido aperturas, porque no nos interesa mostrar campañas sin datos de apertura.

Si aplicamos un filtro visual “Aperturas IS NOT BLANK” en la matriz, la tabla quedaría así:

| Campaña   | Aperturas | Emails enviados |
| --------- | --------- | --------------- |
| Campaña A | 500       | 1,000           |
| Campaña C | 750       | 1,200           |

Perfecto en la matriz. Pero si tenemos un slicer de Campaña para seleccionar campañas o categorías de campañas, todavía veremos Campaña B, aunque no tenga aperturas. Esto puede confundir al usuario.


#### Solución: medida para filtrar slicers

Creamos una medida que refleje si hay aperturas:
```
Campaña con Aperturas =
IF(
    NOT(ISBLANK([Aperturas])),
    1,
    BLANK()
)
```

Luego aplicamos esta medida como filtro visual en el slicer, con la condición “es distinto de BLANK”.


##### Resultado:

Ahora el slicer solo muestra Campaña A y Campaña C.

Campaña B desaparece automáticamente del slicer, sincronizando la selección con los datos visibles en la matriz.
