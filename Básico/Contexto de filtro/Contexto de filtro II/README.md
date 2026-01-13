## Contexto de filtro II

Las cuatro medidas buscan “Color = Rojo”, pero cada una trata el contexto de filtro de forma distinta.
- La clave NO es el rojo.
- La clave es qué filtros se respetan, cuáles se ignoran y cuáles son redundantes.


![filtro](../../docs/imagenes/Contexto_Filtro_II.png) 

#### FILTER_Color_Rojo
De lo que ya veo, dame solo lo rojo

Técnica
  - dim_Producto ya viene filtrado por el contexto actual
  - FILTER solo reduce más
  - NO rompe filtros existentes

```
FILTER_Color_Rojo = CALCULATE(
			[Cantidad],
			FILTER(
				dim_Producto,
				dim_Producto[Color] = "Rojo"
			)
		)
```


#### FILTER_ALL_Color_Rojo
Ignora cualquier color que me estén diciendo y fuerza rojo

Técnica
  - ALL(dim_Producto[Color]) borra el contexto de color
  - Ignora:
      - Fila del visual
      - Segmentadores por color
  - Después aplica Rojo
    
Resultado:
  - Siempre devuelve el mismo valor por fila de color.


```
FILTER_ALL_Color_Rojo = CALCULATE(
			[Cantidad],
			FILTER(
				ALL(dim_Producto[Color]),
				dim_Producto[Color] = "Rojo"
			)
		)
```

#### VALUES_Color_Rojo
Cuenta rojo, pero solo si existe en este contexto

Técnica
  - VALUES(Color) devuelve:
      - "Rojo" si existe
      - BLANK si no existe

Resultado:
  - Igual que la medida simple
  - No aporta nada adicional

```
VALUES_Color_Rojo = CALCULATE(
			[Cantidad],
			dim_Producto[Color] = "Rojo",
			VALUES(dim_Producto[Color])
		)
```

#### DISTINCT_Color_Rojo
Cuenta rojo, pero primero quita colores repetidos

Técnica
  - DISTINCT(dim_Producto[Color]) NO hace nada útil aquí.
    
¿Por qué?
  - Ya estás filtrando a "Rojo"
  - Solo existe un valor posible
  - DISTINCT sobre una columna filtrada a un único valor es redundante

```
DISTINCT_Color_Rojo = CALCULATE(
			[Cantidad],
			dim_Producto[Color] = "Rojo",
			DISTINCT(dim_Producto[Color])
		)
```

