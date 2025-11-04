💻 Configuración Inicial del Proyecto
Sigue estos pasos en tu sistema operativo (Windows, macOS o Linux):

1. Procedimiento para crear carpeta del Proyecto: UIII_boutique_1158
Abre una terminal (Símbolo del sistema, PowerShell, Terminal, etc.) y navega a la ubicación donde deseas guardar tu proyecto. Luego, ejecuta:

Bash

mkdir UIII_boutique_1158
2. Procedimiento para abrir VS Code sobre la carpeta UIII_boutique_1158
Desde la misma terminal, dentro del directorio donde creaste la carpeta, ejecuta:

Bash

cd UIII_boutique_1158
code .
Esto abrirá VS Code con la carpeta UIII_boutique_1158 como raíz del espacio de trabajo.

3. Procedimiento para abrir terminal en VS Code
Dentro de VS Code, usa el atajo de teclado:

Windows/Linux: `Ctrl + Shift + ``

macOS: `Cmd + Shift + ``

Alternativamente, ve al menú Terminal y selecciona Nueva terminal. La terminal se abrirá en la parte inferior, ubicada en la carpeta raíz UIII_boutique_1158.

4. Procedimiento para crear carpeta entorno virtual .venv desde terminal de VS Code
Asegúrate de estar en la terminal de VS Code y ejecuta el siguiente comando para crear el entorno virtual con el nombre .venv:

Bash

python -m venv .venv
5. Procedimiento para activar el entorno virtual
Windows (PowerShell):

Bash

.venv\Scripts\Activate.ps1
Windows (Símbolo del sistema):

Bash

.venv\Scripts\activate.bat
macOS/Linux:

Bash

source .venv/bin/activate
Una vez activo, verás (.venv) al inicio de la línea de comandos en tu terminal.

6. Procedimiento para activar intérprete de python (en VS Code)
Con el entorno virtual activado:

Abre la Paleta de comandos (Ctrl+Shift+P o Cmd+Shift+P).

Escribe y selecciona "Python: Select Interpreter".

Elige el intérprete que está dentro de tu entorno virtual: ./.venv/Scripts/python.exe (o similar, dependiendo de tu SO).

7. Procedimiento para instalar Django
En la terminal de VS Code (con (.venv) activado), ejecuta:

Bash

pip install django
🏗️ Creación de Proyecto y Aplicación
8. Procedimiento para crear proyecto backend_boutique sin duplicar carpeta
Desde la terminal (en la carpeta UIII_boutique_1158), usa el punto (.) al final para crear la estructura del proyecto en el directorio actual, evitando una subcarpeta duplicada:

Bash

django-admin startproject backend_boutique .
9. Procedimiento para ejecutar servidor en el puerto 8036
Ejecuta el servidor de desarrollo:

Bash

python manage.py runserver 8036
10. Procedimiento para copiar y pegar el link en el navegador
Cuando el servidor se ejecute, verás un mensaje similar a este en la terminal:

Starting development server at http://127.0.0.1:8036/

Copia esa dirección URL (o la que se muestre) y pégala en tu navegador web. Si la configuración fue exitosa, verás la página de bienvenida de Django.

11. Procedimiento para crear aplicación app_boutique
Detén el servidor (Ctrl+C). Luego, ejecuta el comando para crear la aplicación dentro de tu proyecto (debes estar en el mismo nivel de manage.py):

Bash

python manage.py startapp app_boutique
💾 Modelos de la Base de Datos
12. Aquí el modelo models.py
Copia y pega el siguiente código en el archivo app_boutique/models.py, reemplazando su contenido existente:

Python

from django.db import models

