# Django: taller teórico práctico #

## A modo de introducción ##

___
No soy el profeta del desarrollo de software, solo les puedo hablar y escribir de lo poco que se y la verdad es que no se mucho. Por esa razón, esto no es un "curso", es un taller en el que, en mayor o en menor medida, todos vamos a aprender algo.

Lo primero que tienes que saber para convertirte en desarrollador de software, es que tienes que leer. Si, leer y leer mucho, y cuando creas que ya leíste suficiente entonces esa es la señal de que debes leer más. Leer para aprender, leer para mejorar lo que aprendiste, leer para corregir un error, leer para despues escribir, leer para enterarte de que no has leído suficiente

Elaborado por: Gersón D. Vizquel A.
Marzo 2018
Agradeciemiento a Erick Ramírez por sus aportes de valor
___

Para trabajar en equipo, desarrollando en cualquier lenguaje de programación, se debe convenir en como escribir el código. Ya existen estándares definidos por lacomunidad como los PSRx para PHP y el PEPx para python. Les recomiendo (POR FAVOR) revisarlos y ponerlos en practica, tanto para codificar como para documentar el código.

## 1- ¿Que es Django? ##

![El framework web para ponis con poderes magicos](djangopony.png)

Django es un framework construído en Python para desarrollo de aplicaciones web enfocado en la velocidad y la agilidad.

Django está centrado en el desarrollo rápido de aplicaciones web y sobre todo usando el principio de la programación DRY (No te repitas) y es algo importante en el core de este framework.

### 1.1- Caracterisiticas resaltantes ###

* Administrador: Django cuenta con un administrador que viene activo por defecto donde se pueden con un par de líneas de código mostrar los modelos de las bases de datos y poder crear, editar, ver y eliminar registros (CRUD).

* Formularios: Crear formularios en Django es muy sencillo y se pueden crear de dos formas: definiendo uno a uno los campos o usar un modelo de la base de datos y Django crea el formulario por nosotros.

* Rutas: El manejo de rutas hace que crear urls complejas sea sencillo de implementar, Django usa el poder de las expresiones regulares de python para hacer este trabajo. La versión 2 cuenta ademas con una mejorada y mas sencilla sintaxis para el manejo de las rutas.

* Autenticación: Django provee un sistema de gestión de usuarios y autenticación que permite que no nos preocupemos por crear un flujo de login y registro.

* Permisos: En Django se tiene control de los permisos a tal punto de decir que usuario puede o no crear, editar, ver y eliminar registros de un modelo especifico.

* Bases de Datos: Django cuenta con un ORM que nos permite preocuparnos en la lógica de nuestra aplicación dejando delega al ORM la responsabilidad de la comunicación con la base de datos, es compatible con los principales motores de bases de datos como PostgreSQL, MySQL, MariaDB, SQLite, Oracle, SQLServer entre otros.

* Extensible: Django puede ser extendido fácilmente instalando paquetes adicionales para crear aplicaciones una tienda, un blog o un API Restful, se encuentran agrupados y ordenados en Django Packages.

* Comunidad: Django tiene una gran comunidad que encuentras siempre en los foros de ayuda y listas de correos.

* Documentación: Django tiene una documentación muy completa que te enseña con ejemplos de código como implementar o usar cada una de sus características.

## 2- Requisitos par el taller ##

* Debian 9
* PostgreSQL
* python 3
* python3-pip
* python3-virtualenv
* git
* Editor de texto de su preferencia: VS Code, Atom, Vim, nano, vi, emacs

### 2.1- Instalar postgresql y crear base de datos de prueba ###

```console
user@nombre_maquina:~$ su -
root@nombre_maquina:/# aptitude install postgresql
root@nombre_maquina:/# su - postgres
postgres@nombre_maquina:~$ psql
postgres=# ALTER USER postgres with encrypted password '123456';
postgres=# CREATE DATABASE pruebadjango ENCODING 'UTF8';
Ctrl + d
postgres@nombre_maquina:~$ exit
root@nombre_maquina:/# /etc/init.d/postgresql restart
```

Con los siguientes comandos puedes hacer y restaurar respaldos comprimidos dePostgreSQL:

```console
pg_dump -F c -O -x -U usuario -W -h localhost -Z 6 -f /ruta/archivo.sqlc -d BaseDeDatos -n esquema -t tabla
pg_restore -F c -U usuario -W -h localhost /ruta/archivo.sqlc -d BaseDeDatos -n esquema -t tabla
```

### 2.2- Instalar python3 python3-pip python3-virtualenv ###

