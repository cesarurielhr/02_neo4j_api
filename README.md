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
  
**1.4 Cargar Datos al neo4j**
  ```
  //Datos para el escenario de Laboratorio 3
  // Creación de categorías
  CREATE (laptops:Categoria {nombre: 'Laptops'}),
         (smartphones:Categoria {nombre: 'Smartphones'}),
         (televisores:Categoria {nombre: 'Televisores'}) RETURN laptops,smartphones,televisores;
  MATCH 
    (laptops:Categoria {nombre: 'Laptops'}),
    (smartphones:Categoria {nombre: 'Smartphones'}),
    (televisores:Categoria {nombre: 'Televisores'})
  CREATE 
    (laptops)-[:CONTIENEN_PRODUCTOS]->(smartphones),
    (laptops)-[:CONTIENEN_PRODUCTOS]->(televisores),
    (smartphones)-[:CONTIENEN_PRODUCTOS]->(televisores)
  
  // Creación de productos
  
  //Laptops
  CREATE (p1:Producto {codigo: 'P01', nombre: 'Laptop Gamer', descripcion: 'Laptop potente para juegos', precio: 12000, stock: 5}),
   	(p2:Producto {codigo: 'P02', nombre: 'Laptop Dell', descripcion: 'Laptop dell', precio: 3000, stock: 8}),
  	(p3:Producto {codigo: 'P03', nombre: 'Laptop HP', descripcion: 'Ideal para estudiantes', precio: 900, stock: 10}),
  	(p4:Producto {codigo: 'P04', nombre: 'Asus TUF Gaming', descripcion: 'Ideal para gamers', precio: 1300, stock:0})
  //Smartphones
  CREATE (p5:Producto {codigo: 'P05', nombre: 'blackview shark 8', descripcion: 'Smartphone de alta gama para gamers', precio: 15000, stock: 20}),
         (p6:Producto {codigo: 'P06', nombre: 'Samungs A05s', descripcion: 'Smartphone Gama Baja', precio: 45000, stock: 15}),
         (p7:Producto {codigo: 'P07', nombre: 'Smartphone A04e', descripcion: 'Smartphone básico', precio: 1400, stock: 25})
         
  //Televisores
  CREATE (p8:Producto {codigo: 'P08', nombre: 'Televisor Smart TV Hinsense', descripcion: 'Televisor inteligente', precio: 750, stock: 3}),
         (p9:Producto {codigo: 'P09', nombre: 'Televisor UHD', descripcion: 'Televisor con resolución UDH', precio: 15000, stock: 0}),
         (p10:Producto {codigo: 'P010', nombre: 'Televisor UHD', descripcion: 'Televisor con resolución UDH', precio: 15000, stock: 0})
  RETURN p1,p2,p3,p4,p5,p6,p7,p8,p9,p10;
  // Creación de relaciones entre productos y categorías
  
  // Laptops
  MATCH (laptops:Categoria {nombre: 'Laptops'}),
        (p1:Producto {codigo: 'P01'}),
        (p2:Producto {codigo: 'P02'}),
        (p3:Producto {codigo: 'P03'}),
        (p4:Producto {codigo: 'P04'})
  CREATE (p1)-[:PERTENECE_A]->(laptops),
         (p2)-[:PERTENECE_A]->(laptops),
         (p3)-[:PERTENECE_A]->(laptops),
         (p4)-[:PERTENECE_A]->(laptops);
  
  // Smartphones
  MATCH (smartphones:Categoria {nombre: 'Smartphones'}),
        (p5:Producto {codigo: 'P05'}),
        (p6:Producto {codigo: 'P06'}),
        (p7:Producto {codigo: 'P07'})
  CREATE (p5)-[:PERTENECE_A]->(smartphones),
         (p6)-[:PERTENECE_A]->(smartphones),
         (p7)-[:PERTENECE_A]->(smartphones);
  
  // Televisores
  MATCH (televisores:Categoria {nombre: 'Televisores'}),
        (p8:Producto {codigo: 'P08'}),
        (p9:Producto {codigo: 'P09'}),
        (p10:Producto {codigo: 'P010'})
  CREATE (p8)-[:PERTENECE_A]->(televisores),
         (p9)-[:PERTENECE_A]->(televisores),
         (p10)-[:PERTENECE_A]->(televisores);
  
  //-------------------------------------------------------------------------
  // Creación de proveedores
  CREATE (prov1:Proveedor {id: 'PR001', nombre: 'Proveedor A', Ciudad: 'Tepic', telefono: '123456789', email: 'tpc@proveedora.com'}),
         (prov2:Proveedor {id: 'PR002', nombre: 'Proveedor B', Ciudad: 'Guadalajara', telefono: '987654321', email: 'gdl@proveedorb.com'}),
         (prov3:Proveedor {id: 'PR003', nombre: 'Proveedor C', Ciudad: 'Mazatlan', telefono: '987654576', email: 'mzt@proveedorc.com'}),
         (prov4:Proveedor {id: 'PR004', nombre: 'Proveedor D', Ciudad: 'Xalisco', telefono: '123456421', email: 'xsc@proveedord.com'}),
         (prov5:Proveedor {id: 'PR005', nombre: 'Proveedor E', Ciudad: 'Monterrey', telefono: '854679899', email: 'mty@proveedora.com'}),
         (prov6:Proveedor {id: 'PR006', nombre: 'Proveedor F', Ciudad: 'CDMX', telefono: '456679899', email: 'cdmx@proveedora.com'})
  return prov1,prov2,prov3,prov4,prov5,prov6;
  
  
  // Relación proveedores-productos
  
  
  
  // Proveedor A suministra algunos productos
  MATCH (prov1:Proveedor {id: 'PR001'}),
        (p1:Producto {codigo: 'P01'}),
        (p2:Producto {codigo: 'P02'}),
        (p6:Producto {codigo: 'P06'}),
        (p10:Producto {codigo: 'P010'})
  CREATE (prov1)-[:SUMINISTRA]->(p1),
         (prov1)-[:SUMINISTRA]->(p2),
         (prov1)-[:SUMINISTRA]->(p6),
         (prov1)-[:SUMINISTRA]->(p10);
  
  // Proveedor B suministra una categoría
  MATCH (prov2:Proveedor {id: 'PR002'}),
        (p5:Producto {codigo: 'P05'}),
        (p6:Producto {codigo: 'P06'}),
        (p7:Producto {codigo: 'P07'})
  CREATE (prov2)-[:SUMINISTRA]->(p5),
         (prov2)-[:SUMINISTRA]->(p6),
         (prov2)-[:SUMINISTRA]->(p7);
  
  // Proveedor C suministra otros productos
  MATCH (prov3:Proveedor {id: 'PR003'}),
        (p3:Producto {codigo: 'P03'}),
        (p8:Producto {codigo: 'P08'})
  CREATE (prov3)-[:SUMINISTRA]->(p3),
         (prov3)-[:SUMINISTRA]->(p8);
  
  // Proveedor D suministra otros productos
  MATCH (prov4:Proveedor {id: 'PR004'}),
        (p4:Producto {codigo: 'P04'}),
        (p7:Producto {codigo: 'P07'}),
        (p9:Producto {codigo: 'P09'})
  CREATE (prov4)-[:SUMINISTRA]->(p4),
         (prov4)-[:SUMINISTRA]->(p7),
         (prov4)-[:SUMINISTRA]->(p9);
  
  // Proveedor E suministra una categoria
  MATCH (prov5:Proveedor {id: 'PR005'}),
        (p8:Producto {codigo: 'P08'}),
        (p9:Producto {codigo: 'P09'}),
        (p10:Producto {codigo: 'P010'})
  CREATE (prov5)-[:SUMINISTRA]->(p8),
         (prov5)-[:SUMINISTRA]->(p9),
         (prov5)-[:SUMINISTRA]->(p10);
  // Proveedor F suministra una categoria
  MATCH (prov5:Proveedor {id: 'PR006'}),
        (p1:Producto {codigo: 'P01'}),
        (p2:Producto {codigo: 'P02'}),
        (p3:Producto {codigo: 'P03'}),
        (p4:Producto {codigo: 'P04'})
  CREATE (prov5)-[:SUMINISTRA]->(p1),
         (prov5)-[:SUMINISTRA]->(p2),
         (prov5)-[:SUMINISTRA]->(p3),
         (prov5)-[:SUMINISTRA]->(p4);
  
  //--------------------------------------------------------------------------------------
  //Creación de pedidos compra 
  
  
  // Creación de pedidos de compra
  CREATE (p1:PedidoCompra {
      id: 'PC001',
      fecha: '2024-10-05',
      cantidad: 1,
      valor: 30000,
      fecha_pedido: '2024-09-20',
      fecha_recepcion:'2024-09-25'
  })
  WITH p1
  MATCH (proveedor:Proveedor {nombre: 'Proveedor B'})
  CREATE (p1)-[:PEDIDO_REALIZADO_A]->(proveedor) 
  RETURN p1;
  
  CREATE (p2:PedidoCompra {
      id: 'PC002',
      fecha: '2024-10-10',
      valor: 15000,
      fecha_pedido: '2024-10-01',
      fecha_recepcion:'2024-10-10'
  })
  
  WITH p2
  MATCH (proveedor:Proveedor {nombre: 'Proveedor C'})
  CREATE (p2)-[:PEDIDO_REALIZADO_A]->(proveedor)
  RETURN p2;
  
  CREATE (p3:PedidoCompra {
      id: 'PC003',
      cantidad: 1,
      fecha: '2024-10-15',
      valor: 200000,
      fecha_pedido: '2024-10-03',
      fecha_recepcion: '2024-10-15'
  })
  WITH p3
  MATCH (proveedor:Proveedor {nombre: 'Proveedor D'})
  CREATE (p3)-[:PEDIDO_REALIZADO_A]->(proveedor)
  RETURN p3;
  
  CREATE (p4:PedidoCompra {
      id: 'PC004',
       cantidad: 2,
      fecha: '2024-10-20',
      valor: 250000,
      fecha_pedido: '2024-10-01',
      fecha_recepcion: '2024-10-10'
  })
  WITH p4
  MATCH (proveedor:Proveedor {nombre: 'Proveedor A'})
  CREATE (p4)-[:PEDIDO_REALIZADO_A]->(proveedor)
  RETURN p4;
  
  CREATE (p5:PedidoCompra {
      id: 'PC005',
      cantidad: 5,
      fecha: '2024-10-25',
      valor: 45000, 
      fecha_pedido: '2024-10-01',
      fecha_recepcion: '2024-10-10'
  })
  WITH p5
  MATCH (proveedor:Proveedor {nombre: 'Proveedor B'})
  CREATE (p5)-[:PEDIDO_REALIZADO_A]->(proveedor)
  RETURN p5;
  
  CREATE (p6:PedidoCompra {
      id: 'PC006',
      cantidad: 3,
      fecha: '2024-10-25',
      valor: 1500, 
      fecha_pedido: '2024-10-01',
      fecha_recepcion: '2024-10-10'
  })
  WITH p6
  MATCH (proveedor:Proveedor {nombre: 'Proveedor B'})
  CREATE (p6)-[:PEDIDO_REALIZADO_A]->(proveedor)
  RETURN p6;
  
  
  
  
  
  
  
  
  //----------------------------------------------------------------------------
  // Creación de pedidos de venta
  CREATE (pedidoVenta1:PedidoVenta {id: 'PV1', codigo: 'P001', nombre: 'Laptop Gamer', cantidad:1, fecha: '2023-10-01', precioTotal: 12000}),
         (pedidoVenta2:PedidoVenta {id: 'PV2', codigo: 'P002', nombre: 'Laptop Gamer,Televisor Smart TV Hinsense', cantidad:2, fecha: '2023-10-03', precioTotal: 24000}),
         (pedidoVenta3:PedidoVenta {id: 'PV3', codigo: 'P003', nombre: 'Laptop Gamer', cantidad:1, fecha: '2023-10-04', precioTotal: 12000}),
         (pedidoVenta4:PedidoVenta {id: 'PV4', codigo: 'P004', nombre: 'blackview shark 8', cantidad:1, fecha: '2023-10-05', precioTotal: 75000}),
         (pedidoVenta5:PedidoVenta {id: 'PV5', codigo: 'P005', nombre: 'Smartphone A05E', cantidad:1, fecha: '2023-10-02', precioTotal: 800}),
         (pedidoVenta6:PedidoVenta {id: 'PV6', codigo: 'P006', nombre: 'Smartphone A04E', cantidad:5, fecha: '2023-09-02', precioTotal: 55000}),
         (pedidoVenta7:PedidoVenta {id: 'PV7', codigo: 'P007', nombre: 'Laptop de Estudio', cantidad: 1, fecha: '2023-10-10', precioTotal: 12000}),
         (pedidoVenta8:PedidoVenta {id: 'PV8', codigo: 'P008', nombre: 'Smartphone AO4E', cantidad: 2, fecha: '2023-10-11', precioTotal: 2200});
  
  //Relaciones
  // Relación incluye producto para el Pedido de Venta 1
  MATCH (pedidoVenta1:PedidoVenta {id: 'PV1'}), 
        (p1:Producto {codigo: 'P01'}) // Laptop Gamer
        
  CREATE (pedidoVenta1)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 12000}]->(p1);
        
  
  // Relación incluye producto para el Pedido de Venta 2
  MATCH (pedidoVenta2:PedidoVenta {id: 'PV2'}), 
        (p2:Producto {codigo: 'P02'}), // Laptop Dell
        (p8:Producto {codigo: 'P08'}) // Televisor Smart TV Hinsense
  CREATE (pedidoVenta2)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 3000}]->(p2),
   (pedidoVenta2)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 750}]->(p8);;
  
  // Relación incluye producto para el Pedido de Venta 3
  MATCH (pedidoVenta3:PedidoVenta {id: 'PV3'}), 
        (p3:Producto {codigo: 'P03'}) // Laptop HP
  CREATE (pedidoVenta3)-[:INCLUYE_PRODUCTO {cantidad: 2, precio_unitario: 900}]->(p3);
  
  // Relación incluye producto para el Pedido de Venta 4
  MATCH (pedidoVenta4:PedidoVenta {id: 'PV4'}), 
        (p4:Producto {codigo: 'P04'}) // Asus TUF Gaming
  CREATE (pedidoVenta4)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 1300}]->(p4);
  
  // Relación incluye producto para el Pedido de Venta 5
  MATCH (pedidoVenta5:PedidoVenta {id: 'PV5'}), 
        (p5:Producto {codigo: 'P05'}) // blackview shark 8
  CREATE (pedidoVenta5)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 15000}]->(p5);
  
  // Relación incluye producto para el Pedido de Venta 6
  MATCH (pedidoVenta6:PedidoVenta {id: 'PV6'}), 
        (p6:Producto {codigo: 'P06'}) // Samungs A05s
  CREATE (pedidoVenta6)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 45000}]->(p6);
  
  // Relación incluye producto para el Pedido de Venta 7
  MATCH (pedidoVenta7:PedidoVenta {id: 'PV7'}), 
        (p7:Producto {codigo: 'P07'}) // Smartphone A04e
  CREATE (pedidoVenta7)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 1400}]->(p7);
  
  // Relación incluye producto para el Pedido de Venta 8
  MATCH (pedidoVenta8:PedidoVenta {id: 'PV8'}), 
        (p9:Producto {codigo: 'P09'}) // Televisor UHD
  CREATE (pedidoVenta8)-[:INCLUYE_PRODUCTO {cantidad: 1, precio_unitario: 15000}]->(p9);
  
  
  //----------------------------------------------------------------------------------------
  
  // Creación de clientes
  CREATE (cli1:Cliente {id: 'C001', nombre: 'Jose Lino', direccion: 'Calle 1', ciudad: 'Ciudad Tepic', telefono: '555123456', email: 'Juanl@mail.com'}),
         (cli2:Cliente {id: 'C002', nombre: 'Sophia Batista', direccion: 'Calle 2', ciudad: 'Ciudad Monterrey', telefono: '555654321', email: 'Sophiabt@mail.com'}),
         (cli3:Cliente {id: 'C003', nombre: 'Carlos Vallarta', direccion: 'Avenida 5', ciudad: 'Guadalajara', telefono: '555123456', email: 'Carlosv@mail.com'}),
         (cli4:Cliente {id: 'C004', nombre: 'María Hernandez', direccion: 'Calle 8', ciudad: 'CDMX', telefono: '555987654', email: 'Mariah@mail.com'}),
         (cli5:Cliente {id: 'C005', nombre: 'Jose Manuel', direccion: 'Calle 10', ciudad: 'Xalisco', telefono: '555321654', email: 'Jmanuel@mail.com'}),
         (cli6:Cliente {id: 'C006', nombre: 'Alejandra Ley', direccion: 'Calle 3', ciudad: 'Xalisco', telefono: '555654321', email: 'Aleley@mail.com'}),
         (cli7:Cliente {id: 'C007', nombre: 'Marlen Bugarin', direccion: 'Calle 4', ciudad: 'Tepic', telefono: '555987321', email: 'mbugarin@mail.com'}),
         (cli8:Cliente {id: 'C008', nombre: 'Maria Candelaria', direccion: 'Calle 5', ciudad: 'Tepic', telefono: '555321987', email: 'mariaCandelaria@mail.com'});
  
  
  // Relación clientes-pedidos de venta
  //Cliente 1
  MATCH (cli1:Cliente {id:'C001'}), 
  (pedidoVenta1:PedidoVenta {id: 'PV1'}) 
  CREATE (cli1)-[:HACE]->(pedidoVenta1);
  MATCH (cli1:Cliente {id:'C001'}), 
  (pedidoVenta6:PedidoVenta {id: 'PV6'})
  CREATE (cli1)-[:HACE]->(pedidoVenta6);
  
  
  //Cliente 2 
  MATCH (cli2:Cliente {id:'C002'}), 
  (pedidoVenta2:PedidoVenta {id: 'PV2'})
  CREATE (cli2)-[:HACE]->(pedidoVenta2);
  MATCH (cli2:Cliente {id:'C002'}), 
  (pedidoVenta1:PedidoVenta {id: 'PV1'}) 
  CREATE (cli2)-[:HACE]->(pedidoVenta1);
  MATCH (cli2:Cliente {id:'C002'}), 
  (pedidoVenta8:PedidoVenta {id: 'PV8'}) 
  CREATE (cli2)-[:HACE]->(pedidoVenta8);
  
  //Cliente 3
  MATCH (cli3:Cliente {id:'C003'}), 
  (pedidoVenta3:PedidoVenta {id: 'PV3'}) 
  CREATE (cli3)-[:HACE]->(pedidoVenta3);
  MATCH (cli3:Cliente {id:'C003'}), 
  (pedidoVenta1:PedidoVenta {id: 'PV1'}) 
  CREATE (cli3)-[:HACE]->(pedidoVenta1);
  MATCH (cli3:Cliente {id:'C003'}), 
  (pedidoVenta2:PedidoVenta {id: 'PV2'}) 
  CREATE (cli3)-[:HACE]->(pedidoVenta2);
  MATCH (cli3:Cliente {id:'C003'}), 
  (pedidoVenta7:PedidoVenta {id: 'PV7'}) 
  CREATE (cli3)-[:HACE]->(pedidoVenta7);
  
  
  
  //Cliente 4
  MATCH (cli4:Cliente {id: 'C004'}), 
  (pedidoVenta4:PedidoVenta {id: 'PV4'}) 
  CREATE (cli4)-[:HACE]->(pedidoVenta4);
  MATCH (cli4:Cliente {id: 'C004'}), 
  (pedidoVenta1:PedidoVenta {id: 'PV1'}) 
  CREATE (cli4)-[:HACE]->(pedidoVenta1);
  MATCH (cli4:Cliente {id: 'C004'}), 
  (pedidoVenta8:PedidoVenta {id: 'PV8'}) 
  CREATE (cli4)-[:HACE]->(pedidoVenta8);
  
  //Cliente 5
  MATCH (cli5:Cliente {id: 'C005'}), 
  (pedidoVenta5:PedidoVenta {id: 'PV5'}) 
  CREATE (cli5)-[:HACE]->(pedidoVenta5);
  MATCH (cli5:Cliente {id: 'C005'}), 
  (pedidoVenta1:PedidoVenta {id: 'PV1'}) 
  CREATE (cli5)-[:HACE]->(pedidoVenta1);
  MATCH (cli5:Cliente {id: 'C005'}), 
  (pedidoVenta4:PedidoVenta {id: 'PV4'}) 
  CREATE (cli5)-[:HACE]->(pedidoVenta4);
  
  
  //Cliente 6
  MATCH (cli6:Cliente {id: 'C006'}),  
  (pedidoVenta6:PedidoVenta {id: 'PV6'}) 
  CREATE (cli6)-[:HACE]->(pedidoVenta6);
  MATCH (cli6:Cliente {id:'C006'}), 
  (pedidoVenta2:PedidoVenta {id: 'PV2'})
  CREATE (cli6)-[:HACE]->(pedidoVenta2);
  MATCH (cli6:Cliente {id:'C006'}), 
  (pedidoVenta5:PedidoVenta {id: 'PV5'})
  CREATE (cli6)-[:HACE]->(pedidoVenta5);
  
  
  //Cliente 7
  MATCH (cli7:Cliente {id:'C007'}), 
  (pedidoVenta7:PedidoVenta {id: 'PV7'})
  CREATE (cli7)-[:HACE]->(pedidoVenta7);
  MATCH (cli7:Cliente {id:'C007'}), 
  (pedidoVenta2:PedidoVenta {id: 'PV2'})
  CREATE (cli7)-[:HACE]->(pedidoVenta2);
  MATCH (cli7:Cliente {id: 'C007'}), 
  (pedidoVenta1:PedidoVenta {id: 'PV1'}) 
  CREATE (cli7)-[:HACE]->(pedidoVenta1);
  
  //Cliente 8
  MATCH (cli8:Cliente {id: 'C008'}),  
  (pedidoVenta8:PedidoVenta {id: 'PV8'}) 
  CREATE (cli8)-[:HACE]->(pedidoVenta8);
  MATCH (cli8:Cliente {id:'C008'}), 
  (pedidoVenta4:PedidoVenta {id: 'PV4'})
  CREATE (cli8)-[:HACE]->(pedidoVenta4);
  MATCH (cli8:Cliente {id:'C008'}), 
  (pedidoVenta6:PedidoVenta {id: 'PV6'})
  CREATE (cli8)-[:HACE]->(pedidoVenta6);
  
  
  
  //------------------------------------------------------------------------------
  // Creación de devoluciones
  CREATE (devolucion1:Devolucion {id: 'DV1', fecha: '2023-10-05', motivo: 'Falla de fábrica', reembolso: true}),
         (devolucion2:Devolucion {id: 'DV2', fecha: '2023-10-06', motivo: 'Producto defectuoso', reembolso: false});
  
  // Relación devoluciones-pedidos de venta
  MATCH (pedidoVenta1:PedidoVenta {id: 'PV1'}), (devolucion1:Devolucion {id: 'DV1'})
  CREATE (devolucion1)-[:PERTENECE_A]->(pedidoVenta1);
  MATCH (cli6:Cliente {id:'C006'}), (devolucion1:Devolucion {id: 'DV1'})
  CREATE (devolucion1)-[:PERTENECE_A]->(cli6);
  
  
  
  MATCH (pedidoVenta3:PedidoVenta {id: 'PV3'}), (devolucion2:Devolucion {id: 'DV2'})
  CREATE (devolucion2)-[:PERTENECE_A]->(pedidoVenta3);
  MATCH (cli3:Cliente {id:'C003'}), (devolucion2:Devolucion {id: 'DV2'})
  CREATE (devolucion2)-[:PERTENECE_A]->(cli3);
  ```

