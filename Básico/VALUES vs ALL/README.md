## VALUES vs ALL

- VALUES → respeta el contexto actual. Solo muestra lo que está visible o filtrado. 

- ALL → ignora todos los filtros en esa columna o tabla. Muestra todo lo que hay en el modelo, sin importar slicers ni filtros.

- ### Ejemplo
  
***Contar marcas visibles en el contexto actual***

```
Función VALUES = COUNTROWS(VALUES(DimProduct[BrandName]))
```

- Si tienes un slicer por categoría y seleccionas “Electrónica”: VALUES devolverá solo las marcas dentro de esa categoría → el conteo se ajusta.

- Si no hay slicers activos → devuelve todas las marcas visibles en el contexto del visual.




  ***Contar todas las marcas de la tabla ignorando filtros***

  ```
  Función ALL II = COUNTROWS(ALL(DimProduct[BrandName]))
  ```

- Aquí no importa el slicer ni el filtro de fila.

- Siempre devuelve el total de marcas en la tabla DimProduct.


![ejemplo](/docs/imagenes/VALUESvsALL.png)
