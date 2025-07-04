CREATE DATABASE vtaszfs;
USE vtaszfs;

CREATE TABLE IF NOT EXISTS clientes(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(20),
	last_name VARCHAR(20),
	email VARCHAR(40)
)ENGINE=INNODB;


CREATE TABLE IF NOT EXISTS puestos(
	id INT AUTO_INCREMENT PRIMARY KEY,
	description TEXT
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS countries(
	id INT AUTO_INCREMENT PRIMARY KEY,
	country_name VARCHAR(20) UNIQUE
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS regions(
	id INT AUTO_INCREMENT PRIMARY KEY,
	region_id VARCHAR(20) UNIQUE,
	country_id INT(11),
	CONSTRAINT FK_country_id FOREIGN KEY (country_id) REFERENCES countries(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS cities(
	id INT AUTO_INCREMENT PRIMARY KEY,
	city_name VARCHAR(20),
	region_id INT(11),
	CONSTRAINT FK_region_id FOREIGN KEY (region_id) REFERENCES regions(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS codigo_postal(
	id INT AUTO_INCREMENT PRIMARY KEY,
	description VARCHAR(10),
	city_id INT(11),
	CONSTRAINT FK_city_id FOREIGN KEY (city_id) REFERENCES cities(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS empleados(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(20),
	last_name VARCHAR(20)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS proveedores(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(20),
	last_name VARCHAR(20)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS direcciones(
	id INT AUTO_INCREMENT PRIMARY KEY,
	calle VARCHAR(30),
	city_id INT(11),
	region_id INT(11),
	CONSTRAINT FK_cityid FOREIGN KEY (city_id) REFERENCES cities(id),
	CONSTRAINT FK_regionid FOREIGN KEY (region_id) REFERENCES regions(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS datos_empelados(
	id INT AUTO_INCREMENT PRIMARY KEY,
	empleado_id INT(11),
	puesto_id INT(11),
	salario DECIMAL(10, 2),
	fecha_contratacion DATE,
	CONSTRAINT FK_empleado_id FOREIGN KEY (empleado_id) REFERENCES empleados(id),
	CONSTRAINT FK_puesto_id FOREIGN KEY (puesto_id) REFERENCES puestos(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS ubicaciones(
	id INT AUTO_INCREMENT PRIMARY KEY,
	cliente_id INT(11),
	direccion_id INT(11),
	proveedor_id INT(11),
	empleado_id INT(11),
	CONSTRAINT FK_cliente_id FOREIGN KEY (cliente_id) REFERENCES clientes(id),
	CONSTRAINT FK_direccion_id FOREIGN KEY (direccion_id) REFERENCES direcciones(id),
	CONSTRAINT FK_proveedor_id FOREIGN KEY (proveedor_id) REFERENCES proveedores(id),
	CONSTRAINT FK_Empleadoid FOREIGN KEY (empleado_id) REFERENCES empleados(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS tipos_producto(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(20)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS productos(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(20),
	precio DECIMAL(10, 2),
	tipoproducto_id INT(11),
	proveedor_id INT(11),
	CONSTRAINT FK_tipoproducto_id FOREIGN KEY (tipoproducto_id) REFERENCES tipos_producto(id),
	CONSTRAINT FK_proveedorid FOREIGN KEY (proveedor_id) REFERENCES proveedores(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS historial_pedido(
	id INT AUTO_INCREMENT PRIMARY KEY,
	pedido_id INT(11),
	fecha_cambio DATETIME,
	CONSTRAINT FK_pedido_id FOREIGN KEY (pedido_id) REFERENCES pedidos(id)
)ENGINE=INNODB;

CREATE TABLE pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    fecha DATE,
    empleado_id INT,
    CONSTRAINT FK_clienteid FOREIGN KEY (cliente_id) REFERENCES clientes(id),
    CONSTRAINT FK_empleados_id FOREIGN KEY (empleado_id) REFERENCES empleados(id)
) ENGINE=INNODB;
CREATE TABLE detalle_pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id INT,
    producto_id INT,
    cantidad INT,
    precio DECIMAL(10, 2),
    CONSTRAINT FK_pedido_id FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    CONSTRAINT FK_producto_id FOREIGN KEY (producto_id) REFERENCES productos(id)
) ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS contacto_proveedores(
	id INT AUTO_INCREMENT PRIMARY KEY,
	email VARCHAR(40),
	proveedor_id INT(11),
	CONSTRAINT FK_Proveedores_id FOREIGN KEY (proveedor_id) REFERENCES proveedores(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS telefono_proveedor(
	id INT AUTO_INCREMENT PRIMARY KEY,
	numero VARCHAR(20),
	contactoproveedor_id INT(11),
	CONSTRAINT FK_contactoproveedor_id FOREIGN KEY (contactoproveedor_id) REFERENCES contacto_proveedores(id)
)ENGINE=INNODB;

CREATE TABLE IF NOT EXISTS telefono_cliente(
	id INT AUTO_INCREMENT PRIMARY KEY,
	numero VARCHAR(20),
	cliente_id INT(11),
	CONSTRAINT FK_Clientes_id FOREIGN KEY (cliente_id) REFERENCES clientes(id)
)ENGINE=INNODB;

2.JOIN

1.
INSERT INTO clientes(id, name, last_name, email) VALUES
(1, 'Ivanna', 'Lopez', 'ivanna.lopez@email.com'),
(2, 'Juliana', 'Perez', 'juliana.perez@email.com'),
(3, 'Lucia', 'Martinez', 'lucia.martinez@email.com'),
(4, 'Fabio', 'Gomez', 'fabio.gomez@email.com'),
(5, 'Simon', 'Torres', 'simon.torres@email.com'),
(6, 'Andres', 'Ramirez', 'andres.ramirez@email.com'),
(7, 'Juan', 'Moreno', 'juan.moreno@email.com'),
(8, 'Viviana', 'Diaz', 'viviana.diaz@email.com'),
(9, 'Louis', 'Smith', 'louis.smith@email.com'),
(10, 'Harry', 'Clark', 'harry.clark@email.com'),
(11, 'Zayn', 'Johnson', 'zayn.johnson@email.com'),
(12, 'Niall', 'Brown', 'niall.brown@email.com');



INSERT INTO empleados(id, name, last_name) VALUES
(101, 'Mario', 'Rojas'),
(102, 'Pedro', 'Hernandez'),
(103, 'Sofia', 'Castillo'),
(104, 'Laura', 'Mejia');



INSERT INTO pedidos (cliente_id, fecha, empleado_id) VALUES
(1, '2024-06-01', 102),
(2, '2024-06-02', 103),
(3, '2024-06-03', 102),
(4, '2024-06-04', 102),
(5, '2024-06-05', 103),
(6, '2024-06-06', 101),
(7, '2024-06-07', 101),
(8, '2024-06-08', 102),
(9, '2024-06-09', 103),
(10, '2023-08-10', 104),
(11, '2025-06-11', 103);


SELECT p.id AS pedido_id, p.fecha, c.name AS nombre_cliente, e.name AS nombre_empleado
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id 
INNER JOIN empleados e ON p.empleado_id = e.id;

2.

INSERT INTO proveedores (id, name, last_name) VALUES
(1, 'Paula', 'Fernandez'),
(2, 'Miguel', 'Lopez'),
(3, 'Johlver', 'Rodriguez');


INSERT INTO tipos_producto(name) VALUES ('Ropa'), ('Electrodomesticos'), ('Alimentos');

INSERT INTO productos(name, precio, tipoproducto_id, proveedor_id) VALUES 
('Pantalon', 30.99, 1, 3),
('Camiseta', 19.99, 1, 2),
('Jeans', 29.99, 1, 2),
('Falda', 25.99, 1, 1),
('Chaqueta', 59.99, 1, 3),
('Vestido', 45.50, 1, 2),
('Blusa', 34.90, 1, 1),
('Televisor', 499.99, 2, 1),
('Smartphone', 299.99, 2, 1),
('Tablet', 199.99, 2, 1),
('Microondas', 95.99, 2, 1),
('Lavadora', 98.99, 2, 3),
('Batidora', 98.99, 2, 1),
('Licuadora', 65.50, 2, 2),
('Aspiradora', 150.00, 2, 3),
('Arroz', 3.50, 3, 3),
('Carne', 5.99, 3, 1),
('Pescado', 6.99, 3, 2),
('Pan', 2.50, 3, 3),
('Huevos', 4.20, 3, 2),
('Leche', 3.10, 3, 1),
('Frijoles', 2.90, 3, 3),
('Aceite', 6.25, 3, 2);


SELECT productos.name AS producto, proveedores.name AS proveedor
FROM productos
INNER JOIN proveedores ON productos.proveedor_id = proveedores.id;

SELECT pr.id, pr.name AS producto, proveedores.name AS proveedor
FROM productos pr
INNER JOIN proveedores ON pr.proveedor_id = proveedores.id;

3.

INSERT INTO countries (country_name) VALUES 
('Colombia'), ('Mexico'), ('Argentina');

INSERT INTO regions (region_id, country_id) VALUES 
('Andina', 1), ('Caribe', 1),
('Norte', 2), ('Sureste', 2),
('Pampeana', 3), ('Patagonia', 3);

INSERT INTO cities(city_name, region_id) VALUES 
('Bogota',1), ('medellin',1),('Barranquilla',2), 
('Monterrey',3),('Merida',4),('Tijuana',3),
('Buenos Aires',5),('Boriloche',6),('Rosario',5);

INSERT INTO codigo_postal (description, city_id) VALUES 
('110111',1), ('050021', 2),('080001',3),
('06000',4), ('97000',5), ('22000', 6),
('1000',7), ('8400',8), ('2000',9);

INSERT INTO direcciones (calle, city_id, region_id) VALUES
('Calle 10 #5-15', 1, 1),('Carrera 45 #10-20', 2, 1),('Avenida del Mar #30-50', 3, 2),
('Av. Constitución 1000', 4, 3),('Calle 60 x 55 y 57', 5, 4),('Blvd. Agua Caliente 1234', 6, 3),
('Av. 9 de Julio 1500', 7, 5),('Ruta 40 km 5000', 8, 6),('Calle Córdoba 123', 9, 5);

INSERT INTO ubicaciones (cliente_id, direccion_id, proveedor_id) VALUES
(1, 1, 2),
(2, 2,3), 
(3, 3,1),  
(4, 4,1),  
(5, 5,2),  
(6, 6,3),  
(7, 7,3),  
(8, 8,1), 
(9, 9,2);  




SELECT 
    p.id AS pedido_id,
    p.fecha,
    c.name AS nombre_cliente,
    c.last_name AS apellido_cliente,
    cp.description AS codigo_postal,
    d.calle AS direccion,
    ci.city_name AS ciudad,
    r.region_id AS region,
    co.country_name AS pais
FROM 
    pedidos p
INNER JOIN 
    clientes c ON p.cliente_id = c.id
LEFT JOIN 
    ubicaciones u ON c.id = u.cliente_id
LEFT JOIN 
    direcciones d ON u.direccion_id = d.id
LEFT JOIN 
    cities ci ON d.city_id = ci.id
LEFT JOIN 
    codigo_postal cp ON ci.id = cp.city_id
LEFT JOIN 
    regions r ON d.region_id = r.id
LEFT JOIN 
    countries co ON r.country_id = co.id
ORDER BY 
    p.fecha DESC;

4.

INSERT INTO puestos (description) VALUES 
('Vendedor'),
('Gerente'),
('Asistente'), 
('Repartidor'),
('Supervisor'),
('Administrador');

INSERT INTO datos_empelados (empleado_id, puesto_id, salario, fecha_contratacion) VALUES
(101, 1, 3500.00, '2020-05-15'),
(102, 2, 5000.00, '2019-03-10'),
(103, 4, 2300.00, '2021-01-20'),
(104, 3, 2900.00, '2022-04-25'),
(105, 1, 3600.00, '2021-07-01'),
(106, 1, 3700.00, '2022-03-18'),
(107, 2, 5200.00, '2018-11-10'),
(108, 3, 2950.00, '2023-01-15'),
(109, 5, 4100.00, '2020-09-22'),
(110, 6, 4700.00, '2019-12-05');



SELECT e.id, e.name, p.id AS pedido_id 
FROM empleados e 
LEFT JOIN pedidos p ON e.id = p.empleado_id; 

5.

SELECT 
    tp.name AS tipo_producto,
    p.name AS nombre_producto
FROM 
    productos p
INNER JOIN 
    tipos_producto tp ON p.tipoproducto_id = tp.id;

6.

SELECT 
    c.id AS cliente_id,
    c.name AS nombre_cliente,
    COUNT(p.id) AS total_pedidos
FROM 
    clientes c
LEFT JOIN 
    pedidos p ON c.id = p.cliente_id
GROUP BY 
    c.id, c.name
ORDER BY 
    total_pedidos DESC;

7.

SELECT 
    p.id AS pedido_id,
    p.fecha,
    e.id AS empleado_id,
    e.name AS nombre_empleado
FROM 
    pedidos p
INNER JOIN 
    empleados e ON p.empleado_id = e.id
ORDER BY 
    p.fecha;

8.

INSERT INTO detalle_pedidos (pedido_id, producto_id, cantidad, precio) VALUES
(1, 1, 2, 30.99),
(2, 2, 1, 499.99), 
(3, 3, 3, 19.99), 
(4, 4, 5, 3.50),  
(5, 5, 1, 299.99),
(6, 6, 2, 29.99),  
(7, 7, 3, 5.99),
(8, 8, 6, 199.99),
(9, 9, 1, 25.99),
(10, 10, 4, 6.99), 
(11, 11, 7, 95.99),
(12, 12, 2, 98.99), 
(13, 13, 1, 98.99),  
(14, 14, 2, 65.50),  
(15, 15, 5, 150.00),
(16, 16, 10, 2.50), 
(17, 17, 12, 4.20),  
(18, 18, 6, 3.10), 
(19, 19, 4, 2.90), 
(20, 20, 3, 6.25); 


SELECT 
    p.id AS producto_id,
    p.name AS nombre_producto,
    p.precio
FROM 
    detalle_pedidos dp
RIGHT JOIN 
    productos p ON dp.producto_id = p.id
WHERE 
    dp.id IS NULL;

9. 

SELECT 
    c.id AS cliente_id,
    c.name AS nombre_cliente,
    COUNT(p.id) AS total_pedidos,
    d.calle AS direccion,
    ci.city_name AS ciudad,
    r.region_id AS region,
    co.country_name AS pais
FROM 
    clientes c
LEFT JOIN 
    pedidos p ON c.id = p.cliente_id
LEFT JOIN 
    ubicaciones u ON c.id = u.cliente_id
LEFT JOIN 
    direcciones d ON u.direccion_id = d.id
LEFT JOIN 
    cities ci ON d.city_id = ci.id
LEFT JOIN 
    regions r ON d.region_id = r.id
LEFT JOIN 
    countries co ON r.country_id = co.id
GROUP BY 
    c.id, c.name, d.calle, ci.city_name, r.region_id, co.country_name
ORDER BY 
    total_pedidos DESC;

10.

SELECT 
    p.id AS producto_id,
    p.name AS nombre_producto,
    p.precio,
    tp.name AS tipo_producto,
    pr.name AS nombre_proveedor
FROM productos p
INNER JOIN tipos_producto tp ON p.tipoproducto_id = tp.id
INNER JOIN proveedores pr ON p.proveedor_id = pr.id
ORDER BY tp.name, p.name;


3. CONSULTAS SIMPLES

1.
SELECT id, name, precio 
FROM productos 
WHERE precio > 50;

2.
SELECT c.id, c.name, ci.city_name 
FROM clientes c
JOIN ubicaciones u ON c.id = u.cliente_id
JOIN direcciones d ON u.direccion_id = d.id
JOIN cities ci ON d.city_id = ci.id
WHERE ci.city_name = 'Bogota';

3. 

SELECT e.id,e.name,de.fecha_contratacion
FROM empleados e
JOIN datos_empelados de ON e.id = de.empleado_id
WHERE de.fecha_contratacion >= DATE_SUB(CURDATE(), INTERVAL 2 YEAR)
ORDER BY de.fecha_contratacion DESC;

4.
SELECT 
    p.id AS id_proveedor,
    p.name AS nombre_Proveedor,
    COUNT(pr.id) AS Total_Productos
FROM 
    proveedores p
INNER JOIN 
    productos pr ON p.id = pr.proveedor_id
GROUP BY 
    p.id, p.name
HAVING 
    COUNT(pr.id) > 5
ORDER BY 
    Total_Productos DESC;

5.

SELECT 
    c.id AS id_cliente,
    c.name AS nombre_cliente
FROM 
    clientes c
LEFT JOIN 
    ubicaciones u ON c.id = u.cliente_id
WHERE 
    u.cliente_id IS NULL;

6.

SELECT 
    dp.id AS detalle_id,
    pr.name AS producto,
    dp.cantidad,
    dp.precio,
    (dp.cantidad * dp.precio) AS subtotal
FROM 
    detalle_pedidos dp
JOIN 
    productos pr ON dp.producto_id = pr.id
ORDER BY 
    dp.id;

7.

SELECT 
    p.description AS puesto,
    COUNT(de.empleado_id) AS empleados,
    ROUND(AVG(de.salario)) AS promedio,
    MIN(de.salario) AS minimo,
    MAX(de.salario) AS maximo
FROM 
    puestos p
LEFT JOIN 
    datos_empelados de ON p.id = de.puesto_id
GROUP BY 
    p.description
ORDER BY 
    promedio DESC;

8.

SELECT 
    tp.id AS codigo,
    tp.name AS tipo_producto,
    COUNT(p.id) AS cantidad_productos
FROM 
    tipos_producto tp
LEFT JOIN 
    productos p ON tp.id = p.tipoproducto_id
GROUP BY 
    tp.id, tp.name
ORDER BY 
    tp.name;

9.
SELECT 
    id AS codigo,
    name AS producto,
    precio
FROM 
    productos
ORDER BY 
    precio DESC
LIMIT 3;

10.

SELECT 
    c.id AS id_cliente,
    c.name AS nombre_cliente,
    COUNT(p.id) AS total_pedidos
FROM 
    clientes c
JOIN 
    pedidos p ON c.id = p.cliente_id
GROUP BY 
    c.id, c.name
ORDER BY 
    total_pedidos DESC
LIMIT 1;


-4. CONSULTAS MULTITABLA

1.
SELECT 
    p.id AS pedido_id,
    p.fecha,
    c.id AS cliente_id,
    c.name AS nombre_cliente,
    pr.name AS producto
FROM 
    pedidos p
JOIN 
    clientes c ON p.cliente_id = c.id
JOIN 
    detalle_pedidos dp ON p.detallepedido_id = dp.id  -- Cambio clave aquí
JOIN 
    productos pr ON dp.producto_id = pr.id
ORDER BY 
    p.fecha DESC, p.id;

2.
SELECT 
    p.id AS pedido_id,
    p.fecha,
    c.name AS cliente,
    d.calle AS direccion,
    ci.city_name AS ciudad,
    r.region_id AS region,
    co.country_name AS pais
FROM 
    pedidos p
JOIN 
    clientes c ON p.cliente_id = c.id
JOIN 
    ubicaciones u ON c.id = u.cliente_id
JOIN 
    direcciones d ON u.direccion_id = d.id
JOIN 
    cities ci ON d.city_id = ci.id
JOIN 
    regions r ON d.region_id = r.id
JOIN 
    countries co ON r.country_id = co.id
ORDER BY 
    p.fecha DESC;

3.

SELECT 
    pr.id AS producto_id,
    pr.name AS producto,
    pr.precio,
    tp.name AS tipo_producto,
    pv.name AS proveedor
FROM 
    productos pr
JOIN 
    tipos_producto tp ON pr.tipoproducto_id = tp.id
JOIN 
    proveedores pv ON pr.proveedor_id = pv.id
ORDER BY 
    tp.name, pr.name;

4.
SELECT DISTINCT
    e.id AS empleado_id,
    e.name AS nombre_empleado,
    ci.city_name AS ciudad
FROM 
    empleados e
JOIN 
    pedidos p ON e.id = p.empleado_id
JOIN 
    clientes c ON p.cliente_id = c.id
JOIN 
    ubicaciones u ON c.id = u.cliente_id
JOIN 
    direcciones d ON u.direccion_id = d.id
JOIN 
    cities ci ON d.city_id = ci.id
WHERE 
    ci.city_name = 'Bogota'
ORDER BY 
    e.name;

5.
SELECT 
    p.id AS producto_id,
    p.name AS producto,
    SUM(dp.cantidad) AS total_vendido
FROM 
    detalle_pedidos dp
JOIN 
    productos p ON dp.producto_id = p.id
GROUP BY 
    p.id, p.name
ORDER BY 
    total_vendido DESC
LIMIT 5;

6.
SELECT 
    c.id AS cliente_id,
    c.name AS nombre_cliente,
    ci.city_name AS ciudad,
    COUNT(p.id) AS total_pedidos
FROM 
    clientes c
JOIN 
    pedidos p ON c.id = p.cliente_id
JOIN 
    ubicaciones u ON c.id = u.cliente_id
JOIN 
    direcciones d ON u.direccion_id = d.id
JOIN 
    cities ci ON d.city_id = ci.id
GROUP BY 
    c.id, c.name, ci.city_name
ORDER BY 
    total_pedidos DESC;

7.
SELECT 
    c.name AS cliente,
    pv.name AS proveedor,
    ci.city_name AS ciudad
FROM 
    clientes c
JOIN 
    ubicaciones uc ON c.id = uc.cliente_id
JOIN 
    direcciones dc ON uc.direccion_id = dc.id
JOIN 
    cities ci ON dc.city_id = ci.id
JOIN 
    proveedores pv
JOIN 
    ubicaciones up ON pv.id = up.proveedor_id
JOIN 
    direcciones dp ON up.direccion_id = dp.id
WHERE 
    dp.city_id = dc.city_id
ORDER BY 
    ci.city_name, c.name;

8.

SELECT 
    tp.name AS tipo_producto,
    SUM(dp.cantidad * dp.precio) AS total_ventas
FROM 
    detalle_pedidos dp
JOIN 
    productos p ON dp.producto_id = p.id
JOIN 
    tipos_producto tp ON p.tipoproducto_id = tp.id
GROUP BY 
    tp.name
ORDER BY 
    total_ventas DESC;

9.

SELECT 
    p.id AS pedido_id,
    p.fecha,
    c.name AS cliente,
    e.name AS empleado,
    pr.name AS producto,
    pv.name AS proveedor,
    dp.cantidad,
    dp.precio
FROM 
    pedidos p
JOIN 
    clientes c ON p.cliente_id = c.id
JOIN 
    empleados e ON p.empleado_id = e.id
JOIN 
    detalle_pedidos dp ON p.detallepedido_id = dp.id
JOIN 
    productos pr ON dp.producto_id = pr.id
JOIN 
    proveedores pv ON pr.proveedor_id = pv.id
ORDER BY 
    p.fecha;

10.

SELECT 
    pv.id AS proveedor_id,
    pv.name AS proveedor,
    SUM(dp.cantidad * dp.precio) AS ingreso_total
FROM 
    proveedores pv
JOIN 
    productos pr ON pv.id = pr.proveedor_id
JOIN 
    detalle_pedidos dp ON pr.id = dp.producto_id
GROUP BY 
    pv.id, pv.name
ORDER BY 
    ingreso_total DESC;
    
    
    
-5. SUBCONSULTAS  

1.
SELECT p.name AS producto, p.precio, tp.name AS categoria
FROM productos p
JOIN tipos_producto tp ON p.tipoproducto_id = tp.id
WHERE p.precio = (
SELECT MAX(pr.precio)
FROM productos pr
WHERE pr.tipoproducto_id = p.tipoproducto_id);

2.
SELECT c.name, SUM(dp.cantidad * dp.precio) AS total_compras
FROM clientes c
JOIN pedidos p ON c.id = p.cliente_id
JOIN detalle_pedidos dp ON p.id = dp.pedido_id
GROUP BY c.id, c.name
HAVING total_compras = (
    SELECT MAX(total_por_cliente)
    FROM (
        SELECT pd.cliente_id, SUM(dpd.cantidad * dpd.precio) AS total_por_cliente
        FROM pedidos pd
        JOIN detalle_pedidos dpd ON pd.id = dpd.pedido_id
        GROUP BY pd.cliente_id
    ) AS subconsulta
);

3. Listar empleados que ganan más que el salario promedio.
SELECT e.name 
FROM empleados e
JOIN datos_empelados de ON e.id = de.empleado_id
WHERE de.salario > (
    SELECT AVG(dem.salario)
    FROM datos_empelados dem
);

4. Consultar productos que han sido pedidos más de 5 veces.
SELECT p.name 
FROM productos p
WHERE (
    SELECT SUM(dp.cantidad)
    FROM  detalle_pedidos dp 
    WHERE dp.producto_id = p.id
)>5;

5. Listar pedidos cuyo total es mayor al promedio de todos los pedidos.
SELECT dp.id AS detalle_pedido_id, p.id AS pedido_id, dp.producto_id, dp.cantidad, dp.precio, 
    (dp.cantidad * dp.precio) AS total
FROM pedidos p
JOIN detalle_pedidos dp ON dp.pedido_id = p.id
WHERE (dp.cantidad * dp.precio) > (
    SELECT AVG(dpe.cantidad * dpe.precio)
    FROM detalle_pedidos dpe
);

6.
SELECT id, name AS nombre_proveedor, total_productos
FROM (
    SELECT  p.id, p.name, COUNT(pr.id) AS total_productos
    FROM  proveedores p
    JOIN productos pr ON p.id = pr.proveedor_id
    GROUP BY p.id, p.name
) AS subconsulta
ORDER BY total_productos DESC
LIMIT 3;

7. Consultar productos con precio superior al promedio en su tipo.
SELECT p.name AS producto, p.precio, tp.name AS tipo_producto
FROM productos p
JOIN tipos_producto tp ON p.tipoproducto_id = tp.id
WHERE p.precio > (
    SELECT AVG(pr.precio)
    FROM productos pr
    WHERE pr.tipoproducto_id = p.tipoproducto_id
);

8.
SELECT c.id AS cliente_id,c.name AS nombre_cliente,COUNT(p.id) AS total_pedidos
FROM clientes c
JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.id, c.name
HAVING COUNT(p.id) > (
    SELECT AVG(total)
    FROM (
    SELECT cliente_id, COUNT(p.id) AS total
    FROM pedidos p
    GROUP BY cliente_id
) AS pedidos_por_cliente
);


9.  Encontrar productos cuyo precio es mayor que el promedio de todos los productos.
SELECT p.name AS producto , p.precio
FROM productos p
WHERE p.precio >(
    SELECT AVG(pr.precio)
    FROM productos pr
);

10. Mostrar empleados cuyo salario es menor al promedio del departamento.
SELECT e.name AS empleado, de.salario, p.description AS puesto
FROM empleados e
JOIN datos_empelados de ON e.id = de.empleado_id
JOIN puestos p ON de.puesto_id = p.id
WHERE de.salario < (
    SELECT AVG(dep.salario)
    FROM datos_empelados dep
    WHERE dep.puesto_id = de.puesto_id
);

7. Procedimientos Almacenados

1.
DELIMITER $$

CREATE PROCEDURE actualizar_precio_proveedor (
    IN proveedor_id_input INT,
    IN porcentaje DECIMAL(5,2)
)
BEGIN
    UPDATE productos
    SET precio = precio + (precio * (porcentaje / 100))
    WHERE proveedor_id = proveedor_id_input;
END $$

DELIMITER ;

CALL actualizar_precio_proveedor(1, 10.00);
CALL actualizar_precio_proveedor(2, -15.00);

SELECT name, precio, proveedor_id
FROM productos
WHERE proveedor_id = 1;

2.
DELIMITER $$

CREATE PROCEDURE obtener_direccion_cliente (
    IN cliente_id INT
)
BEGIN
    SELECT 
        c.id AS cliente_id,
        c.name AS nombre_cliente,
        d.calle,
        ci.city_name AS ciudad,
        r.region_id AS region,
        co.country_name AS pais
    FROM clientes c
    JOIN ubicaciones u ON c.id = u.cliente_id
    JOIN direcciones d ON u.direccion_id = d.id
    JOIN cities ci ON d.city_id = ci.id
    JOIN regions r ON d.region_id = r.id
    JOIN countries co ON r.country_id = co.id
    WHERE c.id = cliente_id;
END $$

DELIMITER ;

CALL obtener_direccion_cliente(2);

3.

DELIMITER $$
CREATE PROCEDURE registrar_pedido_con_detalles (
    IN cliente_id INT,
    IN empleado_id INT,
    IN producto_id INT,
    IN cantidad INT,
    IN precio DECIMAL(10,2)
)
BEGIN
    DECLARE nuevo_pedido_id INT;
    INSERT INTO pedidos (cliente_id, fecha, empleado_id)
    VALUES (cliente_id, CURDATE(), empleado_id);
    SET nuevo_pedido_id = LAST_INSERT_ID();
    INSERT INTO detalle_pedidos (pedido_id, producto_id, cantidad, precio)
    VALUES (nuevo_pedido_id, producto_id, cantidad, precio);
END $$
DELIMITER ;

CALL registrar_pedido_con_detalles( 3, 102, 5, 2, 299.99);

SELECT id AS pedido_id, cliente_id, fecha, empleado_id
FROM pedidos
ORDER BY id DESC
LIMIT 1;

SELECT id AS detalle_id, pedido_id, producto_id, cantidad, precio
FROM detalle_pedidos
ORDER BY id DESC
LIMIT 1;

4.

DELIMITER $$
CREATE PROCEDURE total_ventas_cliente (
    IN cliente_id_input INT
)
BEGIN
    SELECT c.id AS cliente_id, c.name AS nombre_cliente, SUM(dp.cantidad * dp.precio) AS total_ventas
    FROM clientes c
    JOIN pedidos p ON c.id = p.cliente_id
    JOIN detalle_pedidos dp ON p.id = dp.pedido_id
    WHERE c.id = cliente_id_input
    GROUP BY c.id, c.name;
END $$
DELIMITER ;
CALL total_ventas_cliente(3);

5.
DELIMITER $$
CREATE PROCEDURE empleados_por_puesto (
    IN puesto_id INT
)
BEGIN
    SELECT e.id AS empleado_id, e.name AS nombre, e.last_name AS apellido, p.description AS puesto, de.salario
    FROM empleados e
    JOIN datos_empelados de ON e.id = de.empleado_id
    JOIN puestos p ON de.puesto_id = p.id
    WHERE p.id = puesto_id;
END $$
DELIMITER ;
CALL empleados_por_puesto(2);

6. Un procedimiento que actualice el salario de empleados por puesto.

DELIMITER $$
CREATE PROCEDURE actualizar_salario_empleados_puesto(
    IN puesto_id INT,
    IN porcentaje DECIMAL(10, 2)
)
BEGIN 
    UPDATE datos_empelados
    SET salario = salario + (salario * (porcentaje / 100))
    WHERE puesto_id = puesto_id;
END $$
DELIMITER ;

CALL actualizar_salario_empleados_puesto(1, 10.00);

7.
DELIMITER $$
CREATE PROCEDURE listar_pedidos_fechas (
    IN fecha_inicio DATE,
    IN fecha_fin DATE
)
BEGIN
    SELECT p.id AS pedido_id, p.fecha, c.name AS nombre_cliente, e.name AS nombre_empleado
    FROM pedidos p
    JOIN clientes c ON p.cliente_id = c.id
    JOIN empleados e ON p.empleado_id = e.id
    WHERE p.fecha BETWEEN fecha_inicio AND fecha_fin
    ORDER BY p.fecha;
END $$
DELIMITER ;

CALL listar_pedidos_fechas('2024-01-01', '2024-06-30');

8.

DELIMITER $$
CREATE PROCEDURE aplicar_descuento_categoria (
    IN categoria_id INT,
    IN descuento_porcentaje DECIMAL(10,2)
)
BEGIN
    UPDATE productos
    SET precio = precio - (precio * (descuento_porcentaje / 100))
    WHERE tipoproducto_id = categoria_id;
END $$
DELIMITER ;

CALL aplicar_descuento_categoria(2, 15.00);

SELECT id AS producto_id, name AS producto, precio, tipoproducto_id
FROM productos
WHERE tipoproducto_id = 2;

9.
DELIMITER $$
CREATE PROCEDURE listar_proveedores_por_categoria (
    IN tipo_producto_id INT
)
BEGIN
    SELECT DISTINCT pv.id AS proveedor_id, pv.name AS nombre_proveedor, tp.name AS tipo_producto
    FROM productos p
    JOIN proveedores pv ON p.proveedor_id = pv.id
    JOIN tipos_producto tp ON p.tipoproducto_id = tp.id
    WHERE p.tipoproducto_id = tipo_producto_id;
END $$
DELIMITER ;

CALL listar_proveedores_por_categoria(1);

10.
DELIMITER $$
CREATE PROCEDURE pedido_mayor_valor()
BEGIN
    SELECT p.id AS pedido_id, c.name AS nombre_cliente, SUM(dp.cantidad * dp.precio) AS total_pedido
    FROM pedidos p
    JOIN detalle_pedidos dp ON p.id = dp.pedido_id
    JOIN clientes c ON p.cliente_id = c.id
    GROUP BY p.id, c.name
    ORDER BY total_pedido DESC
    LIMIT 1;
END $$
DELIMITER ;

CALL pedido_mayor_valor();
