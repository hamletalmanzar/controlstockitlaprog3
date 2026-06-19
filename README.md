ControlStock

Sistema de control de inventario y gestion de usuarios desarrollado en C# (.NET Framework 4.7.2) con interfaz Windows Forms y base de datos SQL Server.

Repositorio: https://github.com/hamletalmanzar/controlstockitlaprog3

Descripcion

ControlStock es una aplicacion de escritorio que permite gestionar el stock de productos y los usuarios del sistema. Cuenta con un modulo de autenticacion (login) y un modulo de registro de usuarios, conectados a una base de datos SQL Server local.

Estructura del Proyecto

ControlStock/
│
├── clases/                     # Capa de logica y acceso a datos
│   ├── ConexionBD.cs           # Clase estatica para la conexion a SQL Server
│   ├── Producto.cs             # Modelo de datos: Producto
│   ├── ProductoRepository.cs   # CRUD de productos (Obtener, Insertar, Eliminar)
│   ├── UsuarioModel.cs         # Modelo de datos: Usuario
│   └── UsuarioRepository.cs    # Login e insercion de usuarios
│
├── formularios/                # Capa de presentacion (Windows Forms)
│   ├── Form1.cs                # Formulario de Login
│   └── Usuario.cs              # Formulario de registro de usuario
│
├── UIX/                        # Componentes visuales personalizados
│   ├── botones.cs              # Control de boton personalizado
│   └── textBoxOval.cs          # Control TextBox con estilo redondeado
│
├── Program.cs                  # Punto de entrada de la aplicacion
└── App.config                  # Configuracion de ensamblados y runtime

Tecnologias Utilizadas

Lenguaje: C#
Framework: .NET Framework 4.7.2
UI: Windows Forms
Base de datos: SQL Server (via Microsoft.Data.SqlClient 7.0.1)
IDE: Visual Studio

Base de Datos

La aplicacion se conecta a una instancia local de SQL Server Express con la base de datos SistemaVentas.

Cadena de conexion (en ConexionBD.cs):

Server=KING\SQLEXPRESS;
Database=SistemaVentas;
Integrated Security=True;
TrustServerCertificate=True;

Para usar el proyecto en otro equipo, cambia KING por el nombre de tu instancia de SQL Server en ConexionBD.cs.

Tablas requeridas:

sql-- Tabla de usuarios
CREATE TABLE USUARIO (
    id_usuario  INT PRIMARY KEY IDENTITY,
    nombre      VARCHAR(100),
    apellido    VARCHAR(100),
    correo      VARCHAR(150) UNIQUE,
    contrasena  VARCHAR(255),
    rol         VARCHAR(50)   -- 'Administrador' o 'Empleado'
);

-- Tabla de productos
CREATE TABLE PRODUCTO (
    id_producto  INT PRIMARY KEY IDENTITY,
    id_categoria INT,
    nombre       VARCHAR(150),
    codigo       VARCHAR(50),
    precio       DECIMAL(10,2),
    stock        INT
);

Para restaurar la base de datos desde el backup incluido (SistemaVentas.bak), ejecuta en SQL Server Management Studio:

sqlRESTORE DATABASE SistemaVentas
FROM DISK = 'C:\ruta\SistemaVentas.bak'
WITH REPLACE;

Instalacion y Ejecucion

Requisitos previos

Windows 10 o superior
Visual Studio 2019 o superior (con carga de trabajo de desarrollo de escritorio .NET)
SQL Server Express (o superior)
.NET Framework 4.7.2

Pasos

Clonar el repositorio

bash   git clone https://github.com/hamletalmanzar/controlstockitlaprog3.git

Restaurar la base de datos
Abre SQL Server Management Studio y restaura SistemaVentas.bak (ver seccion anterior).
Configurar la conexion
En clases/ConexionBD.cs, actualiza el nombre del servidor si es necesario:

csharp   "Server=TU_SERVIDOR\\SQLEXPRESS;"

Abrir el proyecto
Abre ControlStock.sln con Visual Studio.
Restaurar paquetes NuGet
Visual Studio los restaurara automaticamente al compilar. Si no, ve a:
Herramientas -> Administrador de paquetes NuGet -> Restaurar paquetes
Compilar y ejecutar
Presiona F5 o haz clic en Iniciar.

Modulos Implementados

Login (Form1.cs)

Validacion de campos vacios
Autenticacion por correo y contrasena contra la base de datos
Redireccion segun rol (Administrador / Empleado)

Registro de Usuario (Usuario.cs)

Formulario con campos: nombre, apellido, correo y contrasena
Insercion en la tabla USUARIO con rol por defecto 'Empleado'
Retorno al login al completar el registro

Gestion de Productos (ProductoRepository.cs)

Listar todos los productos ordenados por nombre
Insertar nuevo producto
Eliminar producto por ID

Equipo

Proyecto desarrollado como tarea de Programacion 3 - ITLA.

Notas

La contrasena se almacena en texto plano en esta version inicial. Se recomienda aplicar hashing en versiones futuras.
El archivo .bak contiene la base de datos de prueba con datos iniciales.
La carpeta packages/ puede ignorarse en el repositorio usando .gitignore.