# ==========================================
# MODELO: ventas
# ==========================================
class Venta(models.Model):
    id_venta = models.AutoField(primary_key=True) # ID único de la venta
    total = models.DecimalField(max_digits=10, decimal_places=2) # Total de la venta
    cantidad = models.PositiveIntegerField() # Cantidad de productos vendidos
    empleado = models.CharField(max_length=100) # Nombre del empleado que realizó la venta
    fecha = models.DateTimeField(auto_now_add=True) # Fecha y hora en que se registró la venta

    def __str__(self):
        return f"Venta {self.id_venta} - {self.empleado} - {self.fecha}"

    class Meta:
        db_table = 'ventas' # Nombre de la tabla en la base de datos
        verbose_name = 'Venta'
        verbose_name_plural = 'Ventas'

# ==========================================
# MODELO: clientes
# ==========================================
class Cliente(models.Model):
    id_cliente = models.AutoField(primary_key=True) # ID único de cliente
    nombre = models.CharField(max_length=100) # Nombre del cliente
    telefono = models.CharField(max_length=15, null=True, blank=True) # Teléfono, puede ser opcional
    correo = models.EmailField(max_length=100, unique=True) # Correo electrónico del cliente
    fecha_registro = models.DateTimeField(auto_now_add=True) # Fecha de registro
    direccion = models.TextField(null=True, blank=True) # Dirección del cliente
    metodo_pago = models.CharField(
        max_length=50,
        choices=[ # Métodos de pago posibles
            ('tarjeta', 'Tarjeta de Crédito/Débito'),
            ('efectivo', 'Efectivo'),
            ('transferencia', 'Transferencia Bancaria'),
            ('paypal', 'PayPal')
        ],
        default='tarjeta'
    ) # Método de pago preferido

    def __str__(self):
        return f"{self.nombre} - {self.correo}"

    class Meta:
        db_table = 'clientes' # Nombre de la tabla en la base de datos
        verbose_name = 'Cliente'
        verbose_name_plural = 'Clientes'

# ==========================================
# MODELO: Productos
# ==========================================
class Categoria(models.Model):
    nombre = models.CharField(max_length=100)
    
    def __str__(self):
        return self.nombre
        
    class Meta:
        db_table = 'categorias' # Nombre de la tabla en la base de datos (por consistencia)
        verbose_name = 'Categoría'
        verbose_name_plural = 'Categorías'

class Proveedor(models.Model):
    nombre = models.CharField(max_length=100)
    contacto = models.CharField(max_length=100, null=True, blank=True)
    
    def __str__(self):
        return self.nombre
        
    class Meta:
        db_table = 'proveedores'
        verbose_name = 'Proveedor'
        verbose_name_plural = 'Proveedores'

class Producto(models.Model):
    id_producto = models.AutoField(primary_key=True) # ID único de producto
    nombre = models.CharField(max_length=150) # Nombre del producto
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE, related_name="productos") # Relación con categoría
    precio = models.DecimalField(max_digits=10, decimal_places=2) # Precio del producto
    existencias = models.PositiveIntegerField() # Número de existencias disponibles
    talla = models.CharField(max_length=10, null=True, blank=True) # Talla del producto (opcional)
    proveedor = models.ForeignKey(Proveedor, on_delete=models.CASCADE, related_name="productos") # Relación con proveedor

    def __str__(self):
        return f"{self.nombre} - {self.categoria.nombre}"

    class Meta:
        db_table = 'productos' # Nombre de la tabla en la base de datos
        verbose_name = 'Producto'
        verbose_name_plural = 'Productos'

25. Procedimiento para agregar app_boutique en settings.py de backend_boutique
Abre backend_boutique/settings.py y agrega 'app_boutique' a la lista INSTALLED_APPS:

Python

# backend_boutique/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Tu aplicación
    'app_boutique',
]
12.5 Procedimiento para realizar las migraciones (makemigrations y migrate)
Ejecuta los siguientes comandos en la terminal (donde está manage.py):

Crear archivos de migración (esquemas):

Bash

python manage.py makemigrations
Nota: Esto debería crear migraciones para todos tus modelos (Venta, Cliente, Categoria, Proveedor, Producto).

Aplicar las migraciones (crear las tablas en la base de datos):

Bash