```console
user@nombre_maquina:~$ su -
root@nombre_maquina:/# aptitude install python3 python3-pip python3-virtualenv
```

## 3- Crear el entorno virtual ##

```console
root@nombre_maquina:/# cd /var/www
root@nombre_maquina:/var/www/$ virtualenv -p python3 envPrueba
root@nombre_maquina:/# chown usuario:root -R envPrueba
root@nombre_maquina:/# exit
```

### 3.1- Activar y actualizar el entorno virtual ###

```console
user@nombre_maquina:~$ cd /var/www/envPrueba
user@nombre_maquina:/var/www/envPrueba$ source bin/activate
(envPrueba) user@nombre_maquina:/var/www/envPrueba$ pip install -U virtualenv pip
```

## 4- Instalar Django, crear un nuevo provecto y una nueva aplicación ##

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba$ pip install Django
(envPrueba) user@nombre_maquina:/var/www/envPrueba$ django-admin startproject proyectoPrueba
(envPrueba) user@nombre_maquina:/var/www/envPrueba$ cd proyectoPrueba
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py startapp aplicacionPrueba
```

### 4.1- Registrar la nueva aplicación en el proyecto ###

Después de crear una aplicación también necesitamos decirle a Django que debe utilizarla. Con el editor de su preferencia abrir el archivo: var/www/envPrueba/proyectoPrueba/proyectoPrueba/settinds.py

En la sección "INSTALLED_APPS" agregar:

```python
INSTALLED_APPS = [
    'aplicacionPrueba',
    ...
]
```

Nota: si trabajas con git problamente quieras ignorar convenientemente algunos archivos (*.pyc). Es necesa para ello crea en tu directorio de proyecto el archivo .gitignore con el siguiente contenido:

```INI
*.egg-info
*.pot
*.py[co]
.tox/
*.pyc
__pycache__
MANIFEST
dist/
docs/_build/
docs/locale/
node_modules/
tests/coverage_html/
tests/.coverage
build/
secret.py
tests/report/
opsu/secret.py
```

Luego ejecuta en la consola el siguiente comando:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ find . -name "*.pyc" -exec git rm -f "{}" \;
```

Listo puedes comenzar a interactuar con tu repositorio de control de versiones git.

### 4.2- Modificar el bloque de conexión a la base de datos ###

* Instalar psycopg2 para poder conectar con las bases de datos PostgreSQL:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ pip install psycopg2-binary
```

Con el editor de su preferencia abrir el archivo: /var/www/envPrueba/proyectoPrueba/proyectoPrueba/settinds.py

Reemplazar la sección "DATABASES" por:

```python
DATABASES = {
    'default': {
    'ENGINE': 'django.db.backends.postgresql_psycopg2',
    'OPTIONS': {
        'options': '-c search_path=public'
    },
        'HOST': 'localhost',
        'NAME': 'pruebadjango',
        'PASSWORD': '123456',
        'PORT': '5432',
        'USER': 'postgres'
    }
}
```

### 4.3- Escribir en la base de datos las migraciones que vienen por defecto ###

Django viene con algunos módulos por defecto que facilitan mucho el desarrollo de aplicaciones. Algunos de estos módulos deben escribir algunas tablas en la base de datos, pero nosotros debemos darle la instrucción para que pueda hacerlo.

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py migrate
```

### 4.4- Crear un usuario súper administrador ###

Como he mencionado más arriba Django viene con varias capacidades por defecto. Una de ellas es que ya instalamos un completo y muy seguro sistema de gestión de usuarios. En este punto ya podemos crear un usuario para ir probando en el futuro próximo las aplicaciones en nuestro proyecto:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py createsuperuser
```

>Les advierto que la clave que escriban debe ser una clave de al menos 8 caracteres, no puede ser solo numérica y no puede ser una clave común. Si quieren poder poner una clave del 1 al 6 (NO LO RECOMIENDO) deben comentar el bloque de código AUTH_PASSWORD_VALIDATORS del archivo settings.py.

### 4.5- Modificar la internacionalización (idioma) ###

Con el editor de su preferencia abrir el archivo: /var/www/envPrueba/proyectoPrueba/proyectoPrueba/settinds.py

Ubicar el bloque internacionalización en el archivo settings.py y reemplazarlo por:

```python

LANGUAGE_CODE = 'es'

TIME_ZONE = 'America/Caracas'

