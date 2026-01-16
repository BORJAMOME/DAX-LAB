# TREATAS 

TREATAS crea una relación virtual, temporal e invisible entre una tabla “imaginada” y otra real.
No modifica el modelo, solo el contexto del cálculo.

“Oye Power BI, finge que esto pertenece a esa columna… solo durante esta medida.”


#### Medida 1 — Años 2024–2025
```
Años 2024-2025 TREATAS = 
CALCULATE(
    [Cantidad], 
    TREATAS(
        {2024, 2025},
        Calendario_II[Año]
    )
)
```
- {2024, 2025} → tabla virtual (no existe en el modelo)

- Calendario_II[Año] → columna real

- TREATAS dice: “Trata estos valores como si vinieran del calendario”

#### Medida 2 — Fin 2024 / Inicio 2025

```
Fin 2024 - Inicio 2025 TREATAS = 
CALCULATE(
    [Cantidad], 
    TREATAS(
        {(2024, 12), (2025, 1)},
        Calendario_II[Año],
        Calendario_II[Num Mes]
    )
)
```

- TREATAS dice: Filtra solo: Diciembre 2024 y Enero 2025

![ejemplo](/docs/imagenes/TREATAS.png)

---

## TREATAS + tablas desconectadas

Una tabla desconectada captura la intención del usuario.
TREATAS traduce esa intención en un filtro real sobre el modelo.

#### Ejemplo

Selector de años “especiales” --> Tabla desconectada

```
Selector_Años =
DATATABLE(
    "Año", INTEGER,
    {
        {2023},
        {2024},
        {2025}
    }
)
```

#### El puente: TREATAS
Ahora conectamos intención → datos reales.

```
Cantidad (Selector Años) =
CALCULATE(
    [Cantidad],
    TREATAS(
        VALUES(Selector_Años[Año]),
        Calendario_II[Año]
    )
)
```
- VALUES recoge lo seleccionado en el slicer

- TREATAS lo aplica al calendario

- El calendario filtra ventas


### Ejemplo: períodos no continuos

Fin de año fiscal + arranque

```
Periodo Fiscal Especial =
DATATABLE(
    "Año", INTEGER,
    "Mes", INTEGER,
    {
        {2024, 12},
        {2025, 1}
    }
)
```

```
Cantidad Periodo Fiscal =
CALCULATE(
    [Cantidad],
    TREATAS(
        Periodo Fiscal Especial,
        Calendario_II[Año],
        Calendario_II[Num Mes]
    )
)
```
- Tú controlas exactamente qué meses entran

- Cero dependencias del calendario visual


### Ejemplo: períodos especiales

Tabla desconectada:

```
Selector_Periodo =
DATATABLE(
    "Tipo", STRING,
    {
        {"Último Año"},
        {"Últimos 2 Años"},
        {"Periodo Fiscal"}
    }
)
```

Medida:

```
Cantidad Dinámica =
VAR Seleccion = SELECTEDVALUE(Selector_Periodo[Tipo])
RETURN
SWITCH(
    Seleccion,
    "Último Año",
        CALCULATE(
            [Cantidad],
            TREATAS({YEAR(TODAY())-1}, Calendario_II[Año])
        ),
    "Últimos 2 Años",
        CALCULATE(
            [Cantidad],
            TREATAS(
                {YEAR(TODAY())-1, YEAR(TODAY())-2},
                Calendario_II[Año]
            )
        ),
    "Periodo Fiscal",
        [Cantidad Periodo Fiscal],
    [Cantidad]
)
```
