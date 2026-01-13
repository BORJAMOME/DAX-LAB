## Conjuntos admitidos y excluidos

![ejemplo](/docs/imagenes/ConjuntoExcluidoAdmitido.png)

En DAX podemos decidir qué datos entran y cuáles se quedan fuera usando conjuntos. 

Con CALCULATE le decimos a una medida:
  - “Calcula esto… pero solo con estos países”
  o
  -  “Calcula esto… excepto con estos países”.

Imagina que tienes una urna con bolas.
Cada bola es una venta y cada bola tiene una etiqueta: España, Canadá, Australia…

Ahora te dicen:

  - “Cuéntame todas las bolas” → fácil.

  - “Cuéntame todas menos las de Canadá y Australia” → exclusión.

  - “Cuéntame solo las de Canadá y Australia” → inclusión.

DAX funciona exactamente así.
No cambia los datos, elige qué bolas mirar.

#### Medida base

```
Cantidad = SUM(Ventas[Cantidades])
```
- Esta medida no filtra nada.

#### Medida base

```
Europa = 
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

  - NOT dice: “todo menos esto”


#### Conjunto ADMITIDO (solo Canadá y Australia)
```
o Europa = 
CALCULATE(
    [Cantidad],
    dim_Clientes[País] IN {
        "Canada",
        "Australia"
    }
)
```

- Aquí no hay NOT, así que: Calcula la cantidad solo para Canadá y Australia.
