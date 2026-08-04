A continuación, algunas medidas de color estándar que uso para mis proyectos en Power BI.  
Se pueden crear en bloque en la **vista DAX Query** para facilitar el formateo condicional en tus visualizaciones.

> Consejo: siempre mantén un color transparente en tu modelo; puede ser muy útil para overlays o elementos ocultos.



## Tabla de Colores

| Medida | Color HEX |
|--------|-----------|
| `ColorTransparente`     | `#FFFFFF00` |
| `ColorNegativos`        | `#a81917` |
| `ColorPositivos`        | `#3c8a37` |
| `ColorNeutros`          | `#fbfbfb` |
| `ColorNeutrosMorado`    | `#7c7dff` |
| `ColorNeutrosAzul`      | `#0078d4` |
| `ColorGris1`            | `#d3d3d3` |
| `ColorGris2`            | `#bfbfbf` |
| `ColorGris3`            | `#e5e5e5` |

> Puedes copiar rápidamente el código HEX en cualquier formato condicional o visual de Power BI.



## Código DAX

```DAX
MEASURE 'Tabla Medidas'[ColorTransparente] = "#FFFFFF00"
MEASURE 'Tabla Medidas'[ColorNegativos] = "#a81917"
MEASURE 'Tabla Medidas'[ColorPositivos] = "#3c8a37"
MEASURE 'Tabla Medidas'[ColorNeutros] = "#fbfbfb"
MEASURE 'Tabla Medidas'[ColorNeutrosMorado] = "#7c7dff"
MEASURE 'Tabla Medidas'[ColorNeutrosAzul] = "#0078d4"
MEASURE 'Tabla Medidas'[ColorGris1] = "#d3d3d3"
MEASURE 'Tabla Medidas'[ColorGris2] = "#bfbfbf"
MEASURE 'Tabla Medidas'[ColorGris3] = "#e5e5e5"

```


---
