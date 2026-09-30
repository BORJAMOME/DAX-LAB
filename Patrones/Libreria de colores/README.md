## Librería de colores

Las medidas de color que uso en todos mis informes. Las creo de golpe desde la **vista de consultas DAX** (pegando el bloque de abajo y pulsando "Actualizar modelo") y luego las referencio en el formato condicional.

Siempre incluyo un transparente: sirve para ocultar etiquetas, fondos de botones o barras que no quiero que se vean sin tocar el visual.



### Paleta

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




### Código para la vista de consultas DAX

```DAX
DEFINE
MEASURE 'Tabla Medidas'[ColorTransparente] = "#FFFFFF00"
MEASURE 'Tabla Medidas'[ColorNegativos] = "#a81917"
MEASURE 'Tabla Medidas'[ColorPositivos] = "#3c8a37"
MEASURE 'Tabla Medidas'[ColorNeutros] = "#fbfbfb"
MEASURE 'Tabla Medidas'[ColorNeutrosMorado] = "#7c7dff"
MEASURE 'Tabla Medidas'[ColorNeutrosAzul] = "#0078d4"
MEASURE 'Tabla Medidas'[ColorGris1] = "#d3d3d3"
MEASURE 'Tabla Medidas'[ColorGris2] = "#bfbfbf"
MEASURE 'Tabla Medidas'[ColorGris3] = "#e5e5e5"

EVALUATE
    { [ColorPositivos] }

```


---
