## EJERCICIOS JARDINERÍA ##

Resolución a los ejercicios propuestos por José Juan Sánchez Hernández del IES Celia Viñas (Almería) - 2019/2020 y disponibles en su web https://josejuansanchez.org/bd/ejercicios-consultas-sql/index.html#jardiner%C3%ADa 

### 1. Consultas sobre una tabla
1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.
   ```sql
    select codigo_oficina , ciudad from oficina o ;
   ```
2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.
3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.
4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.
5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.
6. Devuelve un listado con el nombre de los todos los clientes españoles.
7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.
8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
9.  Utilizando la función YEAR de MySQL.
10. Utilizando la función DATE_FORMAT de MySQL.
Sin utilizar ninguna de las funciones anteriores.
Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.
Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.
Utilizando la función ADDDATE de MySQL.
Utilizando la función DATEDIFF de MySQL.
Devuelve un listado de todos los pedidos que fueron rechazados en 2009.
Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.
Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.
Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.
Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.
Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.

### 2. Consultas multitabla (Composición interna)
Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con **INNER JOIN** y **NATURAL JOIN**.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```sql
    select cliente.nombre_cliente "Nombre Cliente", CONCAT(empleado.nombre, empleado.apellido1) "Responsable Ventas"  
    from cliente left join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
    order by cliente.nombre_cliente asc;
```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
    select cliente.nombre_cliente "Cliente con Pagos", CONCAT(empleado.nombre, empleado.apellido1) "Responsable Ventas"  
    from cliente left join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
    left join pago on cliente.codigo_cliente = pago.codigo_cliente
    group by cliente.nombre_cliente, empleado.nombre, empleado.apellido1
    order by cliente.nombre_cliente asc;
```
   
3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
    select cliente.nombre_cliente "Cliente sin Pagos", CONCAT(empleado.nombre, empleado.apellido1) "Responsable Ventas"  
    from cliente left join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
    where codigo_cliente not in (select pago.codigo_cliente from pago)  
    group by cliente.nombre_cliente, empleado.nombre, empleado.apellido1
    order by cliente.nombre_cliente asc;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
    select cliente.nombre_cliente "Cliente con Pagos", CONCAT(empleado.nombre, empleado.apellido1) "Responsable Ventas", oficina.ciudad "Ciudad"
    from cliente
    left join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
    left join pago on cliente.codigo_cliente = pago.codigo_cliente
    left join oficina on empleado.codigo_oficina = oficina.codigo_oficina
    group by cliente.nombre_cliente, empleado.nombre, empleado.apellido1, oficina.ciudad
    order by cliente.nombre_cliente asc;
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.
   ```sql
    select cliente.nombre_cliente "Cliente sin Pagos", CONCAT(empleado.nombre, empleado.apellido1) "Responsable Ventas", oficina.ciudad "Ciudad"  
    from cliente 
    left join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
    left join oficina on empleado.codigo_oficina = oficina.codigo_oficina
    where codigo_cliente not in (select pago.codigo_cliente from pago)  
    group by cliente.nombre_cliente, empleado.nombre, empleado.apellido1, oficina.ciudad 
    order by cliente.nombre_cliente asc;
   ```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.
   ```sql
   select CONCAT(oficina.linea_direccion1, " ",oficina.linea_direccion2) "Dirección de la oficina" from empleado
   right join cliente on empleado.codigo_empleado = cliente.codigo_empleado_rep_ventas
   left join oficina on empleado.codigo_oficina = oficina.codigo_oficina 
   where cliente.ciudad = "Fuenlabrada"
   group by oficina.linea_direccion1, oficina.linea_direccion2 ;
   ```
7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.
   ```sql
   select cliente.codigo_cliente, cliente.nombre_cliente "Cliente", CONCAT(empleado.nombre," ", empleado.apellido1) "Responsable Ventas", oficina.ciudad "Ciudad"  
    from cliente 
    left join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
    left join oficina on empleado.codigo_oficina = oficina.codigo_oficina
    group by cliente.codigo_cliente, cliente.nombre_cliente, empleado.nombre, empleado.apellido1, oficina.ciudad 
    order by cliente.codigo_cliente asc;
   ```
8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.
```sql
   select e.codigo_empleado, concat(e.apellido1, ", ", e.nombre ) , e.codigo_jefe, concat(jefe.apellido1, ", ", jefe.nombre )
