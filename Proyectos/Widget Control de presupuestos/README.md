### Proyecto: Control de Presupuestos

Este proyecto es como tener una lupa sobre tus presupuestos: te dice de un vistazo qué proyectos tiran del carro y cuáles están de relleno.

#### Qué hace

1. Gráfico de barras que habla por sí solo

  - Muestra las ventas totales por departamento.

  - Puedes elegir si quieres ver el Top 1, Top 3 o Top 5 de proyectos en un segmentador.

  - Los proyectos que están en ese Top se iluminan en morado intenso; el resto queda en un morado más suave. Así, con un golpe de vista sabes quién manda.

2. Tarjetas que resumen la historia

  - Una tarjeta te da los datos globales del Top que hayas seleccionado.

  - Otra tarjeta te recuerda el total general, para que veas la proporción que representa el Top sobre el conjunto.

  - Nada de tablas kilométricas ni cifras confusas: todo limpio y directo.

3. Narrativa automática que no te deja pensar demasiado

  - Según cómo estén repartidos los presupuestos, la herramienta te suelta un mensaje claro:

    - Si un proyecto acapara demasiado, te avisa de riesgo de dependencia.

    - Si todo está muy repartido, te dice que hay alta dispersión.

    - Y si todo está equilibrado, confirma que los proyectos principales aportan un peso razonable.


#### Cómo está hecho (sin entrar en tecnicismos aburridos)

  - Color Barra: decide qué barras se iluminan según tu Top N.

  - Narrativa TopN: genera el texto que te explica la concentración de los proyectos.

  - Ranking Presupuesto: calcula quién está arriba y quién abajo.

  - Texto Color: cambia el color del texto según la posición en el ranking.

  - Total Presupuesto: suma todos los presupuestos de los proyectos.

Y todo esto funciona con un segmentador desconectado, para que cambiar entre Top 1, 3 o 5 sea instantáneo y sin complicaciones.

--- 

📂 Se incluye el archivo PBIX para que puedas probarlo, tocarlo y verlo en acción.


![isfiltered](../../docs/imagenes/docs/imagenes/01Widget.png)