python manage.py migrate
27. Procedimiento para registrar los modelos en admin.py y volver a realizar las migraciones (Solo Categoria por ahora)
Abre app_boutique/admin.py y reemplaza su contenido con este código para registrar el modelo Categoria:

Python

# app_boutique/admin.py
from django.contrib import admin
from .models import Categoria

# Registramos el modelo Categoría (27)
admin.site.register(Categoria)

# 27 Por lo pronto solo trabajar con “categoría” dejar pendiente
# admin.site.register(Venta)
# admin.site.register(Cliente)
# admin.site.register(Proveedor)
# admin.site.register(Producto)
Nota: Como solo hemos modificado admin.py, no se requieren nuevas migraciones (paso 12.5) para que el modelo aparezca en el administrador de Django.

⚙️ Configuración de Vistas y URLs (Categoría CRUD)
14. En views.py de app_boutique crear las funciones...
Abre app_boutique/views.py y reemplaza su contenido con el código para las funciones solicitadas:

Python

# app_boutique/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Categoria
from django.db.models import ProtectedError # Importar para manejo de errores

# Funciones de Vistas (14)

# ==========================================
# INICIO
# ==========================================
def inicio_boutique(request):
    """Muestra la página de inicio."""
    return render(request, 'inicio.html')

# ==========================================
# CATEGORIA - CRUD
# ==========================================

def agregar_categoria(request):
    """Maneja la creación de una nueva categoría."""
    if request.method == 'POST':
        # 23. No utilizar forms.py. Se toma el dato directamente
        nombre = request.POST.get('nombre')
        
        # 28. No validar entrada de datos.
        
        if nombre:
            Categoria.objects.create(nombre=nombre)
            return redirect('ver_categorias')
    
    return render(request, 'categoria/agregar_categoria.html')

def ver_categorias(request):
    """Muestra una lista de todas las categorías."""
    categorias = Categoria.objects.all().order_by('id')
    return render(request, 'categoria/ver_categorias.html', {'categorias': categorias})

def actualizar_categoria(request, pk):
    """Muestra el formulario para editar una categoría existente."""
    categoria = get_object_or_404(Categoria, pk=pk)
    return render(request, 'categoria/actualizar_categoria.html', {'categoria': categoria})

def realizar_actualizacion_categoria(request, pk):
    """Procesa la actualización de la categoría."""
    categoria = get_object_or_404(Categoria, pk=pk)
    
    if request.method == 'POST':
        # 23. No utilizar forms.py. Se toma el dato directamente
        nombre = request.POST.get('nombre')
        
        # 28. No validar entrada de datos.
        
        if nombre:
            categoria.nombre = nombre
            categoria.save()
            return redirect('ver_categorias')
    
    # En caso de error, podría redirigir al formulario de nuevo
    return redirect('actualizar_categoria', pk=pk)


def borrar_categoria(request, pk):
    """Maneja la eliminación de una categoría."""
    categoria = get_object_or_404(Categoria, pk=pk)
    
    if request.method == 'POST':
        try:
            categoria.delete()
            return redirect('ver_categorias')
        except ProtectedError:
            # Ejemplo de manejo de error (opcional pero recomendado)
            mensaje_error = "No se puede eliminar la categoría porque tiene productos asociados."
            return render(request, 'categoria/borrar_categoria.html', {
                'categoria': categoria,
                'error': mensaje_error
            })
            
    return render(request, 'categoria/borrar_categoria.html', {'categoria': categoria})
24. Procedimiento para crear el archivo urls.py en app_boutique
Crea un nuevo archivo llamado urls.py dentro de la carpeta app_boutique y agrega el siguiente código:

Python

# app_boutique/urls.py
from django.urls import path
from . import views

