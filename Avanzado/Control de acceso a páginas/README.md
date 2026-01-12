### Control de acceso a la página de “X” del informe según departamento

Imagina que tienes un informe de Power BI con varias páginas:

- Página Operativa: ventas, stock, KPIs generales → accesible a todos los comerciales.

- Página de Proyecciones Financieras: contiene presupuestos, márgenes futuros y escenarios estratégicos → solo accesible al equipo de Finanzas.

El requisito: los comerciales no deben poder ver la página de Proyecciones Financieras y, si intentan entrar, deben ser redirigidos a una página de “Solicitar Acceso”.

#### Solución:

1. Grupo de seguridad en Azure AD

  - Creamos un grupo llamado Security_AzGroup_Finance

  - Solo los usuarios de Finanzas son miembros.

2. Importar la membresía del grupo a Power BI

  - Usamos Power Automate para extraer la lista de miembros y guardarla en SharePoint como CSV.

  - Cargamos ese CSV en Power BI → tabla: Dim_Security_AzGroup_Finance con columna userPrincipalName.

3. Página de solicitud de acceso

  - Creamos una página llamada Solicitar Acceso

  - Contiene instrucciones para pedir permisos y contacto del responsable.

4. Medida DAX para controlar acceso

```
Acceso_Finanzas =
IF (
    USERPRINCIPALNAME()
        IN VALUES('Dim_Security_AzGroup_Finance'[userPrincipalName]),
    "Proyecciones Financieras",
    "Solicitar Acceso"
)
```

#### Qué hace esta medida:

  - Comprueba si el usuario actual está en el grupo de Finanzas.

  - Devuelve el nombre de la página a la que debe navegar:

      - Si es Finanzas → Proyecciones Financieras

      - Si no → Solicitar Acceso

5. Configurar el botón de navegación

  - Insertar un botón de acción

  - Tipo → “Page navigation”

  - Destino → fx → campo [Acceso_Finanzas]

Con esto:

- Los usuarios autorizados llegan a la página sensible.

- Los demás son redirigidos automáticamente a la página de solicitud.


### Filtrar visuales sensibles

Si quieres controlar cada visual de la página, puedes usar:
```
Acceso_Finanzas_Visual =
IF (
    USERPRINCIPALNAME()
        IN VALUES('Dim_Security_AzGroup_Finance'[userPrincipalName]),
    1,
    0
)
```

- Aplicas esta medida como filtro a nivel visual: is 1

- Resultado:

    - Finanzas → ven los gráficos

    - Otros → gráficos no se muestran

Puedes añadir un mensaje explícito cuando no tengan acceso, mejorando la experiencia del usuario.

