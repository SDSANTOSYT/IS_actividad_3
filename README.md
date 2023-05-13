## INFORMACIÓN DEL ESTUDIANTE
**Nombre:** Sebastián De Jesús Santos Vergara

**NRC:**  4519

### PREGUNTAS
1.	¿Cuál es el producto más vendido en una ubicación geográfica específica?


Para este caso se tomó como ubicación geográfica a la ciudad de nacimiento, especificamente la ciudad de barranquilla. Para responder esta pregunta se realizó la siguiente consulta en SQL:

```
WITH clientes_barranquilla AS (
SELECT Numero, Nombre_1, Nombre_2, Apellidos, Ciudad_de_Nacimiento
FROM `int-is.is_act3.clientes` 
WHERE Ciudad_de_Nacimiento = 'Barranquilla'
), 
compras_barranquilla AS(
SELECT Numero, producto As Codigo
FROM clientes_barranquilla cli
INNER JOIN `is_act3.compras` com ON cli.Numero = com.cliente
)

SELECT COUNT(*) AS numero_compras, prod.Codigo, Producto 
FROM compras_barranquilla com
INNER JOIN `is_act3.productos` prod ON prod.Codigo = com.Codigo
GROUP BY Producto, prod.Codigo
ORDER BY numero_compras DESC
```

como se puede apreciar se usaron 2 subconsultas:

la primera llamada  `clientes_barranquilla` donde se toman los datos que necesitamos de los clientes de la tabla del mismo nombre para filtrar a los que tienen a barranquilla como su ciudad de nacimiento, y la segunda llamada `compras_barranquilla` donde se hace un `INNER JOIN` con la tabla de `compras` para obtener las compras realizadas por los clientes en barranquilla.

con las subconsultas ya hechas finalmente se hace una consulta donde se agrupan los datos por el `producto`, `codigo` y se hace un `COUNT` para obtener el numero de compras de cada producto. Eso nos devolverá una tabla con todos los productos con su nombre, su codigo ordenados desde mayor numero de compras a menor y el primero de la lista será el mas vendido.

2.	¿Cuántos clientes han comprado productos de un rango de precios específico?