urlpatterns = [
    # INICIO (14)
    path('', views.inicio_boutique, name='inicio_boutique'),
    
    # CATEGORÍA CRUD (14, 24)
    path('categoria/agregar/', views.agregar_categoria, name='agregar_categoria'),
    path('categoria/ver/', views.ver_categorias, name='ver_categorias'),
    path('categoria/actualizar/<int:pk>/', views.actualizar_categoria, name='actualizar_categoria'),
    path('categoria/actualizar/realizar/<int:pk>/', views.realizar_actualizacion_categoria, name='realizar_actualizacion_categoria'),
    path('categoria/borrar/<int:pk>/', views.borrar_categoria, name='borrar_categoria'),
]
26. Realizar las configuraciones correspondiente a urls.py de backend_boutique
Abre backend_boutique/urls.py y agrega la ruta de tu aplicación, reemplazando el contenido existente (o solo la lista urlpatterns):

Python

# backend_boutique/urls.py
from django.contrib import admin
from django.urls import path, include # Importar 'include'

urlpatterns = [
    path('admin/', admin.site.urls),
    # Enlazar las URLs de la aplicación app_boutique (26)
    path('', include('app_boutique.urls')), 
]
📁 Estructura y Archivos HTML (Templates)
29. Al inicio crear la estructura completa de carpetas y archivos.
Estructura de Carpetas y Archivos (29): (Se asume que los archivos settings.py, manage.py, etc., ya existen de los pasos 8 y 11)

UIII_boutique_1158/
├── .venv/
├── backend_boutique/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py  <-- Modificado (26)
│   └── wsgi.py
├── app_boutique/
│   ├── migrations/
│   ├── templates/  <-- Creada (15)
│   │   ├── categoria/  <-- Creada (21)
│   │   │   ├── agregar_categoria.html  <-- Creado (22)
│   │   │   ├── actualizar_categoria.html <-- Creado (22)
│   │   │   ├── borrar_categoria.html     <-- Creado (22)
│   │   │   └── ver_categorias.html       <-- Creado (22)
│   │   ├── base.html   <-- Creado (16)
│   │   ├── footer.html <-- Creado (16)
│   │   ├── header.html <-- Creado (16)
│   │   ├── inicio.html <-- Creado (16)
│   │   └── navbar.html <-- Creado (16)
│   ├── __init__.py
│   ├── admin.py  <-- Modificado (27)
│   ├── apps.py
│   ├── models.py <-- Modificado (12)
│   ├── urls.py   <-- Creado (24)
│   └── views.py  <-- Modificado (14)
└── manage.py
15. Crear la carpeta “templates” dentro de “app_boutique”
Crea la carpeta: app_boutique/templates.

21. Crear la subcarpeta carpeta categoria dentro de app_boutique/templates
Crea la carpeta: app_boutique/templates/categoria.

16, 17, 18, 19, 20. Archivos HTML base
Crea los siguientes archivos dentro de app_boutique/templates:

