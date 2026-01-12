### Evitar que un slicer muestre opciones sin datos

Imagina que trabajas en una empresa de logística que gestiona envíos de distintos transportistas a distintos destinos.

- Tabla de hechos: fact_envios (cada envío con transportista, destino y cantidad de paquetes)

- Dimensiones: dim_transportistas y dim_destinos

Problema típico:

- En un informe, el usuario selecciona un transportista, por ejemplo “Transportista X”.

- Hay un slicer de destinos donde aparecen todos los destinos posibles.

- Algunos destinos como “Destino Z” no tienen envíos con ese transportista.

- Si el usuario selecciona “Destino Z”, los gráficos muestran blancos, lo que confunde y frustra al usuario.


##### Medida para resolverlo

```
Destinos Válidos =
COUNTROWS(RELATEDTABLE(fact_envios))
```

#### Cómo funciona:

1. RELATEDTABLE(fact_envios) devuelve todas las filas de la tabla de hechos que están relacionadas con el destino en ese momento del slicer.

2. COUNTROWS cuenta cuántos envíos hay.

Se coloca esta medida como filtro en el slicer de destinos, configurando:
Destinos Válidos >= 1

---

#### Múltiples tablas de hechos

Si además quieres incluir devoluciones (fact_devoluciones), la medida se amplía: 

```
Destinos Válidos =
COUNTROWS(RELATEDTABLE(fact_envios)) + COUNTROWS(RELATEDTABLE(fact_devoluciones))
```
