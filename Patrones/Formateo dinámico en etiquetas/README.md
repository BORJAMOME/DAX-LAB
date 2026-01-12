## Formateo dinámico en etiquetas según valores

En los dashboards de BI muchas veces tenemos ventas que varían muchísimo: algunas semanas vendemos 40.000 €, otras decenas de millones. Si usamos el formato automático de Power BI, los números pequeños aparecen como “0,0 M” y eso confunde al usuario.

Con esta medida, controlamos completamente cómo se muestran los valores, para que siempre se vea el número de forma clara, sin importar si es miles, millones o miles de millones, y podemos aplicar formato de moneda o sin moneda según el tipo de medida.

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

Así se ve todo con sentido para los usuarios.

#### Formateo dinámico:

```
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
SWITCH(
    TRUE(),
    _Pct > 0,
        FORMAT(_Pct, "+0.0%; +0.0%; +0.0%"),
    _Pct < 0,
        FORMAT(_Pct, "-0.0%; -0.0%; -0.0%"),
    ABS(_Pct) = 0,
        FORMAT(_Pct, "0.0%; 0.0%; 0.0%")
)
```