base.html (16, 17)
HTML

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Boutique UIII{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <style>
        /* Estilo general para colores suaves y atractivos (28) */
        :root {
            --color-principal: #a5d8ff; /* Azul cielo suave */
            --color-secundario: #f0bfff; /* Lila suave */
            --color-fondo: #f8f9fa; /* Gris claro */
            --color-texto: #343a40; /* Gris oscuro */
        }
        body {
            background-color: var(--color-fondo);
            color: var(--color-texto);
            padding-bottom: 60px; /* Espacio para el footer fijo */
        }
        .navbar-custom {
            background-color: var(--color-principal);
        }
        .footer-custom {
            background-color: var(--color-principal);
            color: var(--color-texto);
            padding: 10px 0;
            position: fixed; /* Footer fijo (19) */
            bottom: 0;
            width: 100%;
        }
        .card-header-custom {
            background-color: var(--color-secundario);
            color: var(--color-texto);
        }
    </style>
</head>
<body>
    {% include 'navbar.html' %} 

    <div class="container mt-4">
        {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
navbar.html (16, 18)
HTML

<nav class="navbar navbar-expand-lg navbar-custom shadow-sm">
    <div class="container-fluid">
        <a class="navbar-brand text-dark fw-bold" href="{% url 'inicio_boutique' %}">
            <i class="bi bi-shop me-2"></i>Sistema de Administración Boutique (18)
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav ms-auto">
                
                <li class="nav-item">
                    <a class="nav-link active text-dark" aria-current="page" href="{% url 'inicio_boutique' %}">
                        <i class="bi bi-house-door-fill me-1"></i>Inicio (18)
                    </a>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle text-dark" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-tags-fill me-1"></i>Categoría (18)
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_categoria' %}">Agregar Categoría</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_categorias' %}">Ver Categorías</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="#">Actualizar Categoría</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Categoría</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle text-dark" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-people-fill me-1"></i>Clientes (18)
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Ver Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Clientes</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle text-dark" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-cart-fill me-1"></i>Ventas (18)
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Ventas</a></li>
                        <li><a class="dropdown-item" href="#">Ver Ventas</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Ventas</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Ventas</a></li>
                    </ul>
                </li>
                
            </ul>
        </div>
    </div>
</nav>
header.html (16)
Este archivo no es utilizado directamente en la estructura, ya que base.html incluye navbar.html y maneja las etiquetas de la cabecera (<head>).

footer.html (16, 19)
HTML

<footer class="footer-custom text-center mt-5">
    <div class="container">
        <span class="text-dark">
            &copy; Derechos de autor {{ "now"|date:"Y" }}. Creado por **Joel Rojas, Cbtis 128** (19).
            Fecha del sistema: {{ "now"|date:"d/m/Y H:i" }}
        </span>
    </div>
</footer>
inicio.html (16, 20)
HTML

{% extends 'base.html' %}

{% block title %}Inicio - Boutique UIII{% endblock %}

{% block content %}
<div class="p-5 mb-4 rounded-3 text-center" style="background-color: var(--color-secundario);">
    <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold text-dark">Bienvenido al Sistema de Administración de la Boutique</h1>
        <p class="fs-4 text-dark">Gestión eficiente para Categorías, Clientes, Productos y Ventas.</p>
    </div>
</div>

<div class="row align-items-center">
    <div class="col-md-6">
        <h2>Información del Sistema (20)</h2>
        <p>Este sistema ha sido desarrollado como proyecto para la Unidad III, utilizando Python con el framework Django. Su objetivo es proporcionar una interfaz de administración completa para una boutique, facilitando las operaciones CRUD esenciales para la gestión de inventario y clientes.</p>
        <ul class="list-unstyled">
            <li><i class="bi bi-check-circle-fill text-success me-2"></i>Lenguaje: Python</li>
            <li><i class="bi bi-check-circle-fill text-success me-2"></i>Framework: Django</li>
            <li><i class="bi bi-check-circle-fill text-success me-2"></i>Base de Datos: SQLite (por defecto)</li>
        </ul>
    </div>
    <div class="col-md-6 text-center">
        
    </div>
</div>
{% endblock %}
22. Archivos HTML de Categoría (CRUD)
Crea los siguientes archivos dentro de app_boutique/templates/categoria:

agregar_categoria.html
HTML

{% extends 'base.html' %}

{% block title %}Agregar Categoría{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card shadow-lg border-0">
            <div class="card-header card-header-custom text-center">
                <h3 class="mb-0">Agregar Nueva Categoría</h3>
            </div>
            <div class="card-body">
                <form method="post">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre de la Categoría</label>
                        <input type="text" class="form-control" id="nombre" name="nombre" required>
                    </div>
                    <div class="d-grid gap-2">
                        <button type="submit" class="btn btn-primary" style="background-color: #f0bfff; border-color: #f0bfff; color: #343a40;">Guardar Categoría</button>
                        <a href="{% url 'ver_categorias' %}" class="btn btn-outline-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
ver_categorias.html
HTML

{% extends 'base.html' %}

{% block title %}Ver Categorías{% endblock %}

{% block content %}
<div class="card shadow-lg border-0">
    <div class="card-header card-header-custom text-center">
        <h3 class="mb-0">Listado de Categorías</h3>
    </div>
    <div class="card-body">
        <div class="d-flex justify-content-end mb-3">
            <a href="{% url 'agregar_categoria' %}" class="btn btn-success">
                <i class="bi bi-plus-circle me-2"></i>Agregar Categoría
            </a>
        </div>
        
        {% if categorias %}
        <div class="table-responsive">
            <table class="table table-hover table-striped">
                <thead class="table-dark" style="background-color: var(--color-secundario); border-color: var(--color-secundario);">
                    <tr>
                        <th scope="col"># ID</th>
                        <th scope="col">Nombre</th>
                        <th scope="col" class="text-center">Acciones (Ver, Editar, Borrar) (22)</th>
                    </tr>
                </thead>
                <tbody>
                    {% for cat in categorias %}
                    <tr>
                        <td>{{ cat.id }}</td>
                        <td>{{ cat.nombre }}</td>
                        <td class="text-center">
                            <button type="button" class="btn btn-sm btn-info text-white" disabled>
                                <i class="bi bi-eye"></i> Ver
                            </button>
                            <a href="{% url 'actualizar_categoria' pk=cat.id %}" class="btn btn-sm btn-warning">
                                <i class="bi bi-pencil"></i> Editar
                            </a>
                            <a href="{% url 'borrar_categoria' pk=cat.id %}" class="btn btn-sm btn-danger">
                                <i class="bi bi-trash"></i> Borrar
                            </a>
                        </td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
        {% else %}
        <div class="alert alert-info text-center" role="alert">
            No hay categorías registradas. ¡Agrega la primera!
        </div>
        {% endif %}
    </div>
</div>
{% endblock %}
actualizar_categoria.html
HTML

{% extends 'base.html' %}

{% block title %}Actualizar Categoría{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card shadow-lg border-0">
            <div class="card-header card-header-custom text-center">
                <h3 class="mb-0">Actualizar Categoría: **{{ categoria.nombre }}**</h3>
            </div>
            <div class="card-body">
                <form method="post" action="{% url 'realizar_actualizacion_categoria' pk=categoria.id %}">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nuevo Nombre de la Categoría</label>
                        <input type="text" class="form-control" id="nombre" name="nombre" value="{{ categoria.nombre }}" required>
                    </div>
                    <div class="d-grid gap-2">
                        <button type="submit" class="btn btn-primary" style="background-color: #f0bfff; border-color: #f0bfff; color: #343a40;">Guardar Cambios</button>
                        <a href="{% url 'ver_categorias' %}" class="btn btn-outline-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
borrar_categoria.html
HTML

{% extends 'base.html' %}

{% block title %}Borrar Categoría{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card shadow-lg border-danger border-2">
            <div class="card-header bg-danger text-white text-center">
                <h3 class="mb-0">Confirmar Eliminación</h3>
            </div>
            <div class="card-body">
                {% if error %}
                    <div class="alert alert-warning" role="alert">
                        <strong>Advertencia:</strong> {{ error }}
                    </div>
                    <a href="{% url 'ver_categorias' %}" class="btn btn-secondary mt-3">Volver al listado</a>
                {% else %}
                    <p class="lead text-center">¿Estás seguro que deseas **eliminar** la categoría?</p>
                    <h4 class="text-center text-danger mb-4">"{{ categoria.nombre }}" (ID: {{ categoria.id }})</h4>
                    
                    <form method="post">
                        {% csrf_token %}
                        <div class="d-grid gap-2">
                            <button type="submit" class="btn btn-danger">Sí, Eliminar</button>
                            <a href="{% url 'ver_categorias' %}" class="btn btn-outline-secondary">Cancelar</a>
                        </div>
                    </form>
                {% endif %}
            </div>
        </div>
    </div>
</div>
{% endblock %}
31. Finalmente ejecutar servidor en el puerto puerto 8036.
Para finalizar esta primera parte y verificar la funcionalidad:

Bash

python manage.py runserver 8036
Tu proyecto está ahora totalmente funcional (30) para la gestión (CRUD) de categorías.
