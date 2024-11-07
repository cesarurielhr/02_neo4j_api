# DOCUMENTACION DE LA API  
## Bases de Datos NoSQL
> [!NOTE]
> **Alumno:** César Uriel Hernández Rodríguez `@cesarurielhr`  
> **Grupo:** 5A (7:00-8:00)  
> **Docente:** Jorge Saúl Montes Cáceres  

**1. Considere el caso de una empresa mayorista de materiales de construcción. Todas las ventas que hace una sucursal implica la emisión de una factura. A la empresa le interesan los siguientes aspectos:**

## Prerequisitos de las APIS:
**1.1 Desargar el archivo txt llamado datosNEO4j.txt**

**1.2. Descargar desde DockerHub la imagen de la APIS con el siguiente comando:**
```
docker pull cesarurielhr/02_redis_api
```
**1.3 Inicializar un docker Compose en que incluya un contenedor api y otro contenedorque contiene los bd de neo4j y la bd de cache redis**

**1.4 ENDPOINTS**
 
**1.5 Querys**
Q01. Obtener la lista de productos que tienen menos de 10 unidades en stock.
MATCH (p:Producto) 
WHERE p.stock < 10 
RETURN p;
- Q02. Encontrar los proveedores que suministran productos de una categoría en específico..
  MATCH (prov:Proveedor)-[:SUMINISTRA]->(p:Producto)-[:PERTENECE_A]->(cat:Categoria {nombre: 'Smarphones'}) RETURN DISTINCT prov
- Q03. Obtener la lista de pedidos de compra que fueron realizados a un proveedor en específico.
  MATCH (prov:Proveedor {id: 'PR002'})<-[:PEDIDO_REALIZADO_A]-(pc:PedidoCompra) RETURN pc, prov
- Q04. Encontrar los productos que han sido comprados por más de 5 clientes diferentes.
   MATCH (cli:Cliente)-[:HACE]->(pv:PedidoVenta)
 WITH pv, COUNT(DISTINCT cli) AS numClientes
 WHERE numClientes > 5
- Q05. Obtener la lista de todos los  proveedores.
  MATCH (prov:Proveedor) RETURN prov
- Q06. Encontrar los pedidos de venta que tienen una devolución
 MATCH (dv:Devolucion)-[:PERTENECE_A]->(pv:PedidoVenta) 
RETURN pv;
 RETURN pv;
- Q07. Listar los pedidos de venta que tienen un valor total mayor a $10,000.
- MATCH (pv:PedidoVenta) 
WHERE pv.precioTotal > 10000 
RETURN pv;
- Q08. Cambiar todos los productos suministrados por un proveedor a otro proveedor.
 MATCH (provAnt:Proveedor {id: 'PR002'})-[r:SUMINISTRA]->(p:Producto) 
           MATCH (provNuevo:Proveedor {id: 'PR005'}) 
           DELETE r 
           CREATE (provNuevo)-[:SUMINISTRA]->(p) 
           RETURN p;
- Q09. Obtener la lista de proveedores que han recibido pedidos de compra por más de $50,000 en total.
MATCH (prov:Proveedor)<-[:REALIZADO_A]-(pc:PedidoCompra)
WITH prov, SUM(pc.valor) AS totalCompras
WHERE totalCompras > 50000
RETURN prov;
- Q10. Encontrar los productos que se encuentran agotados (sin stock) en el inventario.
  MATCH (p:Producto) 
WHERE p.stock = 0 
RETURN p;
- Q11. Obtener la lista de clientes.
    MATCH (cli:Cliente) 
    RETURN cli;

- Q12. Eliminar todos los proveedores y sus nodos asociados..
   MATCH (p:Proveedor)
            OPTIONAL MATCH (p)-[r]-()
            DETACH DELETE p
            RETURN COUNT(p) AS eliminados;
- Q13. Todos los productos de una categoría específica eliminados del inventario.
MATCH (p:Producto)-[:PERTENECE_A]->(cat:Categoria {nombre: 'Laptops'})
            RETURN p

- Q14. Todos los pedidos de compra de un proveedor en particular son transferidos a otro proveedor por un cambio de contrato.
  MATCH (provAnt:Proveedor {id: 'PR004'})<-[:REALIZADO_A]-(pc:PedidoCompra) 
MATCH (provNuevo:Proveedor {id: 'PR001'}) 
MATCH (provAnt)<-[r:REALIZADO_A]-(pc) 
DELETE r // Eliminar solo la relación
CREATE (provNuevo)-[:REALIZADO_A]->(pc);
- Q15. Eliminar todos los clientes que han realizado devoluciones..
  MATCH (d:Devolucion)-[:PERTENECE_A]->(c:Cliente)
            WITH c, d, c AS clienteEliminado
            DETACH DELETE c, d
            RETURN clienteEliminado;