**1.4 ENDPOINTS** 💻

 Archivo de backend en postaman [02_ne4j_api_app.postman_collection.json ](https://github.com/cesarurielhr/02_neo4j_api/blob/main/02_ne4j_api_app.postman_collection.json)
 
 Para poder utilizar el archivo deberas importarlo a postman para probar las 12 querys selecionadas de las cuales 1 fue creada para complentar el requerimento 
 este query debe de ejecutarse antes de las demas querys. 
- Q0. Agregar un nuevo provedor y los podructos que sumistra
> [!NOTE]
> Para agregar productos se debe pasar el ID del producto que queramos asignar al proveedor
  ```
  http://localhost:3000/api/proveedores
  ```
  body:
  ```
     {"id":"PRO008",
     "nombre":"nextel",
     "ciudad":"Tepic",
     "telefono":35464564,
     "email":"nextel@hotmail.com",
     "productos":["P06","P07","P09"]
     }
  ```
- Q1.obtener la lista de productos que tienen menos de 10 unidades en stock.

  ```
  /GET http://localhost:3000/api/productos/stock/10
- Q2. Encontrar los proveedores que suministran productos de una categoría en específico..
  ```
  /GET http://localhost:3000/api/provedores/categoria/Smartphones
  ```
- Q3. Obtener la lista de pedidos de compra que fueron realizados a un proveedor en específico..
  ```
  /GET http://localhost:3000/api/proveedores/PR002
  ```
- Q5.obtener la lista de productos que tienen menos de 10 unidades en stock.
  ```
  /GET http://localhost:3000/api/proveedores
  ```
- Q8. Cambiar todos los productos suministrados por un proveedor a otro proveedor.
  ```
  /PUT http://localhost:3000/api/proveedores/PR002
  ```
  body:
    ```
    {
     "idproveedorn":"PR005"
    }
    ```
- Q10. Encontrar los productos que se encuentran agotados (sin stock) en el inventario.
  ```
  /GET http://localhost:3000/api/productos/sinstock
  ```
- Q11. Obtener la lista de clientes.
  ```
  /GET http://localhost:3000/api/clientes
  ```
- Q12. Eliminar todos los proveedores y sus nodos asociados..
  ```
  /DELETE http://localhost:3000/api/proveedores/delete
  ```  
- Q13. Todos los productos de una categoría específica eliminados del inventario.
  ```
  /DELETE http://localhost:3000/api/categoria/Laptops
  ```
- Q14. Todos los pedidos de compra de un proveedor en particular son transferidos a otro proveedor por un cambio de contrato.
  ```
  /PUT http://localhost:3000/api/proveedores/otro/PR002
  ```
  body:
  ```
     {
       "idproveedorn":"PR005"
     }
  ```
-  Q15. Eliminar todos los clientes que han realizado devoluciones..
   ```  
   /DELETE http://localhost:3000/api/cliente/devoluciones
   ```

**1.5 Querys** 🕸️

Los siguiente fueron realizado con lenguaje cypher para NEO4J siendo las querys solitadas para el laboratorio


- Q01. Obtener la lista de productos que tienen menos de 10 unidades en stock.
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
