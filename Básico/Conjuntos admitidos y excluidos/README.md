## Conjuntos admitidos y excluidos

![ejemplo](/docs/imagenes/ConjuntoExcluidoAdmitido.png)

En DAX podemos decidir qué datos entran y cuáles se quedan fuera usando conjuntos. 

Con CALCULATE le decimos a una medida:
  - “Calcula esto… pero solo con estos países”
  o
  -  “Calcula esto… excepto con estos países”.

Con IN defino un conjunto de valores; con NOT delante, todo lo que queda fuera de él. CALCULATE no cambia los datos, solo elige qué filas mira.

#### Medida base

```DAX
Cantidad = SUM ( Ventas[Cantidades] )
```
- Esta medida no filtra nada.

#### Conjunto EXCLUIDO (todo menos Canadá y Australia)

```DAX
Sin Canadá ni Australia = 
CALCULATE(
    [Cantidad],
    NOT dim_Clientes[País] IN {
        "Canada",
        "Australia"
    }
)
```
¿Qué está pasando aquí?

  - CALCULATE cambia el contexto de filtro

  - IN { ... } define un conjunto

  - NOT invierte la condición: todo menos esto

El nombre de la medida importa. En la primera versión la llamé "Europa", pero sin Canadá ni Australia siguen quedando Estados Unidos y otros países. Si el negocio quiere Europa, lo correcto es filtrar por una columna de continente, no por exclusión.


#### Conjunto ADMITIDO (solo Canadá y Australia)
```DAX
Canadá y Australia = 
CALCULATE(
    [Cantidad],
    dim_Clientes[País] IN {
        "Canada",
        "Australia"
    }
)
```

- Aquí no hay NOT, así que: Calcula la cantidad solo para Canadá y Australia.