```

### 4.6- Ubicación de archivos estáticos ###

También necesitaremos agregar una ruta para los archivos estáticos (aprenderemos todo sobre los archivos estáticos más tarde).

Con el editor de su preferencia abrir el archivo: /var/www/envPrueba/proyectoPrueba/proyectoPrueba/settinds.py

Ve hacia abajo hasta el final del archivo y reemplazar las lines MEDIA_URL, MEDIA_ROOT, STATIC_URL, STATIC_ROOT, por:

```python
STATIC_URL = '/static/'

MEDIA_URL = '/media/'

MEDIA_ROOT = os.path.join(BASE_DIR,'media')

STATIC_ROOT = 'static'
```

### 4.7- Probar nuestro proyecto ###

Ahora hay que probar todo a quedado bien instalado y configurado y que nuestro proyecto está funcionando. Para esto debemos levantar el servidor de desarrollo que viene junto a Django:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py runserver 127.0.0.1:8000
```

Si todo lo has hecho al pie de la letra y yo no cometí algún error de omisión deberían ver algo como esto:

```console
System check identified no issues (0 silenced).
March 25, 2018 - 12:15:56
Django version 2.x, using settings 'proyectoPrueba.settings'
Starting development server at http://127.0.0.1:8080/
Quit the server with CONTROL-C.
```

>Para ver nuestro proyecto en funcionamiento, con el navegador web de su preferencia, abra la url <http://127.0.0.1:8000/admin>

## 5- Modelando los datos ##

Los modelos de datos en Django son objetos (propiedades y métodos) que el mapeo objeto-relacional (ORM) usa para interactuar entre Django (python) y la base de datos. Existe una enorme cantidad de documentación para Django y para los modelos no hay excepción.

Cada aplicación dentro de nuestro proyecto contiene un archivo llamado models.py. Es en este archivo donde debemos escribir los objetos que representaran nuestro modelo de datos. Abre el archivo models.py que se encuentra en /var/www/envPrueba/proyectoPrueba/proyectoPrueba/aplicacionPrueba y reemplaza su contenido por:

```python
# -*- coding: utf-8
"""
Modelo de datos de la app globales
"""
from django.db import models

###############################################################################
class Pais(models.Model):
    """Modelo Pais
    Este objeto especifica los atributos y metodos necesarios para gestionar un
    registro de paises en nuestra aplicación. Esta basado en el ISO 3166-1. Para
    mayor información visite https://es.wikipedia.org/wiki/ISO_3166-1
    """
    cod_iso_pais = models.CharField(max_length=3, blank=True, null=True)
    nombre_pais = models.CharField(max_length=255, blank=True, null=True)
    nombre_pais_iso = models.CharField(max_length=255, blank=True, null=True)
    codigo_pais_alfa2 = models.CharField(max_length=2, blank=True, null=True)
    codigo_pais_alfa3 = models.CharField(max_length=3, blank=True, null=True)

    def __str__(self):
        return self.nombre_pais

    class Meta:
        db_table = 'pais'
        # verbose_name = 'País'
        # verbose_name_plural = 'Paises'

###############################################################################
class Estado(models.Model):
    """Modelo Estado
    Este objeto especifica los atributos y metodos necesarios para gestionar un
    registro de estados en Venezuela en nuestra aplicación. Esta basado en el
    ISO 3166-2:VE. Para mayor información visite
    https://en.wikipedia.org/wiki/ISO_3166-2:VE
    """
    nombre_estado = models.CharField(max_length=255)
    pais_id = models.ForeignKey(Pais, on_delete=models.PROTECT)

    def __str__(self):
        return self.nombre_estado

    class Meta:
        db_table = 'estado'

###############################################################################
class Municipio(models.Model):
    nombre_municipio = models.CharField(max_length=255)
    estado_id = models.ForeignKey(Estado, on_delete=models.PROTECT)

    def __str__(self):
        return self.nombre_municipio

    class Meta:
        db_table = 'municipio'

###############################################################################
class Parroquia(models.Model):
    nombre_parroquia = models.CharField(max_length=255)
    municipio_id = models.ForeignKey(Municipio, on_delete=models.PROTECT)

    def __str__(self):
        return self.nombre_parroquia

    class Meta:
        db_table = 'parroquia'

```

Luego de escribir nuestros modelos de datos, debemos migrarlos hacia nuestro sistema de gestión de bases de datos (DBMS) que puede ser SQL o NoSQL. Para ello debemos ejecutar el siguiente comando:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py makemigrations aplicacionPrueba
```

Este comando creara el script de base de datos necesarios para crear las tablas y sus relaciones en el DBMS que estemos utilizando (postgreSQL para este taller). Si quieres ver el script, ejecuta el siguiente comando:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py sqlmigrate aplicacionPrueba 0001
```

