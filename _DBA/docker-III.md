---
title: Postgres en Docker
layout: default
nav_order: 3
parent: Data - Dev

---

# Postgres en Docker





Este artículo te sirve para ejecutar SQL Server en Docker.
Se asume que has aprendido ya los scripts que utilizamos para crear imagen de docker en SQL Server. Esta vez, creaemos la imagen sobre un volumen (mount point).

Para ello necesitas:

- Visual Studio Code [Instalación Visual Studio Code](https://code.visualstudio.com/download)
- Docker [Descarga](https://docs.docker.com/engine/install/)
- Azure Data Studio (opcional) [Descarga](https://learn.microsoft.com/es-es/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall#download-azure-data-studio)


---


<details open markdown="block">
  <summary>Tabla de Contenidos</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Crear volumen

Este comando creará un volumen llamado vol_docker20 que utilizará la ruta C:\VMs\postgres20_data en tu sistema Windows como su ubicación de almacenamiento.


```powershell
docker volume create `
  --driver "local" `
  --opt "type=none" `
  --opt "device=C:\VMs\postgres20_data" `
  --opt "o=bind" `
  vol_docker20

```

Desglose del comando:
- --driver local: Utiliza el driver local para el volumen.
- --opt type=none: No especifica un tipo de sistema de archivos, ya que estamos usando una ubicación existente.
- --opt device="C:\VMs\postgres20_data": Especifica la ruta concreta en el host (Windows) donde se almacenará el volumen.
- --opt o=bind: Usa la opción de bind mount para enlazar la ruta especificada en el host al volumen de Docker.
- vol_docker20: El nombre que deseas asignar al volumen.


## Crear contenedor postgreSQL


```powershell
docker create `
  --name postgres20 `
  -e POSTGRES_PASSWORD=P@ssword! `
  -v vol_docker20:/var/lib/postgresql/data `
  -p 5555:5432 `
  postgres
```
Desglose del comando:
- docker create: Crea un contenedor a partir de una imagen pero no lo inicia.
- --name postgres20: Asigna el nombre postgres20 al contenedor.
- -e POSTGRES_PASSWORD=P@ssword!: Establece la variable de entorno POSTGRES_PASSWORD para definir la contraseña del usuario postgres.
- -v vol_docker20:/var/lib/postgresql/data: Monta el volumen vol_docker20 en el directorio /var/lib/postgresql/data del contenedor. Esta es la ruta por defecto donde PostgreSQL almacena sus datos.
postgres: Especifica la imagen de Docker para PostgreSQL que se utilizará (la imagen oficial).
- -p 5555:5432: Mapea el puerto 5432 del contenedor (puerto por defecto de PostgreSQL) al puerto 5555 en el host. Esto significa que podrás acceder a PostgreSQL desde tu máquina host en el puerto 5555.

## Iniciar el contenedor

```powershell
docker start postgres20
```

## Listar contenedores en marcha

```powershell
docker ps
```

## Listar contenedores (apagados también)

```powershell
docker ps -a
```

## Parar el contenedor

```powershell
docker stop -t 15 postgres20
```

Este comando envía una señal al contenedor postgres20 para detenerse de manera ordenada. Si el contenedor no se detiene después de un tiempo predeterminado (10 segundos por defecto), Docker lo forzará a detenerse. Si deseas ajustar este tiempo de espera, puedes usar el parámetro -t seguido del número de segundos para definir el tiempo de espera antes de forzar la detención. Por ejemplo:


## Borrar contenedor

```powershell
docker rm postgres20
```

## borrrar volumen


```powershell
docker volume rm vol_docker20
```
Borrar el volumen no implica que se borre el contenido en el sistema de archivos.



## Videos adicionales

- [Curso de 16 videos de Docker](https://www.youtube.com/playlist?list=PLQpe1zyko1phYOibEoOksV4QX0R0WpVln)



