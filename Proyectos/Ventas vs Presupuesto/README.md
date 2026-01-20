## Ventas vs Presupuesto

Comparación flexible entre datos reales y presupuestados

Este caso muestra cómo comparar Ventas Reales vs Presupuesto cuando no comparten la misma granularidad ni las mismas dimensiones, algo muy habitual en modelos reales de Power BI.

### Problema

Tenemos dos mundos distintos:

**Ventas reales (REAL)**

- Detalle por fecha de factura

- Nivel transaccional

**Presupuesto (PRESU20)**

- Agregado por mes y ciudad

- Sin fecha diaria


Adaptar el contexto de las ventas al contexto del presupuesto, sin forzar relaciones físicas en el modelo. -> Aquí entra en juego TREATAS.


![ejemplo](/docs/imagenes/VentasvsPresupuesto.png)

### Medidas

**Ventas reales**
```
Ventas según fecha factura = 
SUM(REAL[IMPORTE])
```

**Presupuesto ajustado al contexto del informe**

```
Presupuesto Ajustado = 
CALCULATE(
    SUM(PRESU20[IMPORTE]),
    TREATAS(
        VALUES(CALENDARIO[Mes Num]),
        PRESU20[MES]
    ),
    TREATAS(
        VALUES(ID_CENTRO[CIUDAD]),
        PRESU20[CIUDAD]
    )
)
```

- PRESU20 no está relacionado directamente con el calendario ni con centros

- TREATAS crea relaciones virtuales

- Se fuerza el cruce de:

  - Mes del calendario → Mes del presupuesto

  - Ciudad del centro → Ciudad del presupuesto



**Diferencia entre Ventas y Presupuesto**

```
Diferencia = 
[Ventas según fecha factura] - [Presupuesto Ajustado]
```

**Gestión de blancos y casos límite**

```
DELTA_VENTAS_vs_PRESUP = 
IF (
    ISBLANK ( [Ventas según fecha factura] ) && ISBLANK ( [Presupuesto Ajustado] ),
    BLANK (),
    IF (
        NOT ( ISBLANK ( [Ventas según fecha factura] ) ) && ISBLANK ( [Presupuesto Ajustado] ),
        [Ventas según fecha factura],
        [Ventas según fecha factura] - [Presupuesto Ajustado]
    )
)
```

- Evita mostrar diferencias cuando no hay datos

- Permite distinguir:

  - Ventas sin presupuesto
  
  - Presupuesto sin ventas
  
  - Comparación real


**Separación visual de positivos y negativos**

Presupuesto

```
PRESUP_DELTA_NEG =
IF ( [DELTA_VENTAS_vs_PRESUP] < 0, [Presupuesto Ajustado], BLANK() )

PRESUP_DELTA_POS =
IF ( [DELTA_VENTAS_vs_PRESUP] >= 0, [Presupuesto Ajustado], BLANK() )
```


Ventas

```
VENTAS_DELTA_NEG =
IF ( [DELTA_VENTAS_vs_PRESUP] < 0, [Ventas según fecha factura], BLANK() )

VENTAS_DELTA_POS =
IF ( [DELTA_VENTAS_vs_PRESUP] >= 0, [Ventas según fecha factura], BLANK() )
```

Esto permite:

- Mostrar barras separadas

- Colorear por encima / por debajo del presupuesto

- Construir gráficos claros sin lógica visual compleja



📂 Se incluye el archivo PBIX para que puedas probarlo, tocarlo y verlo en acción.