Luego hay que decirle a Django que ejecute ese script para que se hagan efectivos en el DBMS. El comando para esto es:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ ./manage.py migrate aplicacionPrueba 0001
```

>0001 es el numero secuencial asignado por Django a cada migración. La siguiente seria 0002 y la que le sigue 0003 y así sucesivamente.

Ahora puedes ir a tu DBMS y verificar que se crearon las tablas correspondientes a tús modelos Django.  Muchas mas cosas (atributos o propiedades y metodos) se pueden escribir en el archivo **models.py**, lee la documentación de tu preferencia para explorar todas las posibilidades.

## 6- Administrador de django ##

Una de las partes más poderosas de Django es la interfaz de administración automática. Django admin lee los metadatos de sus modelos para proporcionar una interfaz sencilla, rápida, centrada en el modelo y visualmente agradable.

Para agregar, editar y borrar datos a nuestros modelos, utilizaremos el administrador de Django. Abramos ahora el archivo admin.py qu se encuentra en la ruta /var/www/envPrueba/proyectoPrueba/proyectoPrueba/aplicacionPrueba y reemplaza su contenido por:

```python
"""Sección administrativa del modelo de datos
"""
from django.contrib import admin
from .models import Pais, Estado, Municipio, Parroquia


@admin.register(Pais)
class PaisAdmin(admin.ModelAdmin):
    """Clase admin del modelo Pais
    Con esta clase Admin se pueden controlar el modelo Pais desde el
    administrador de Django
    """
    search_fields = ['nombre_pais',]
    list_display = [
        'nombre_pais',
        'cod_iso_pais',
        'nombre_pais_iso',
        'codigo_pais_alfa2',
        'codigo_pais_alfa3',
        # 'link_relacionado',
    ]
    show_full_result_count = True
    actions_selection_counter = True

    # def link_relacionado(self, obj):
    #     url = '../estado/?q=&id_pais__exact='
    #     url_link = 'Ver Programas que Otorgan este Título'
    #     return '<a href="%s%s">%s</a>' % (url, obj.id, url_link)

    # link_relacionado.allow_tags = True

    # link_relacionado.short_description = 'Programas que Otorgan este Título'


@admin.register(Estado)
class EstadoAdmin(admin.ModelAdmin):
    pass


@admin.register(Municipio)
class MunicipioAdmin(admin.ModelAdmin):
    pass


@admin.register(Parroquia)
class ParroquiaAdmin(admin.ModelAdmin):
    pass
```

Ahora puedes volver a ingreasar en el admin de tú aplicación y podras ver como nuestros modelos ahora cuentan con una interce gráfica para listar, agregar, editar y eliminar registros.

Ya esta GUI es bastante buena, pero se puede mejorar un poco. En la línea de comandos escribe lo siguiente:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ pip install django-suit
```

Loque hicimos fue instalar un nuevo conjunto de plantillas para el Django admin. Ahora abre el archivo: /var/www/envPrueba/proyectoPrueba/proyectoPrueba/settinds.py y en la sección "INSTALLED_APPS" despues de la línea agregar:

```python
INSTALLED_APPS = [
    'aplicacionPrueba',
    'suit',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

Refresca tu admin en el navegador y veras ahora el admin con un nuevo aspecto mucho mas agradable.

## 7- Formularios ##

## 8- Vista: la logica de nuestro desarrollo ##

En Django una vista es un bloque de codigo que contiene algo de logica para nuestro software. Puede ser tan simple como renderizar en el navegador un ¡Hola Mundo! hasta operaciones de cualquier área de la matematica.

Modifiquemos un poco nuestras vistas para realizar un CRUD de los paises de nuestro modelo de datos.

Abre el archivo view.py de nuestra aplicación "***aplicacionPrueba***" y reemplaza el contenido con el siguiente código:

```python
""" Vistas: Lógica de nuestra aplicación de pruebas
"""
from django.views.generic.edit import CreateView
from django.views.generic.list import ListView
from django.shortcuts import render
from .models import Pais, Estado


###############################################################################
def index(request):
    """Vista para el landing page de nuestra aplicaicón
    """
    return render(request, 'index.html')


###############################################################################
def listar_pais(request):
    """Vista basada en funciones para listar los paises
    """
    contexto = {}  # Diccionario de contexto para la plantilla

    # Cargamos todos los paises de nuestro modelo
    contexto['pais'] = Pais.objects.all()

    # Renderizamos la lista de paises
    return render(request, 'listar_pais.html', contexto)


