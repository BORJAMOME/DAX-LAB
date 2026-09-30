## TREATAS

TREATAS toma una tabla de valores y la aplica como filtro sobre columnas del modelo, como si esos valores vinieran de ellas. No toca el modelo: el filtro solo existe dentro de la medida.

Lo uso en tres situaciones: filtrar por una lista fija de valores, conectar una tabla desconectada (un selector) con el modelo, y cruzar tablas que no tienen relación física, como un presupuesto con otra granularidad (ver [Ventas vs Presupuesto](<../../Proyectos/Ventas vs Presupuesto/>)).


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

- TREATAS aplica esos valores como filtro sobre la columna del calendario

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

Una tabla desconectada guarda lo que el usuario elige en un slicer, pero no filtra nada por sí sola. TREATAS convierte esa selección en un filtro sobre el modelo.

#### Ejemplo

Tabla desconectada con los años que se ofrecen en el slicer:

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

#### La medida que las conecta

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
- VALUES recoge lo seleccionado en el slicer.
- TREATAS lo aplica como filtro sobre `Calendario_II[Año]`.
- El calendario filtra ventas por la relación normal.

Si no hay nada seleccionado, VALUES devuelve los tres años de la tabla y la medida suma 2023-2025.


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
        'Periodo Fiscal Especial',
        Calendario_II[Año],
        Calendario_II[Num Mes]
    )
)
```
- La tabla define exactamente qué combinaciones año-mes entran, aunque no sean consecutivas.

- Para añadir o quitar un mes se edita la tabla, no la medida.


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

`YEAR ( TODAY () ) - 1` cambia con la fecha de refresco: el informe de enero y el de diciembre muestran "Último año" distinto sin tocar nada.

### Cuándo no uso TREATAS

Si existe una relación física entre las tablas, la uso. TREATAS es más lento que una relación y el filtro no es visible en el modelo, así que quien herede el informe tiene que leer la medida para entender de dónde sale el número.