from empleado e
left join empleado jefe on e.codigo_jefe = jefe.codigo_empleado
group by e.codigo_jefe, e.codigo_empleado ;
   ```
10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.
```sql
select cliente.codigo_cliente, cliente.nombre_cliente from cliente 
left join pedido on cliente.codigo_cliente = pedido.codigo_cliente
where fecha_entrega > fecha_esperada
group by cliente.codigo_cliente, cliente.nombre_cliente;
```
11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.
```sql
select cliente.codigo_cliente, producto.gama from pedido 
left join cliente on pedido.codigo_cliente = cliente.codigo_cliente 
left join detalle_pedido on pedido.codigo_pedido = detalle_pedido.codigo_pedido 
left join producto on detalle_pedido.codigo_producto = producto.codigo_producto
group by cliente.codigo_cliente, producto.gama ;
```

#### 2.1. Consultas multitabla (Composición externa)
Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN y NATURAL RIGHT JOIN.

Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.  
```
select cliente.codigo_cliente, nombre_cliente from cliente left join pago on cliente.codigo_cliente = pago.codigo_cliente where pago.codigo_cliente is null
group by codigo_cliente, nombre_cliente;
```
Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.
```
select cliente.codigo_cliente , nombre_cliente from cliente left join pedido on cliente.codigo_cliente = pedido.codigo_cliente where pedido.codigo_cliente is null;
```
Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.

```
select cliente.codigo_cliente , nombre_cliente from cliente 
left join pedido on cliente.codigo_cliente = pedido.codigo_cliente
left join pago on cliente.codigo_cliente = pago.codigo_cliente where pago.codigo_cliente is null and pedido.codigo_cliente is null
group by codigo_cliente, nombre_cliente;
```
Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.

Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.

Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

Devuelve un listado de los productos que nunca han aparecido en un pedido.

Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.
```
select oficina.codigo_oficina, oficina.ciudad from oficina 
left join empleado e on e.codigo_oficina = oficina.codigo_oficina 
left join cliente c on c.codigo_empleado_rep_ventas = e.codigo_empleado
left join pedido on pedido.codigo_cliente = c.codigo_cliente 
left join detalle_pedido dp on pedido.codigo_pedido = dp.codigo_pedido 
left join producto p on dp.codigo_producto = p.codigo_producto 
left join gama_producto g on p.gama = g.gama
where g.gama NOT LIKE("Frutales") group by codigo_oficina, ciudad;
```
Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago. 
```
select * from cliente c
left join pedido p on p.codigo_cliente = c.codigo_cliente 
left join pago pa on pa.codigo_cliente = c.codigo_cliente 
where p.codigo_pedido is not null and pa.codigo_cliente is null group by nombre_cliente;
```
Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado. 
```
select e.codigo_empleado empleado, e.nombre nombre, e.apellido1 apellido, e2.codigo_empleado codigo_jefe, e2.nombre nombre_jefe, e2.apellido1 apellido_jefe from empleado e
left join cliente c on c.codigo_empleado_rep_ventas = e.codigo_empleado 
left join empleado e2 on e.codigo_jefe = e2.codigo_empleado 
where c.codigo_cliente is null;
```
### 3. Consultas resumen

¿Cuántos empleados hay en la compañía? 

¿Cuántos clientes tiene cada país? 

¿Cuál fue el pago medio en 2009? 

¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma descendente por el número de pedidos. 

Calcula el precio de venta del producto más caro y más barato en una misma consulta. 

Calcula el número de clientes que tiene la empresa. 

¿Cuántos clientes tiene la ciudad de Madrid? 

¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan por M? 

Devuelve el nombre de los representantes de ventas y el número de clientes al que atiende cada uno. 

Calcula el número de clientes que no tiene asignado representante de ventas. 

Calcula la fecha del primer y último pago realizado por cada uno de los clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente. 

Calcula el número de productos diferentes que hay en cada uno de los pedidos. 

Calcula la suma de la cantidad total de todos los productos que aparecen en cada uno de los pedidos. 

Devuelve un listado de los 20 productos más vendidos y el número total de unidades que se han vendido de cada uno. El listado deberá estar ordenado por el número total de unidades vendidas.
La facturación que ha tenido la empresa en toda la historia, indicando la base imponible, el IVA y el total facturado. La base imponible se calcula sumando el coste del producto por el número de unidades vendidas de la tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la suma de los dos campos anteriores.
La misma información que en la pregunta anterior, pero agrupada por código de producto.
La misma información que en la pregunta anterior, pero agrupada por código de producto filtrada por los códigos que empiecen por OR.
Lista las ventas totales de los productos que hayan facturado más de 3000 euros. Se mostrará el nombre, unidades vendidas, total facturado y total facturado con impuestos (21% IVA).
### 4. Subconsultas
#### 4.1 Con operadores básicos de comparación
Devuelve el nombre del cliente con mayor límite de crédito.  
```
select c.nombre_cliente, c.limite_credito from cliente c
where c.limite_credito >= all (select c2.limite_credito from cliente c2);
```
Devuelve el nombre del producto que tenga el precio de venta más caro. 


Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla detalle_pedido. Una vez que sepa cuál es el código del producto, puede obtener su nombre fácilmente.)  


Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar INNER JOIN).  


Devuelve el producto que más unidades tiene en stock.  

Devuelve el producto que menos unidades tiene en stock.
Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de Alberto Soria.
#### 4.2 Subconsultas con ALL y ANY
Devuelve el nombre del cliente con mayor límite de crédito.
Devuelve el nombre del producto que tenga el precio de venta más caro.
Devuelve el producto que menos unidades tiene en stock.
#### 4.3 Subconsultas con IN y NOT IN
Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.
Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
Devuelve un listado que muestre solamente los clientes que sí han realizado ningún pago.
Devuelve un listado de los productos que nunca han aparecido en un pedido.
Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.
Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.
Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
#### 4.4 Subconsultas con EXISTS y NOT EXISTS
Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
Devuelve un listado que muestre solamente los clientes que sí han realizado ningún pago.
Devuelve un listado de los productos que nunca han aparecido en un pedido.
Devuelve un listado de los productos que han aparecido en un pedido alguna vez.
1.4.8.5 Subconsultas correlacionadas
### 5. Consultas variadas
Devuelve el listado de clientes indicando el nombre del cliente y cuántos pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido.  
```
select c.nombre_cliente , count(p.codigo_pedido) from cliente c
left join pedido p on p.codigo_cliente = c.codigo_cliente where p.codigo_pedido is not null group by c.nombre_cliente order by c.nombre_cliente 
```
Devuelve un listado con los nombres de los clientes y el total pagado por cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han realizado ningún pago.  
```
select c.nombre_cliente, sum(p.total) from cliente c
left join pago p on p.codigo_cliente = c.codigo_cliente where p.codigo_cliente is not null group by c.nombre_cliente;
```
Devuelve el nombre de los clientes que hayan hecho pedidos en 2008 ordenados alfabéticamente de menor a mayor.  
```
select c.nombre_cliente, p.codigo_pedido, p.fecha_pedido from cliente c
left join pedido p on c.codigo_cliente = p.codigo_cliente 
where YEAR(p.fecha_pedido) = '2008' order by c.nombre_cliente desc;
```
Devuelve el nombre del cliente, el nombre y primer apellido de su representante de ventas y el número de teléfono de la oficina del representante de ventas, de aquellos clientes que no hayan realizado ningún pago.
```
 select c.nombre_cliente, e.nombre , e.apellido1 , o.ciudad , o.telefono from cliente c
 left join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado 
 left join oficina o on e.codigo_oficina = o.codigo_oficina 
 left join pago p on c.codigo_cliente = p.codigo_cliente 
 where p.codigo_cliente is null;
```

Devuelve el listado de clientes donde aparezca el nombre del cliente, el nombre y primer apellido de su representante de ventas y la ciudad donde está su oficina.
Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.
Devuelve un listado indicando todas las ciudades donde hay oficinas y el número de empleados que tiene.
