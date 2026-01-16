## HASONEVALUE

HASONEVALUE sirve para saber si hay un único valor activo en una columna.
Es ideal para detectar nivel de agregación y cambiar la lógica del cálculo o del mensaje.



![ejemplo](/docs/imagenes/HASONEVALUE.png)




```
Un valor Año = 
HASONEVALUE(Calendario_II[Año]) 
```

```
Un valor Trimestre = 
HASONEVALUE(Calendario_II[Trimestre]) 
```

```
Nivel Actual =
SWITCH (
    TRUE (),
    HASONEVALUE ( Calendario_II[Día] ), "IVA 21% diario",
    HASONEVALUE ( Calendario_II[Mes] ), "IVA 10% mensual",
    HASONEVALUE ( Calendario_II[Trimestre] ), "IVA 4% trimestral",
    HASONEVALUE ( Calendario_II[Año] ), "Total / Año sin IVA",
    "Total General"
)
```

```
Cantidad con IVA = 
SWITCH(
    TRUE(),
    ISINSCOPE(Calendario_II[Día]), [Cantidad] * 1.21,        // IVA 21% diario
    ISINSCOPE(Calendario_II[Mes]), [Cantidad] * 1.10,        // IVA 10% mensual
    ISINSCOPE(Calendario_II[Trimestre]), [Cantidad] * 1.04,  // IVA 4% trimestral
    [Cantidad]                                              // Total / Año sin IVA
)
```

