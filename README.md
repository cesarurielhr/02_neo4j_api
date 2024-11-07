# DOCUMENTACION DE LA API ⚛️
## Bases de Datos NoSQL 🤟
## LABORATORIO 02_NEO4J_API 🟣🟠🔵

> **DATOS DEL ALUMNO**🧑‍🎓
> 
> **Alumno:** César Uriel Hernández Rodríguez [`@cesarurielhr`](https://github.com/cesarurielhr) 👾  
> **Grupo:** 5A (7:00-8:00)  
> **Docente:** Jorge Saúl Montes Cáceres  

## CASO EJEMPLO - Gestión de Inventarios de una empresa🏬

Para la Gestión de Inventarios de una empresa de distribución de productos electrónicos, un sistema adecuado debería cubrir varias áreas clave y permitir realizar diversas consultas (queries) para optimizar la operación. A continuación, te doy una descripción detallada de los componentes y las consultas que se necesitan en este sistema.

**Estructura del Sistema:** 🏗️

**Productos:** 📺

 - Atributos: ID, nombre de la empresa, país de origen, teléfono y correo electrónico de contacto.
 - Un proveedor puede suministrar uno o más productos, y es esencial mantener esta relación para poder rastrear la procedencia de cada producto.

**Pedidos de Compra:** 👛

 - Atributos: producto pedido, cantidad solicitada, precio unitario, fecha de pedido y fecha de recepción.
 - Cada pedido de compra está asociado a un proveedor específico, lo cual permite controlar las relaciones de compra y el flujo de stock en el inventario.

**Clientes:** 🧔

 - Atributos: ID, nombre, dirección, ciudad, teléfono y correo electrónico.
 - Los clientes pueden hacer múltiples pedidos de venta a lo largo del tiempo, lo que permite registrar la historia de compras y la fidelidad de cada cliente.

**Pedidos de Venta:** 💵

  - Atributos: productos solicitados, cantidad, precio de venta, y fecha de entrega.
  - Cada pedido está asociado a un cliente, lo cual permite rastrear las ventas y calcular el valor total de cada pedido.
    
**Devoluciones:** ⏮️

  - Atributos: fecha de devolución, motivo de la devolución, y si se realizó un reembolso.
  - Cada devolución está vinculada a un pedido de venta y a un cliente específico, permitiendo el control de devoluciones y del servicio post-venta.

## Prerequisitos de las APIS: 🤓🐋
**1.1 Desargar el archivo txt llamado [Datos_NEO4j.txt](https://github.com/cesarurielhr/02_neo4j_api/blob/main/Datos_NEO4J.txt)**

**1.2. Descargar desde DockerHub 🐳 la imagen de la APIS con el siguiente comando:** 
```
docker pull cesarurielhr/02_redis_api
```
**1.3 Inicializar un docker-Compose 🐳 en que incluya un contenedor api y otros contenedores que contienen los bd prinpal como el caso de neo4j y la bd de cache con redis**

  Archivo: [docker-compose.yml](https://github.com/cesarurielhr/02_neo4j_api/blob/main/docker-compose.yml) 

**1.4 ENDPOINTS** 💻

 Archivo de backend en postaman [02_ne4j_api_app.postman_collection.json ](https://github.com/cesarurielhr/02_neo4j_api/blob/main/02_ne4j_api_app.postman_collection.json)
 
 Para poder utilizar el archivo deberas importarlo a postman para probar las 12 querys selecionadas de las cuales 1 fue creada para complentar el requerimento 
 este query debe de ejecutarse antes de las demas querys.


**1.5 Querys** 🕸️

Los siguiente fueron realizado con lenguaje cypher para NEO4J siendo las querys solitadas para el laboratorio

Q01. Obtener la lista de productos que tienen menos de 10 unidades en stock.
  ```
  MATCH (p:Producto) 
  WHERE p.stock < 10 
  RETURN p
  ```
- Q02. Encontrar los proveedores que suministran productos de una categoría en específico..
  ```
  MATCH (prov:Proveedor)-[:SUMINISTRA]->(p:Producto)-[:PERTENECE_A]->(cat:Categoria {nombre: 'Smartphones'}) 
  RETURN DISTINCT prov
  ```
- Q03. Obtener la lista de pedidos de compra que fueron realizados a un proveedor en específico..
  ```
  MATCH (prov:Proveedor {id: 'PR002'})<-[:PEDIDO_REALIZADO_A]-(pc:PedidoCompra) RETURN pc, prov
  ```
- Q04. Encontrar los productos que han sido comprados por más de 5 clientes diferentes.
  ```
   MATCH (cli:Cliente)-[:HACE]->(pv:PedidoVenta)-[:INCLUYE_PRODUCTO]->(p:Producto)
   WITH pv,p, COUNT(DISTINCT cli) AS numClientes
   WHERE numClientes > 5 return pv,p
  ```
- Q05. Obtener la lista de todos los  proveedores.
  ```
  MATCH (prov:Proveedor) RETURN prov
  ```
- Q06. Encontrar los pedidos de venta que tienen una devolución
  ```
  MATCH (cli:Cliente)<-[:PERTENECE_A]-(devolucion:Devolucion)-[:PERTENECE_A]->(pedidoVenta:PedidoVenta)
  RETURN cli, devolucion, pedidoVent
  ```
- Q07. Listar los pedidos de venta que tienen un valor total mayor a $10,000.
  ```
   MATCH (pv:PedidoVenta) 
   WHERE pv.precioTotal > 10000 
   RETURN pv;
   ```
- Q08. Cambiar todos los productos suministrados por un proveedor a otro proveedor.
   ```
   MATCH (provAnt:Proveedor {id: 'PR002'})-[r:SUMINISTRA]->(p:Producto) 
           MATCH (provNuevo:Proveedor {id: 'PR005'}) 
           DELETE r 
           CREATE (provNuevo)-[:SUMINISTRA]->(p) 
           RETURN p
   ```
   Comprobacion: Antes de la ejecucion del query
   ```
   MATCH(prov:Proveedor {id:'PR002'})-[:SUMINISTRA]->(p:Producto) return prov,p
   MATCH(prov:Proveedor {id:'PR005'})-[:SUMINISTRA]->(p:Producto) return prov,p
   ```
  Despues de la ejecucion del query
   ```
   MATCH(prov:Proveedor {id:'PR002'})-[:SUMINISTRA]->(p:Producto) return prov,p
   MATCH(prov:Proveedor {id:'PR005'})-[:SUMINISTRA]->(p:Producto) return prov,p
   ```
- Q09. Obtener la lista de proveedores que han recibido pedidos de compra por más de $50,000 en total.
  ```
  MATCH (prov:Proveedor)<-[:PEDIDO_REALIZADO_A]-(pc:PedidoCompra)
  WITH prov, SUM(pc.valor) AS totalCompras
  WHERE totalCompras > 50000
  RETURN prov
  ```
- Q10. Encontrar los productos que se encuentran agotados (sin stock) en el inventario.
  ```
  MATCH (p:Producto) 
  WHERE p.stock = 0 
  RETURN p
  ```
- Q11. Obtener la lista de clientes.
  ```
     MATCH (cli:Cliente) 
     RETURN cli
  ```
- Q12. Eliminar todos los proveedores y sus nodos asociados..
  ```
   MATCH (prov:Proveedor)
   OPTIONAL MATCH (prov)-[r1]-(p:Producto)-[r2]-()
   DETACH DELETE prov,p
   RETURN COUNT(prov) AS proveedoresEliminados;

  ```
- Q13. Todos los productos de una categoría específica eliminados del inventario.
  ```
  //Primero antes de ejecutar y depues de ejecutar
  MATCH (p:Producto)-[:PERTENECE_A]->(cat:Categoria {nombre: 'Laptops'})
  RETURN p
  //Query 13
  MATCH (p:Producto)-[:PERTENECE_A]->(cat:Categoria {nombre:'Laptops'}) DETACH DELETE p;
  ```
- Q14. Todos los pedidos de compra de un proveedor en particular son transferidos a otro proveedor por un cambio de contrato.
  ```
  MATCH (provNuevo:Proveedor {id: 'PR001'}) 
  MATCH (provAnt:Proveedor {id: 'PR002'})<-[r:PEDIDO_REALIZADO_A]-(pc) 
  DELETE r 
  CREATE (provNuevo)<-[:PEDIDO_REALIZADO_A]-(pc);
  ```
- Q15. Eliminar todos los clientes que han realizado devoluciones..
  ```
  MATCH (d:Devolucion)-[:PERTENECE_A]->(c:Cliente)
            WITH c, d, c AS clienteEliminado
            DETACH DELETE c, d
            RETURN clienteEliminado;
  ```
  Comprobar con el query Q6
