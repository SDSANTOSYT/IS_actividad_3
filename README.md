## INFORMACIÓN DEL ESTUDIANTE
**Nombre:** Sebastián De Jesús Santos Vergara

**NRC:**  4519

### PREGUNTAS
_1.	¿Cuál es el producto más vendido en una ubicación geográfica específica?_


Para este caso se tomó como ubicación geográfica a la ciudad de nacimiento, especificamente la ciudad de barranquilla. Para responder esta pregunta se realizó la siguiente consulta en SQL:

```sql
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

_2.	¿Cuántos clientes han comprado productos de un rango de precios específico?_

Aquí usaremos un rango de precios entre $1.500 a $7.500

-	Con esta consulta se obtiene una tabla con una sola columna que tiene la cantidad de clientes que han comprado los productos:

```sql
WITH productos_intervalo AS (
SELECT Codigo, Precio
FROM `int-is.is_act3.productos` 
WHERE Precio > 1500 AND Precio < 7500
), 
compras_intervalo AS (
SELECT Codigo, Precio, cliente
FROM productos_intervalo 
INNER JOIN `is_act3.compras` compras ON compras.producto = Codigo
),
clientes AS (
SELECT Numero, Nombre_1, Nombre_2, Apellidos
FROM `is_act3.clientes` 
INNER JOIN compras_intervalo ON Numero = cliente
GROUP BY Numero, Nombre_1, Nombre_2, Apellidos
)

SELECT COUNT(*) AS cantidad_clientes
FROM clientes
```
aqui también usamos 2 subconsultas:

la primera `productos_intervalo` para tomar los productos dentro del rango de precios, y la segunda `compras_intervalo` para agarrar las compras de dichos productos. finalmente solo queda hacer una consulta con un `COUNT`


-	En el caso de que también se quisiera saber quiénes son los clientes que compraron estos productos, se puede usar la siguiente consulta y solo se tendría que ver la cantidad de entradas de la tabla para saber cuántos son:

```sql
WITH productos_intervalo AS (
SELECT Codigo, Precio
FROM `int-is.is_act3.productos` 
WHERE Precio > 1500 AND Precio < 7500
), 
compras_intervalo AS (
SELECT Codigo, Precio, cliente
FROM productos_intervalo 
INNER JOIN `is_act3.compras` compras ON compras.producto = Codigo
)

SELECT Numero, Nombre_1, Nombre_2, Apellidos
FROM `is_act3.clientes` 
INNER JOIN compras_intervalo ON Numero = cliente
GROUP BY Numero, Nombre_1, Nombre_2, Apellidos

```