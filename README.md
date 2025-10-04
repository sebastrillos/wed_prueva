💻 Proyecto grupal: “ShopEase” con Django + Bootstrap
🧠 Descripción general

ShopEase será una tienda virtual básica donde los usuarios:

Inician sesión 🔐

Ven productos 📦

Consultan o editan su perfil 👤

Revisan su carrito 🛒

🏗️ Estructura general del proyecto Django
ShopEase/
│
├── manage.py
├── shopease/                     # Carpeta del proyecto principal
│   ├── __init__.py
│   ├── settings.py               # Configuración general
│   ├── urls.py                   # Rutas globales
│   └── wsgi.py
│
├── core/                         # App principal
│   ├── __init__.py
│   ├── models.py                 # Tablas (Usuario, Producto, Carrito)
│   ├── views.py                  # Lógica de cada página
│   ├── urls.py                   # Rutas de la app
│   ├── templates/core/           # Archivos HTML
│   │   ├── login.html
│   │   ├── home.html
│   │   ├── perfil.html
│   │   └── carrito.html
│   └── static/core/              # Archivos estáticos
│       ├── css/
│       │   └── styles.css
│       ├── js/
│       │   └── main.js
│       └── img/
│           └── logo.png
│
└── db.sqlite3                    # Base de datos local

🌐 Páginas requeridas (4 según lineamientos)
Página	Ruta Django	Descripción
Login	/login/	Página de inicio de sesión de usuario.
Inicio / Productos	/	Muestra los productos disponibles.
Perfil	/perfil/	Muestra y edita datos del usuario.
Carrito	/carrito/	Visualiza productos agregados y total.
⚙️ Pasos para crear el proyecto
1. Crear el entorno Django
pip install django
django-admin startproject shopease
cd shopease
python manage.py startapp core

2. Configurar settings.py

Agrega 'core' a INSTALLED_APPS y configura las rutas para templates y staticfiles.

INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'core',
]

STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'core/static']
TEMPLATE_DIRS = [BASE_DIR / 'core/templates']

3. Definir modelos básicos (core/models.py)

Ejemplo simple:

from django.db import models
from django.contrib.auth.models import User

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=8, decimal_places=2)
    imagen = models.ImageField(upload_to='productos/', null=True, blank=True)

    def __str__(self):
        return self.nombre

4. Crear las vistas (core/views.py)

Ejemplo de la vista para el inicio:

from django.shortcuts import render
from .models import Producto

def home(request):
    productos = Producto.objects.all()
    return render(request, 'core/home.html', {'productos': productos})

def login_view(request):
    return render(request, 'core/login.html')

def perfil(request):
    return render(request, 'core/perfil.html')

def carrito(request):
    return render(request, 'core/carrito.html')

5. Configurar las rutas (core/urls.py)
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('login/', views.login_view, name='login'),
    path('perfil/', views.perfil, name='perfil'),
    path('carrito/', views.carrito, name='carrito'),
]


Y enlázalas en shopease/urls.py:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('core.urls')),
]

6. Crear las plantillas HTML con Bootstrap

Ejemplo básico para home.html:

{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>ShopEase | Inicio</title>
  <link rel="stylesheet" href="{% static 'core/css/bootstrap.min.css' %}">
</head>
<body class="bg-light">
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <a class="navbar-brand" href="#">ShopEase</a>
    <div class="collapse navbar-collapse">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a href="{% url 'perfil' %}" class="nav-link">Perfil</a></li>
        <li class="nav-item"><a href="{% url 'carrito' %}" class="nav-link">Carrito</a></li>
      </ul>
    </div>
  </nav>

  <div class="container mt-4">
    <div class="row">
      {% for producto in productos %}
      <div class="col-md-3 mb-4">
        <div class="card">
          <img src="{{ producto.imagen.url }}" class="card-img-top" alt="{{ producto.nombre }}">
          <div class="card-body">
            <h5 class="card-title">{{ producto.nombre }}</h5>
            <p class="card-text">${{ producto.precio }}</p>
            <button class="btn btn-primary">Agregar al carrito</button>
          </div>
        </div>
      </div>
      {% endfor %}
    </div>
  </div>
</body>
</html>

🧾 Documentación técnica (para entregar)

Incluye:

Portada con nombre del grupo y proyecto.

Descripción del sitio y frameworks usados (Django + Bootstrap).

Capturas de las 4 páginas.

Explicación técnica: modelos, vistas y rutas.

Reporte de commits de GitHub.