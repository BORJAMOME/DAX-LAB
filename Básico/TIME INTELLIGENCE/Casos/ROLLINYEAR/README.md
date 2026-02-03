## RollingYer 

Idea principal -> los últimos 12 meses que se van moviendo

- Si estás en enero → mira del febrero anterior a enero
- Si estás en junio → mira de julio del año pasado a junio
- Si estás en noviembre → mira de diciembre del año pasado a noviembre


Una empresa vende productos todos los días pero:

- No se trabajan todos los días
- Hay meses mejores y peores
- El negocio cambia con el tiempo

Sabiendo esto, la empresa se hace la siguiente pregunta: ¿Cuántas unidades estamos vendiendo al día, de media, ahora mismo?
No el año pasado entero, no desde enero. Ahora mismo. 


El error viene al comparar noviembre 2023 contra todo 2022. Esto no refleja cómo va el negocio hoy.

Para eso usamos ROLLINGYEAR -> Voy a mirar siempre los últimos 12 meses completos, contados hacia atrás desde el día que estoy analizando.


**Ejemplo:** si estamos viendo noviembre de 2023, ¿cuáles son los últimos 12 meses desde noviembre de 2023?
La respuesta sería: del 1 de diciembre de 2022 al 30 de noviembre de 2023.


```
UnidadesVendidas/día ROLLINGYEAR = 
CALCULATE (
    [UnidadesVendidas / día],
    DATESINPERIOD (
        'dim_Calendario'[Fecha],
        MAX ( dim_Calendario[Fecha] ),
        -12,
        MONTH
    )
)
```

#### 1.¿En qué fecha estoy?

Noviembre de 2023 por lo que la fecha más grande es ***30/11/2023**

#### 2.¿Qué periodo voy a usar?

Del 01/12/2022 al 30/11/2023

#### 2.Qué hago con esas fechas?
- Suma todas las unidades vendidas en ese periodo
- Cuenta solo los días laborables de ese periodo
- Divide y te devuelve la media de unidades vendidas por día laboral en los últimos 12 meses


Hay que tener en cuenta siempre la siguiente pregunta cuando usamos ROLLINGYEAR:

***Si miro solo lo que ha pasado en los últimos 12 meses, ¿cómo de bien estamos vendiendo ahora mismo?***


![ejemplo](/docs/imagenes/ROLLINGYEAR.png)




