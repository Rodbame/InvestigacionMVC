#Investigacion sobre el modelo en MVC

##¿Que es el MVC?
El modelo vista controlador se basa en un estilo de arquitectura de software el cual separa el codigo en tres apartados diferentes, como lo seria los datos de la aplicacion, la intefaz del usuario, y la logica de control.

Aunque en esta investigacion solo se manejara de manera mas especifica la parte del modelo.

##¿Que es el modelo en MVC?
El modelo es responsable de la gestión de los datos de la aplicación, así como de la lógica de negocio y del comportamiento de la aplicación. En otras palabras, el modelo representa la información subyacente y las reglas de negocio que se aplican a esa información. Por ejemplo, si se está diseñando una aplicación de gestión de pedidos, el modelo podría contener información sobre los productos, los clientes y los pedidos, así como reglas de negocio para validar los pedidos o para calcular los costos.

##Aplicaciones del modelo
- **Gestión de datos:** El modelo se utiliza para gestionar los datos de la aplicación, incluyendo la creación, lectura, actualización y eliminación de los mismos. El modelo también se encarga de aplicar las reglas de negocio y realizar la validación de los datos.

- **Lógica de negocio:** El modelo también se utiliza para implementar la lógica de negocio de la aplicación, como el procesamiento de pagos, la validación de formularios, la generación de informes, entre otros.

- **Integración con bases de datos y otros sistemas:** El modelo puede interactuar con bases de datos y otros sistemas externos para obtener y actualizar datos de manera dinámica.

- **Acceso a datos:** El modelo también se utiliza para acceder a los datos almacenados en la aplicación y proporcionarlos a la vista y el controlador.

En general el modelo es utilizado en todo lo que seria la gestion de los datos y la logica del negocio de la aplicacion, este puede aplicarse para distintos ambitos, unos de los mas utilizados serian el hacer llamadas a las bases de datos para realizar las funciones de un crud que seria crear, leer, actualizar y eliminar


```modelo de un CRUD
<?php
//Se requiere el archivo que contiene la conexion de la base de datos
require 'bd/conexion_bd.php';

//Se crea una nueva clase con las funciones para el CRUD, el cual se BD_PDO para poder utilizarlo en los metodos
class Productos extends BD_PDO
{
	//Funcion para hacer un insert en la base de datos
	function Insertar($nombre, $cantidad, $idproveedor, $idcategoria)
	{
		$this->Ejecutar_Instruccion("Insert into productos(Nombre,Cantidad,id_proveedor,id_categoria) values('$nombre','$cantidad','$idproveedor','$idcategoria')");
	}
	//Funcion para hacer una busqueda en la base de datos
	function Buscar($buscar)
	{
		$result = $this->Ejecutar_Instruccion("Select productos.id_producto,productos.Nombre,productos.Cantidad,concat(proveedores.Nombres,' ',proveedores.Apellido_p,' ',proveedores.Apellido_m) as Nombre_prov,categorias.Nombre from productos INNER JOIN proveedores ON productos.id_proveedor=proveedores.id_proveedor INNER JOIN categorias ON productos.id_categoria=categorias.id_categoria where productos.Nombre like '%$buscar%'");
		return $result;
	}
	//Funcion para traer el listado de la tabla donde se muestren todos los productos de la base de datos
	function Buscartodo()
	{
		$result = $this->Ejecutar_Instruccion("Select productos.id_producto,productos.Nombre,productos.Cantidad,concat(proveedores.Nombres,' ',proveedores.Apellido_p,' ',proveedores.Apellido_m) as Nombre_prov,categorias.Nombre from productos INNER JOIN proveedores ON productos.id_proveedor=proveedores.id_proveedor INNER JOIN categorias ON productos.id_categoria=categorias.id_categoria;");
		return $result;
	}
	//Funcion para eliminar algun producto de la base de datos
	function Eliminar($id)
	{
		$this->Ejecutar_Instruccion("Delete from productos where id_producto = '$id'");
	}
	//Funcion para modificar algun producto de la base de datos
	function Modificar($nombre, $cantidad, $idproveedor, $idcategoria, $idproducto)
	{
		$this->Ejecutar_Instruccion("Update productos set Nombre='$nombre',Cantidad='$cantidad',id_proveedor='$idproveedor',id_categoria='$idcategoria' where id_producto = '$idproducto'");
	}
	//Funcion para generar la tabla con los productos de la base de datos, usando la funcion de Buscartodo()
	function Tabla_gen($result)
	{
		$tabla = "";
		foreach ($result as $renglon) {
			$tabla .= "<tr>";
			$tabla .= '<td>' . $renglon[0] . '</td>';
			$tabla .= '<td>' . $renglon[1] . '</td>';
			$tabla .= '<td>' . $renglon[2] . '</td>';
			$tabla .= '<td>' . $renglon[3] . '</td>';
			$tabla .= '<td>' . $renglon[4] . '</td>';
			if ($_SESSION['privilegio'] == 'Admin') {
				$tabla .= '<td><input type="button" id="btneliminar" class="btn" name="btneliminar" value="Eliminar" onclick="javascript: eliminar(' . $renglon[0] . ');"></td>';
				$tabla .= '<td><input type="button" id="btnmodificar" class="btn" name="btnmodificar" value="Modificar" onclick="javascript: modificar(' . $renglon[0] . ');"></td>';
			}
			$tabla .= '</tr>';
		}
		return $tabla;
	}
}

```