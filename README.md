# Consúltenos

Sistema Django para administrar tiques de una mesa de ayuda.

## Requisitos

- Python 3.10 o posterior.
- Dependencias instaladas desde `requirements.txt`.

## Instalación y ejecución local

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
$env:DJANGO_DEBUG = "1"
$env:DJANGO_ALLOWED_HOSTS = "127.0.0.1,localhost"
python manage.py migrate
python manage.py crear_datos_demo
python manage.py runserver
```

`DJANGO_SECRET_KEY` debe configurarse como variable de entorno en despliegues.
Si se omite, el proceso genera una clave aleatoria de desarrollo que cambia
al reiniciar. `DJANGO_FORCE_HTTPS=1` activa la redirección HTTPS y las cookies
seguras; no debe activarse al usar `runserver` sin TLS. Django lee variables
del entorno del proceso; `.env.example` es una plantilla, no se carga
automáticamente.

## Comprobaciones

```powershell
python manage.py check
$env:DJANGO_SECRET_KEY = "<clave-de-despliegue>"
$env:DJANGO_DEBUG = "0"
$env:DJANGO_FORCE_HTTPS = "1"
python manage.py check --deploy
python manage.py test mesa_ayuda -v 2
```

Los registros de seguridad se escriben en `logs/seguridad.log` sin RUT,
teléfonos, correos ni observaciones de los tiques.

El acceso se bloquea después de cinco intentos fallidos durante 15 minutos;
un inicio correcto reinicia los intentos. El control usa `django-axes` y sus
migraciones se aplican con `python manage.py migrate`.
