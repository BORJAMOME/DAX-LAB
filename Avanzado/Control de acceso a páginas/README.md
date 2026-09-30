## Redirigir a una página según el grupo del usuario

**Aviso antes de nada:** esto es un patrón de *navegación*, no de seguridad. Controla a qué página lleva un botón, pero los datos siguen en el modelo y cualquier usuario con acceso al informe puede llegar a ellos (pestañas de página, exportar datos, Analizar en Excel). Si los datos son sensibles de verdad, la respuesta es RLS/OLS o un informe aparte. Lo explico al final.

### El caso

Un informe con dos páginas:

- **Operativa**: ventas, stock y KPIs generales. La ve todo el equipo comercial.
- **Proyecciones Financieras**: presupuestos y escenarios. Pensada para Finanzas.

Requisito de negocio: que el botón "Proyecciones" lleve a los de Finanzas a su página y al resto a una página de "Solicitar acceso" con el contacto del responsable. El objetivo es orientar al usuario, no bloquearle.

### Solución

1. **Grupo en Entra ID (Azure AD)**: `Security_AzGroup_Finance`, con los usuarios de Finanzas.
2. **Membresía en el modelo**: un flujo de Power Automate exporta los miembros a un CSV en SharePoint, y ese CSV se carga como `Dim_Security_AzGroup_Finance` con la columna `userPrincipalName`.
3. **Página "Solicitar acceso"** con instrucciones y contacto.
4. **Medida de destino**:

```DAX
Acceso_Finanzas =
IF (
    USERPRINCIPALNAME ()
        IN VALUES ( 'Dim_Security_AzGroup_Finance'[userPrincipalName] ),
    "Proyecciones Financieras",
    "Solicitar Acceso"
)
```

5. **Botón**: Acción → Navegación de página → Destino → fx → `[Acceso_Finanzas]`.

La página Proyecciones se oculta en la vista de lectura para que no aparezca en las pestañas.

### Ocultar visuales concretos

```DAX
Acceso_Finanzas_Visual =
INT (
    USERPRINCIPALNAME ()
        IN VALUES ( 'Dim_Security_AzGroup_Finance'[userPrincipalName] )
)
```

Como filtro de visual con la condición "es 1", el gráfico aparece vacío para quien no está en el grupo. Es útil para no enseñar un visual a medio cargar, pero de nuevo: el dato sigue en el modelo.

### Límites (y qué haría en un entorno real)

- **Páginas ocultas no son privadas.** Un usuario puede mostrarlas desde el menú, o llegar por un marcador o un enlace directo.
- **Filtrar el visual no protege el dato.** Cualquier otro visual, el panel de exportación o Analizar en Excel lo devuelven.
- **La membresía se desfasa.** Si el flujo de Power Automate falla, el CSV se queda con usuarios antiguos.

Para restringir datos de verdad:

- **RLS (Row-Level Security)** en el modelo, con roles asignados a grupos de Entra ID. Cada usuario solo recibe las filas que le tocan, lo vea desde donde lo vea.
- **OLS (Object-Level Security)** si lo que hay que ocultar son tablas o columnas enteras (por ejemplo, márgenes).
- **Informes o espacios de trabajo separados** cuando la audiencia es muy distinta.

El patrón de esta página tiene sentido *encima* de RLS: la seguridad la da el modelo, y la navegación dinámica mejora la experiencia de quien no tiene acceso.