###############################################################################
class AgregarPais(CreateView):
    """Vista basada en Clase para agregar paises
    """
    model = Pais
    success_url = 'index'


###############################################################################
def editar_pais(request, id_registro=None):
    """Vista basada en funciones para editar los paises
    """
    contexto = {}  # Diccionario de contexto para la plantilla
    contexto['pais'] = Pais.objects.filter(pk=id_registro)
    return render(request, 'editar_pais.html', contexto)


###############################################################################
class ListarEstado(ListView):
    """Lista los estado de un pais
    """
    model = Estado
    template_name = 'listar_estado.html'

    def get_queryset(self):
        return Estado.objects.filter(pais=self.kwargs['pais'])

```

Estas son vistas muy sencilla. Para poder ver en funcionamiento cualquiera de nuestras vistas recien creadas debemos especificar cual es la URL (ruta) que nos permite acceder a ese recurso, veremos como crear nuestras URL's en el siguiente punto.

## 9- URL's ##

Una URL (Uniform Resource Locator) "... es una cadena de caracteres con la cual se asigna una dirección única a cada uno de los recursos de información disponibles en Internet. Localizador de recursos uniforme. (Sin fecha). En Wikipedia. Recuperado el 23 de marzo de 2018 de <https://es.wikipedia.org/wiki/Localizador_de_recursos_uniforme">

Para Django un URL es una cadena de caracteres que apunta un proceso que a su vez renderiza directa o inderectamentea una plantilla HTML en nuestro navegador web.

Django posee una estructura jerarquica en sus URL, primero hay un archivo urls.py en la aplicación del proyecto desde el que se importan las URL descritas en los archivos urls.py que están dentro del directorio de cada aplicación.

Si abres el archivo /var/www/envPrueba/proyectoPrueba/proyectoPrueba/urls.py veras una estructura similar a esta:

```python
"""proyectoPrueba URL Configuration
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('aplicacionPrueba/', include('aplicacionPrueba.urls')),
    path('admin/', admin.site.urls),
]
```

Por esta razon es que todas las rutas de nuestra aplicación presedidas por "admin/" funcionan, porque buscan todas las posibles conbinaciones que existen el er archivo urls.py de la aplicacion admin.

Ahora crearemos las URL de nuestra aplicación de prueba.

Con el editor de tú preferencia crea el archivo urls.py en la raiz de tú aplicación ***aplicacionPrueba*** y agregale el siguiente contenido:

```python
"""Rutas de nuestra aplicación
"""
from django.urls import path
from django.views.generic.list import ListView
from django.views.generic.edit import DeleteView
from . import views
from .views import AgregarPais, ListarEstado
from .models import Pais

app_name = 'prueba'

urlpatterns = [
    path('', views.index, name='index'),
    path('paises', views.listar_pais, name='listar-pais'),
    # path(
    #     'paises',
    #     ListView.as_view(model=Pais, template_name='listar_pais.html'),
    #     name='listar-pais'
    # ),
    path('agregar-pais', AgregarPais.as_view(), name='agregar-pais'),
    path('editar-pais/<int:idPais>', views.editar_pais, name='editar-pais'),
    path(
        'eliminar-pais/<int:pk>',
        DeleteView.as_view(
            model=Pais,
            template_name='pais_confirm_delete.html',
            success_url='../paises'
        ),
        name='eliminar-pais'
    ),
    path(
        'estados/<int:pais>', ListarEstado.as_view(), name='listar-estado'
    ),
]

```

Si levantas el servidor de desarrollo y vas consecuentemente a la ruta que creaste, veras el resultado de nuestra vista y nuestra ruta. Esta es una de las vistas y rutas mas sencillas que podemos realizar, sin embargo para una página web necesitaremos un poco mas que eso.

## 10- Plantillas ##

Ya vimos y aprendimos los conceptos de iniciación en el desarrollo de aplicaciones con Django. Aprendimos a codificar funcionalidades más interesantes relacionadas con operaciones de consulta a nuestros modelos de datos, algun procesamiento lógico un poco más exigente y el **renderizado en plantillas html** para mejorar la experiencia de los usuarios en nuestra aplicación.

Una plantilla Django es un bloque html con capacidades de herencia de otras plantillas y tags que permiten la carga dinámica de valores en renderizado.

Creemos la plantilla que renderiza nuestra ruta de acceso a la aplicación o index. Para ello dentro de la ***aplicacionPrueba*** debemos crear el directorio templates, y dentro de este crear el archivo index.html con el siguiente contenido:

```HTML
{% extends "base.html" %}

{% block title %}Index{% endblock %}

{% block contenido %}
<a href="{% url "prueba:listar-pais" %}">Pais</a>
{% endblock %}
```

Como estamos extendiendo otra plantilla (base.html), tambien debemos crear esa plantilla en el mismo directorio con el siguiente contenido:

```HTML
{% load staticfiles %}

<!DOCTYPE html>
<!--[if lt IE 7]>      <html class="no-js lt-ie9 lt-ie8 lt-ie7" lang=""> <![endif]-->
<!--[if IE 7]>         <html class="no-js lt-ie9 lt-ie8" lang=""> <![endif]-->
<!--[if IE 8]>         <html class="no-js lt-ie9" lang=""> <![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang=""> <!--<![endif]-->
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <title>Prueba - {% block title %}{% endblock %}</title>
    <meta name="description" content="">
    <link rel="apple-touch-icon" href="{% static "apple-touch-icon.png" %}">
    <link
        href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css"
        rel="stylesheet"
        integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4"
        crossorigin="anonymous"
    >
    <link
        href="https://cdn.datatables.net/1.10.16/css/jquery.dataTables.min.css"
        rel="stylesheet"
    >
    <link
        href="https://cdn.datatables.net/1.10.16/css/dataTables.bootstrap4.min.css"
        rel="stylesheet"
    >
    <link
        href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
        rel="stylesheet"
    >

    <script
        src="https://code.jquery.com/jquery-3.3.1.slim.min.js"
        integrity="sha256-3edrmyuQ0w65f8gfBsqowzjJe2iM6n0nKciPUp8y+7E="
        crossorigin="anonymous">
    </script>
    <script
        src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js">
    </script>
    <script
        src="https://cdn.datatables.net/1.10.16/js/dataTables.bootstrap4.min.js">
    </script>
  </head>
  <body>
    {% block contenido %}{% endblock contenido %}
    <!-- =================== Bootstrap core JavaScript ==================== -->
    <!-- ============ Latest compiled and minified JavaScript ============= -->
    <script
        src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/js/bootstrap.min.js"
        integrity="sha384-uefMccjFJAIv6A+rW+L4AHf99KvxDjWSu1z9VI8SKNVmz4sk7buKt/6v9KI65qnm"
        crossorigin="anonymous">
    </script>
    {% block javascripts %}{% endblock %}
  </body>
</html>
```

## 11- Seguridad ##

## 12- Django: despliegue ##

El despliegue o como dirían en el imperio mesmo "the deployment" con nginx es el camino empedrao', largo tedioso y, por ser menos común, con el que solemos tener menos experiencia y por tanto más dificultades. A pesar de esto lo explicamos primero porque según los entendidos es el que mejor rendimiento tiene a la hora de servir aplicaciones python.

En caso de que vayamos a mover la aplicación a un servidor de producción, es importante llevarnos los requisitos del sistema que no es mas que una lista de todo el software adicional que hemos utilizado para desarrollar nuestra aplicación. Para ello debemos ejecutar el siguiente comando:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ pip freeze > requerimientos.txt
```

Edita el archivo ***requerimientos.txt*** y elimina la línea *pkg-respurces==0.0.0*

En la nueva localidad de de nuestro proyecto, lo que debemos hacer es crear un entorno virtual como se explicó en los pasos 1.2 al 2.1. Luego en ese nuevo entorno virtual copiamos nuestra carpeta ***proyectoPrueba*** con todo su contenido. Ingresar a la carpeta ***proyectoPrueba*** y ejecutar el comando:

```console
pip install -r requerimientos.txt
```

### 12.1- Despliegue con nginx ###

>Lo primero que hay que hacer es instalar gunicorn. Gunicorn es un servidor WSGI HTTP para Python.

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ pip install gunicorn
```

Lo siguiente es crear el archivo ***gunicorn_start*** en /var/www/envPrueba/bin/ con el siguiente contenido:

```bash
#!/bin/bash

NAME="taller_django"                           # Nombre de la aplicación
DJANGODIR=/var/www/envPrueba/proyectoPrueba    # Directorio del proyecto Django
SOCKFILE=/var/www/envPrueba/run/gunicorn.sock  # Nos comunicaremos usando este socket unix
USER=usuario                                   # El usuario que ejecuta la app
GROUP=root                                     # El grupo que ejecuta la app
NUM_WORKERS=1                                  # Cuantos trabajadores procesaran Gunicorn (cant. de procesadores + 1).
DJANGO_SETTINGS_MODULE=proyectoPrueba.settings # Que archivo de configuración usará Django
DJANGO_WSGI_MODULE=proyectoPrueba.wsgi         # Nombre del módulo WSGI

echo "Iniciando $NAME as `whoami`"

# Activar el entorno virtual
cd $DJANGODIR
source ../bin/activate
export DJANGO_SETTINGS_MODULE=$DJANGO_SETTINGS_MODULE
export PYTHONPATH=$DJANGODIR:$PYTHONPATH

# Crea el directorio run en caso de que no exista
RUNDIR=$(dirname $SOCKFILE)
test -d $RUNDIR || mkdir -p $RUNDIR

# Iniciar la app Django con gunicorn
# Los programas que están supuestos a usar supervisor no deben usar demonios (no usar --daemon)
exec ../bin/gunicorn ${DJANGO_WSGI_MODULE}:application \
  --name $NAME \
  --workers $NUM_WORKERS \
  --user=$USER --group=$GROUP \
  --bind=unix:$SOCKFILE \
  --log-level=debug \
  --log-file=-
```

>Probamos el iniciador gunicorn:

```console
(envPrueba) user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ deactivate
user@nombre_maquina:/var/www/envPrueba/proyectoPrueba$ cd ~
user@nombre_maquina:~$ /var/www/envPrueba/bin/gunicorn_start
Ctrl + c
```

>Ahora debemos instalar el ***supervisor***. Supervisor es un sistema cliente/servidor que permite monitorear y controlar procesos en sistemas operativos tipo UNIX.

```console
user@nombre_maquina:~$ su -
root@nombre_maquina:/# aptitude install supervisor
```

>Luego creamos el archivo ***proyecto.conf*** en la ruta /etc/supervisor/conf.d/ con el siguiente contenido:

```apacheconf
[program:proyectoPrueba]
command = /var/www/envPrueba/bin/gunicorn_start                  ; Comando para iniciar la aplicación
user = root                                                      ; Usuario que corre la aplicación
stdout_logfile = /var/www/envPrueba/logs/gunicorn_supervisor.log ; Donde se escribirá la aplicación
redirect_stderr = true
autostart=true
autorestart=true
```

>Ahora cargamos nuestra configuración de ***supervisor***:

```console
root@nombre_maquina:/# supervisorctl reread
```

>Y también podemos probar los otros comandos de ***supervisor***

```console
root@nombre_maquina:/# supervisorctl update proyectoPrueba
root@nombre_maquina:/# supervisorctl status proyectoPrueba
root@nombre_maquina:/# supervisorctl stop proyectoPrueba
root@nombre_maquina:/# supervisorctl start proyectoPrueba
root@nombre_maquina:/# supervisorctl restart proyectoPrueba
```

>Ahora hay que configurar el host virtual de nginx... Si, yo tambien dije ¿hasta cuando?. Llamaremos al archivo proyectoPrueba.conf y debemos guardarlo en la ruta /etc/nginx/sites-available/, el contenido que debe tener ese archivo es:

```nginx
upstream proyectoPrueba_app_server {
  # fail_timeout=0 means we always retry an upstream even if it failed
  # to return a good HTTP response (in case the Unicorn master nukes a
  # single worker for timing out).

  server unix:/var/www/envPrueba/run/gunicorn.sock fail_timeout=0;
}

server {

    listen   80;
    server_name proyectoPrueba.org;

    client_max_body_size 4G;

    access_log /var/www/envPrueba/logs/nginx-access.log;
    error_log /var/www/envPrueba/logs/nginx-error.log;

    location /static/ {
        alias   /var/www/envPrueba/proyectoPrueba/static/;
    }

    location /media/ {
        alias   /var/www/envPrueba/proyectoPrueba/media/;
    }

    location / {
        # an HTTP header important enough to have its own Wikipedia entry:
        #   http://en.wikipedia.org/wiki/X-Forwarded-For
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        # enable this if and only if you use HTTPS, this helps Rack
        # set the proper protocol for doing redirects:
        # proxy_set_header X-Forwarded-Proto https;

        # pass the Host: header from the client right along so redirects
        # can be set properly within the Rack application
        proxy_set_header Host $http_host;

        # we don't want nginx trying to do something clever with
        # redirects, we set the Host: header above already.
        proxy_redirect off;

        # set "proxy_buffering off" *only* for Rainbows! when doing
        # Comet/long-poll stuff.  It's also safe to set if you're
        # using only serving fast clients with Unicorn + nginx.
        # Otherwise you _want_ nginx to buffer responses to slow
        # clients, really.
        # proxy_buffering off;

        # Try to serve static files from nginx, no point in making an
        # *application* server like Unicorn/Rainbows! serve static files.
        if (!-f $request_filename) {
            proxy_pass http://proyectoPrueba_app_server;
            break;
        }
    }

    # Error pages
    error_page 500 502 503 504 /500.html;
    location = /500.html {
        root /var/www/envPrueba/proyectoPrueba/static/;
    }
}
```

>Abrir el archivo /etc/hosts y agregar la siguinete línea:

```bash
127.0.0.1   proyectoPrueba.org
```

>Reiniciar el servicio de tu app

```console
root@nombre_maquina:/# supervisorctl start proyectoPrueba
```

>Puedes ir al navegador agregar la excepción para proyectoPrueba.org, escribir esa misma URL en la barra de busqueda y listo, apligación desplegada.

### 12.2- Despliegue con apache2 ###

>Modificar el archivo wsgy.py que está en /var/www/envPrueba/proyectoPrueba/proyectoPrueba/wsgy.py con el siguiente contenido:

```python
import os, sys

sys.path.append("/var/www/envPrueba/proyectoPrueba/proyectoPrueba")

from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "proyectoPrueba.settings")

application = get_wsgi_application()
```

>Se debe crear ahora el virtual host de apache en /etc/apache2/sites-available/proyectoPrueba.conf y colocarle el siguiente contenido:

```apacheconf
<VirtualHost *:80>
        WSGIDaemonProcess proyectoPrueba.org python-home=/var/www/envPrueba python-path=/www/envOpsu/myapp
        WSGIProcessGroup  proyectoPrueba.org
        WSGIScriptAlias / /var/www/envPrueba/proyectoPrueba/proyectoPrueba/wsgi.py

        ServerName proyectoPrueba.org

        ServerAdmin webmaster@midominio
        DocumentRoot /var/www/envPrueba

        Alias /static "/var/www/envPrueba/proyectoPrueba/static/"
        <Directory /var/www/envPrueba/proyectoPrueba/static>
           Require all granted
        </Directory>

        Alias /media "/var/www/envPrueba/proyectoPrueba/media/"
        <Directory /var/www/envPrueba/proyectoPrueba/media>
           Require all granted
        </Directory>

        <Directory /var/www/envPrueba/proyectoPrueba/opsu>
           <Files wsgi.py>
              Require all granted
           </Files>
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

>Activar el sitio:

```console
root@nombre_maquina:/# a2ensite proyectoPrueba.conf
```

>Reiniciar apache:

```console
root@nombre_maquina:/# /etc/init.d/apache2 reload
```

___

#### Fuentes consultadas ####

<https://docs.djangoproject.com>
<https://www.digitalocean.com/community/tutorial_series/django-development>
<https://tutorial.djangogirls.org>
<https://stackoverflow.com/>
<https://es.stackoverflow.com/>
<http://www.maestrosdelweb.com/curso-django-despliegue-en-el-servidor-web/>
<https://simpleisbetterthancomplex.com>

Autoría:
Esta presentación  fue elaborada  por Gersón D Vizquel A. El y/o los autores a su vez son los editores del texto. Las contribuciones de terceras personas serán aceptadas o rechazadas y en su caso reconocidas en este apartado.

Cómo citar este documento:
Vizquel G (2018). Django: Taller teórico práctico.

Licencia:

>Copyleft   2017.  Gersón D. VIzquel A., bajo las siguientes licencias:
CC BY- NC-SA: Creative Commons Reconocimiento-NoComercial-CompartirIgual 4.0 Internacional (CC BY-NC-SA 4.0)
Puede encontrarse la licencia completa en: <https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es_ES>
>
>![CC BY- NC-SA](imagenes/by-nc-sa.svg)

Usted es libre de:

Compartir — copiar y redistribuir el material en cualquier medio o formato.

Adaptar — remezclar, transformar y crear a partir del material

El licenciador no puede revocar estas libertades mientras cumpla con los términos de la licencia. Bajo las condiciones siguientes:

![Reconocimiento](imagenes/by.svg) Reconocimiento — Debe reconocer adecuadamente la autoría, proporcionar un enlace a la licencia e indicar si se han realizado cambios. Puede hacerlo de cualquier manera razonable, pero no de una manera que sugiera que tiene el apoyo del licenciador o lo recibe por el uso que hace.

![NoComercial](imagenes/nc.svg) NoComercial — No puede utilizar el material para una finalidad comercial.

![CompartirIgual](imagenes/cc.svg) CompartirIgual — Si remezcla, transforma o crea a partir del material, deberá difundir sus contribuciones bajo la misma licencia que el original.
