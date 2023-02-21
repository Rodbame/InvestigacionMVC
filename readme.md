# Investigacion sobre el modelo de MVC

## ¿Que es el MVC?
El modelo vista controlador se basa en un estilo de arquitectura de software el cual separa el codigo en tres apartados diferentes, como lo seria los datos de la aplicacion, la intefaz del usuario, y la logica de control.

Aunque en esta investigacion solo se manejara de manera mas especifica la parte del modelo.

## ¿Que es el modelo en MVC?
El modelo es responsable de la gestión de los datos de la aplicación, así como de la lógica de negocio y del comportamiento de la aplicación. En otras palabras, el modelo representa la información subyacente y las reglas de negocio que se aplican a esa información. Por ejemplo, si se está diseñando una aplicación de gestión de pedidos, el modelo podría contener información sobre los productos, los clientes y los pedidos, así como reglas de negocio para validar los pedidos o para calcular los costos.

## Aplicaciones del modelo
- **Gestión de datos:** El modelo se utiliza para gestionar los datos de la aplicación, incluyendo la creación, lectura, actualización y eliminación de los mismos. El modelo también se encarga de aplicar las reglas de negocio y realizar la validación de los datos.

- **Lógica de negocio:** El modelo también se utiliza para implementar la lógica de negocio de la aplicación, como el procesamiento de pagos, la validación de formularios, la generación de informes, entre otros.

- **Integración con bases de datos y otros sistemas:** El modelo puede interactuar con bases de datos y otros sistemas externos para obtener y actualizar datos de manera dinámica.

- **Acceso a datos:** El modelo también se utiliza para acceder a los datos almacenados en la aplicación y proporcionarlos a la vista y el controlador.

En general el modelo es utilizado en todo lo que seria la gestion de los datos y la logica del negocio de la aplicacion, este puede aplicarse para distintos ambitos, unos de los mas utilizados serian el hacer llamadas a las bases de datos para realizar las funciones de un crud que seria crear, leer, actualizar y eliminar.

## La infraestructura para el almacenamiento y recuperacion de datos

La infraestructura para el almacenamiento y recuperación de datos dentro del modelo en MVC puede incluir diferentes tecnologías y herramientas, dependiendo del tipo de aplicación y de los requisitos de almacenamiento de datos. Algunos componentes comunes de esta capa pueden incluir:

- Sistemas de gestión de bases de datos (DBMS): Estos son sistemas diseñados para almacenar y gestionar grandes cantidades de datos, proporcionando herramientas para la creación, modificación y eliminación de datos, así como para la consulta y recuperación de los mismos.

- Modelos de datos: Estos son estructuras que representan los datos de la aplicación, y que pueden incluir tablas, clases, objetos o cualquier otro tipo de estructura de datos.

- Capa de acceso a datos: Esta capa se encarga de gestionar la comunicación entre el modelo y la base de datos, proporcionando una interfaz para que el modelo pueda realizar operaciones de lectura, escritura y eliminación de datos.

### Ejemplos:
Un ejemplo simple de como es que se puede utilizar el modelo en un CRUD de productos
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

Otro ejemplo de un uso del modelo pero ahora en JS seria el siguiente:
```
// Definimos la clase del modelo
class Modelo {
  // Creamos el constructor del modelo
  constructor(parametro1, parametro2) {
    // Inicializamos las propiedades con los valores de los parámetros
    this.propiedad1 = parametro1;
    this.propiedad2 = parametro2;
  }

  // Creamos los métodos de acceso a las propiedades del modelo
  getPropiedad1() {
    return this.propiedad1;
  }

  getPropiedad2() {
    return this.propiedad2;
  }

  // Creamos un método para realizar una operación con el modelo
  operacion() {
    // Realizamos alguna operación con las propiedades del modelo
    let resultado = this.propiedad1 * this.propiedad2;
    // Retornamos el resultado de la operación
    return resultado;
  }
}
```
Este ejemplo simplemente lo que hace es el obtener 2 parametros para despues darles un valor y realizar la operación que queramos realizar, en el caso de este ejemplo lo que se hace es obtener esos dos parametros con el fin de multiplicarlos, esto puede servir por ejemplo al momento de realizar una calculadora de practica o algo por el estilo.
# Investigacion sobre las vistas en el MVC

