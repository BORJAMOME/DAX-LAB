# Proyecto: Control de Presupuestos

Este proyecto es como tener una lupa sobre tus presupuestos: te dice de un vistazo qué proyectos tiran del carro y cuáles están de relleno.

![Widget](../../docs/imagenes/Widget.png)

### Qué hace

#### 1. Gráfico de barras que habla por sí solo

  - Muestra las ventas totales por departamento.

  - Puedes elegir si quieres ver el Top 1, Top 3 o Top 5 de proyectos en un segmentador.

  - Los proyectos que están en ese Top se iluminan en morado intenso; el resto queda en un morado más suave. Así, con un golpe de vista sabes quién manda.

#### 2. Tarjetas que resumen la historia

  - Una tarjeta te da los datos globales del Top que hayas seleccionado.

  - Otra tarjeta te recuerda el total general, para que veas la proporción que representa el Top sobre el conjunto.

  - Nada de tablas kilométricas ni cifras confusas: todo limpio y directo.

#### 3. Narrativa automática que no te deja pensar demasiado

  - Según cómo estén repartidos los presupuestos, la herramienta te suelta un mensaje claro:

    - Si un proyecto acapara demasiado, te avisa de riesgo de dependencia.

    - Si todo está muy repartido, te dice que hay alta dispersión.

    - Y si todo está equilibrado, confirma que los proyectos principales aportan un peso razonable.


### Cómo está hecho:

#### 1. Preparar la base de datos y medidas principales

Primero, necesitamos los datos de los proyectos y sus presupuestos. Aquí se creó la medida más básica:

Total Presupuesto
```
Total Presupuesto = SUM(Proyectos[Presupuesto (€)])
```

#### 2.  Crear el ranking de proyectos

Para saber quiénes son los más importantes, necesitamos ordenarlos por presupuesto:

```
Ranking Presupuesto

Ranking Presupuesto = 
RANKX(
    ALL(Proyectos[Proyecto]),
    [Total Presupuesto],
    ,
    DESC
)
```

#### 3. Tabla desconectada para el segmentador

Para permitir que el usuario elija Top 1, 3 o 5, creamos una tabla desconectada:

```
Ranking Presupuestos = GENERATESERIES(1, 5, 1)
```

  - Genera una serie de números del 1 al 5.

  - Esta tabla no tiene relación con la tabla de proyectos, por eso se llama desconectada.


En el segmentador también puedes mostrar un nombre más amigable:

```
Nombre Parametro = "Top " & 'Ranking Presupuestos'[Ranking Presupuestos]
```

#### 4. Colorear las barras según el Top N seleccionado

Ahora queremos que el Top N se vea más destacado en el gráfico de barras.

```
Color Barra = 
VAR SeleccionarTopN = SELECTEDVALUE('Ranking Presupuestos'[Ranking Presupuestos])
RETURN
IF([Ranking Presupuesto] <= SeleccionarTopN, "#5b5cff99", "#5b5cff20")
```

```
Texto Color funciona de forma similar, pero para el color del texto sobre las barras:

Texto Color = 
VAR SeleccionarTopN = SELECTEDVALUE('Ranking Presupuestos'[Ranking Presupuestos])
RETURN
IF([Ranking Presupuesto] <= SeleccionarTopN, "#5b5cff", "#b5b5b5")
```

#### 5. Crear la narrativa automática del Top N

Esta es la parte que hace que el widget hable por sí mismo: explica la concentración de los proyectos de forma clara.

```
Narrativa TopN = 
VAR SeleccionarTopN = INT( SELECTEDVALUE('Ranking Presupuestos'[Ranking Presupuestos], 3) )
VAR TotalPresupuesto = [Total Presupuesto]
VAR SinDatos = ISBLANK(TotalPresupuesto) || TotalPresupuesto = 0

VAR TableTopN =
    TOPN(
        SeleccionarTopN,
        ALLSELECTED('Proyectos'[Proyecto]),
        [Total Presupuesto],
        DESC
    )

VAR TopNSales = SUMX(TableTopN, [Total Presupuesto])
VAR SalesPercentage = DIVIDE(TopNSales, TotalPresupuesto, BLANK())

VAR RangoInferior = 
    SWITCH( SeleccionarTopN,
        1, 0.25,
        3, 0.55,
        5, 0.70
    )

VAR RangoSuperior =
    SWITCH( SeleccionarTopN,
        1, 0.40,
        3, 0.70,
        5, 0.85
    )

VAR Diagnostico =
    SWITCH(
        TRUE(),
        SinDatos, "No hay datos suficientes.",
        ISBLANK(RangoInferior) || ISBLANK(RangoSuperior), "Rango no definido.",
        SalesPercentage < RangoInferior, 
            SWITCH( SeleccionarTopN,
                1, "El proyecto principal tiene una aportación reducida. Alta dispersión.",
                3, "Los 3 mayores no alcanzan una masa crítica suficiente.",
                5, "Los 5 principales muestran baja representatividad."
            ),
        SalesPercentage > RangoSuperior,
            SWITCH( SeleccionarTopN,
                1, "Un único proyecto concentra un peso excesivo. Riesgo elevado.",
                3, "Alta concentración en pocos proyectos. Dependencia significativa.",
                5, "Concentración excesiva en los 5 principales."
            ),
        SWITCH( SeleccionarTopN,
            1, "El proyecto principal mantiene un nivel de concentración adecuado.",
            3, "Los 3 mayores representan una concentración equilibrada.",
            5, "Los 5 principales aportan un peso representativo y razonable."
        )
    )

RETURN
IF(
    SinDatos,
    "Top " & SeleccionarTopN & ": Datos insuficientes",
    "Top " & SeleccionarTopN & ": " &
    FORMAT( TopNSales / 1000, "#,0" ) & " mil €" &
    " (" & FORMAT(SalesPercentage, "0.0%") & " del total)" &
    UNICHAR(10) & Diagnostico
)
```

- TOPN selecciona los proyectos más grandes según el Top N elegido.

- SalesPercentage calcula qué porcentaje del total representan.

- RangoInferior/RangoSuperior define si la concentración es baja, equilibrada o alta.


Esto evita tener que interpretar números complejos: el widget ya te dice qué pasa.



#### Combinar todo en el gráfico y las tarjetas

- El gráfico de barras usa Color Barra y Texto Color para diferenciar los proyectos según el Top N.

- Una tarjeta dinámica muestra los totales del Top N seleccionado (usando Narrativa TopN).

- Otra tarjeta muestra el total general para comparar rápidamente.

Así, en un vistazo, el usuario sabe:

- Qué proyectos destacan

- Qué proporción del total representan

- Si la distribución es equilibrada o riesgosa



--- 

📂 Se incluye el archivo PBIX para que puedas probarlo, tocarlo y verlo en acción.


