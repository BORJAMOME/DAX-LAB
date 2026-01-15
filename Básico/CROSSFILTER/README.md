## CROSSFILTER 


***CROSSFILTER*** permite cambiar la dirección de propagación de filtros entre dos tablas relacionadas.

Por defecto, los filtros van de la tabla “uno” a la “muchos” (single direction).

Con ***CROSSFILTER***(..., Both), los filtros se propagan en ambos sentidos, generando resultados que antes no veías.

#### Ejemplo:

Imagina un negocio de retail:

- Una tabla Clientes y otra tabla Ventas.

- Cada cliente puede comprar varios productos (relación 1:n).

Si filtramos por color de producto:

- Dirección normal: el filtro va de clientes → ventas.

- Con Both: el filtro fluye de clientes → ventas y de ventas → clientes, permitiendo que un filtro aplicado en colores afecte quién es cliente y viceversa.

- CROSSFILTER(Column1, Column2, Direction) se usa dentro de CALCULATE.

- Direction puede ser:

  - ***Single*** → dirección predeterminada de la relación
      
  - ***Both*** → activa la relación bidireccional temporalmente para esa medida
      
  - ***None*** → ignora la relación completamente
 
  

| Medida                            | Qué hace                                                                                                                                      |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `Clientes-Colores`                | Cuenta clientes considerando ventas filtradas por colores, con relación bidireccional (Both).                                                 |
| `Clientes-Colores (sin magia)`    | Cuenta clientes **sin cruzar filtros** de ventas → el color no afecta.                                                                        |
| `Reino Unido-Colores`             | Cuenta clientes de Reino Unido y además filtra colores de ventas → Both permite que el color “influya” sobre los clientes filtrados por país. |
| `Reino Unido-Colores (sin magia)` | Simplemente cuenta clientes de Reino Unido, sin considerar colores → dirección normal de la relación no propaga el filtro de ventas.          |





![ejemplo](/docs/imagenes/crossfilter.png)






```
Clientes-Colores =
CALCULATE (
    [Total_Clientes],
    CROSSFILTER ( dim_Clientes[IdCliente], Ventas[IdCliente], BOTH )
)
```

```
Clientes-Reino Unido =
CALCULATE (
    COUNT ( dim_Clientes[IdCliente] ),
    dim_Clientes[País] = "Reino Unido"
)
```

```
Reino Unido-Colores =
CALCULATE (
    [Clientes-Reino Unido],
    CROSSFILTER ( dim_Clientes[IdCliente], Ventas[IdCliente], BOTH )
)
```

```
Clientes-Colores (sin magia) = [Total_Clientes]
```

```
Reino Unido-Colores (sin magia) = [Clientes-Reino Unido]
```

