## Formateo dinámico en etiquetas según valores

En los dashboards de BI muchas veces tenemos ventas que varían muchísimo: algunas semanas vendemos 40.000 €, otras decenas de millones. Si usamos el formato automático de Power BI, los números pequeños aparecen como “0,0 M” y eso confunde al usuario.

La medida elige la escala (K o M) según el tamaño de cada valor, con o sin símbolo de euro.

#### Ejemplo

Imagina que eres un analista de ventas de una empresa retail:

- Enero → 2.400.000 €

- Febrero → 1.250.000 €

- Marzo → 42.000 €

Si usamos formato automático:

- Enero → 2,4 M

- Febrero → 1,3 M

- Marzo → 0,0 M (muy confuso)

Con la medida de Labels Format, el resultado será:

- Enero → 2,4 M

- Febrero → 1,3 M

- Marzo → 42,0 K

Así cada etiqueta usa la escala que le corresponde.

#### Formateo dinámico:

```DAX
Ventas_Format =
VAR _Valor = [Ventas]
VAR AbsValor = ABS(_Valor)
VAR EsMoneda = TRUE -- ponemos TRUE si queremos formato con símbolo €, FALSE si solo número
RETURN
SWITCH(
    TRUE(),
    AbsValor >= 1000000,
        FORMAT(
            _Valor / 1000000,
            IF(EsMoneda, "€0.0""M""; (€0.0""M"")", "0.0""M"";(0.0""M"")")
        ),
    AbsValor >= 1000,
        FORMAT(
            _Valor / 1000,
            IF(EsMoneda, "€0.0""K""; (€0.0""K"")", "0.0""K"";(0.0""K"")")
        ),
    FORMAT(
        _Valor,
        IF(EsMoneda, "€#,0; (€#,0)", "#,0;(#,0)")
    )
)
```

#### Formateo dinámico +/-:

```
Δ_Ventas_Format =
VAR _Valor = [Ventas] - [Ventas_Año_Anterior]
VAR AbsValor = ABS(_Valor)
RETURN
SWITCH(
    TRUE(),
    AbsValor >= 1000000,
        FORMAT(
            _Valor / 1000000,
            "+0.0""M"";-0.0""M"""
        ),
    AbsValor >= 1000,
        FORMAT(
            _Valor / 1000,
            "+0.0""K"";-0.0""K"""
        ),
    FORMAT(_Valor, "+#,0;-#,0")
)
```

#### Formateo dinámico con %:

```
Δ_%_Ventas_Format =
VAR _Delta = [Ventas] - [Ventas_Año_Anterior]
VAR _Pct = DIVIDE(_Delta, [Ventas_Año_Anterior], BLANK())
RETURN
    FORMAT ( _Pct, "+0.0%;-0.0%;0.0%" )
```

Un formato de tres secciones (`positivo;negativo;cero`) resuelve el signo sin SWITCH: FORMAT elige la sección según el valor.

#### Ojo: FORMAT devuelve texto

Las tres medidas devuelven texto, así que no sirven como valor de un gráfico; solo como etiqueta. Para que el valor siga siendo numérico uso **cadenas de formato dinámico** (propiedad de la medida → Formato → Dinámico), con la misma lógica:

```DAX
-- Cadena de formato dinámico de [Ventas]
VAR v = ABS ( SELECTEDMEASURE () )
RETURN
    SWITCH (
        TRUE (),
        v >= 1000000, "€#,0.0,,""M""",
        v >= 1000,    "€#,0.0,""K""",
        "€#,0"
    )
```

Cada coma al final del formato divide entre 1.000, así que no hay que dividir el valor a mano. Es lo que uso hoy por defecto; la versión con FORMAT la mantengo para etiquetas que combinan número y texto.