## ¿Que es la vista en MVC?
La vista en MVC es la capa de presentación de una aplicación que se encarga de mostrar los datos y la interfaz de usuario, mientras que el controlador es el intermediario entre la vista y el modelo, y el modelo es responsable de la lógica de negocios y el acceso a los datos.

## Ejemplo de Vista 
Este simple ejemplo de la vista de un registro de usuarios mostraria principalmente lo que seria la vista de los usuarios que desean registrarse
```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Formulario de registro</title>
	<!-- Agregamos los estilos de Bootstrap -->
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>
	<div class="container">
		<div class="row">
			<div class="col-md-6 offset-md-3">
				<h1>Formulario de registro</h1>
				<!-- Creamos un formulario para el registro de usuarios -->
				<form method="post" action="/registro">
					<!-- Agregamos un campo para el nombre de usuario -->
					<div class="form-group">
						<label for="nombre_usuario">Nombre de usuario</label>
						<input type="text" class="form-control" id="nombre_usuario" name="nombre_usuario" placeholder="Ingresa tu nombre de usuario">
					</div>
					<!-- Agregamos un campo para la dirección de correo electrónico -->
					<div class="form-group">
						<label for="email">Correo electrónico</label>
						<input type="email" class="form-control" id="email" name="email" placeholder="Ingresa tu dirección de correo electrónico">
					</div>
					<!-- Agregamos un campo para la contraseña -->
					<div class="form-group">
						<label for="contrasena">Contraseña</label>
						<input type="password" class="form-control" id="contrasena" name="contrasena" placeholder="Ingresa tu contraseña">
					</div>
					<!-- Agregamos un botón para enviar el formulario -->
					<button type="submit" class="btn btn-primary">Registrarse</button>
				</form>
			</div>
		</div>
	</div>
	<!-- Agregamos los scripts de Bootstrap -->
	<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/popper.js@1.14.7/dist/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
	<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</body>
</html>

```

Y el resultado de esta pagina nos daria la siguiente interfaz:

<image src="./img/vista.png" alt="Muestra del ejemplo anterior">

Esta vista seria con la que el usuario podria interactuar y donde mediante los inputs llenaria la informacion necesaria para crear el usuario en este caso, pudiendo asi enviar la informacion por el controlador y este mandando esos datos a el modelo para poder realizar por ejemplo un insert en mysql.

## Representacion de datos hacia el usuario
Otro uso que nos brinda el modelo ya que interactua directamente con la base de datos seria el crearnos la parte de tabla donde se muestren los datos de la base de datos.

### Ejemplo:
Primero tendriamos el codigo HTML con PHP para la generacion de las columnas de las tablas, en esta misma tabla podemos observar que se genera el rellenado de la tabla mediante PHP en la parte de **```<?php echo $datos; ?>```** que es tomada del controlador y el controlador la toma desde el modelo.

#### Parte de la vista
```
	<table class="table table-striped">
			<tr>
				<td>Num</td>
				<td>Nombre</td>
				<td>Cantidad</td>
				<td>Proveedor</td>
				<td>Categoria</td>
				<?php 
					 if ($_SESSION['privilegio']=='Admin') 
					 {
				?>
				<td colspan="2" align="center">Accion</td>
				
					 
				<?php } ?>
				
				<?php echo $datos; ?>
		</table>
	</div>
```
#### Parte del controllador
```
	if (isset($_POST['btnbuscar'])) 
		{
			$buscar = $_POST['txtbuscar'];
			$result = $obj->Buscar($buscar);
			$datos = $obj->Tabla_gen($result);
		}
		else
		{
			$result = $obj->Buscartodo();
			$datos = $obj->Tabla_gen($result);
		}
```
#### Parte del modelo
```
	//Funcion para traer el listado de la tabla donde se muestren todos los productos de la base de datos
	function Buscartodo()
	{
		$result = $this->Ejecutar_Instruccion("Select productos.id_producto,productos.Nombre,productos.Cantidad,concat(proveedores.Nombres,' ',proveedores.Apellido_p,' ',proveedores.Apellido_m) as Nombre_prov,categorias.Nombre from productos INNER JOIN proveedores ON productos.id_proveedor=proveedores.id_proveedor INNER JOIN categorias ON productos.id_categoria=categorias.id_categoria;");
		return $result;
	}
	//Generador de la tabla con los resultados
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
```
Esto en conjunto con  nos generaria una tabla como la siguiente:
<image src="./img/ejemplovista.png" alt="Muestra del ejemplo anterior">


