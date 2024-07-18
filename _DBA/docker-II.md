---
title: SQL Server en Docker (II)
layout: default
nav_exclude: true

---

# SQL Server en Docker





Este artículo te sirve para ejecutar SQL Server en Docker.

Para ello necesitas:

- Visual Studio Code [Instalación Visual Studio Code](https://code.visualstudio.com/download)
- Docker [Descarga](https://docs.docker.com/engine/install/)
- SQL Server Management Studio (opcional) [Descarga](https://learn.microsoft.com/es-es/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)
- Azure Data Studio (opcional) [Descarga](https://learn.microsoft.com/es-es/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall#download-azure-data-studio)


---


<details open markdown="block">
  <summary>Tabla de Contenidos</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Configuración y arrancar instancia de SQL Server en Docker

Estos tres enlaces te servirán para introducirte en Docker y sus beneficios:
- [Qué es Docker](https://learn.microsoft.com/es-es/dotnet/architecture/microservices/container-docker-introduction/docker-defined)
- [Terminología Docker](https://learn.microsoft.com/es-es/dotnet/architecture/microservices/container-docker-introduction/docker-terminology)
- [Contenedores, imágenes, y registros de Docker](https://learn.microsoft.com/es-es/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)


[Opcional] - [Pdf sobre arquitecturas de micro servicios en Azure](https://github.com/dotnet-architecture/eBooks/raw/main/current/microservices/NET-Microservices-Architecture-for-Containerized-NET-Applications.pdf?WT.mc_id=dotnet-35129-website)


Estos videos también te pueden servir (para empezar no son necesarios todos):

- [Docker para principiantes - inglés](https://www.youtube.com/watch?v=pg19Z8LL06w)
- [Docker para principiantes - español](https://www.youtube.com/watch?v=SMqdC6g6Y2o&t=5159s)
- [Ejecutar SQL Server en Docker](https://www.youtube.com/watch?v=mgS_AdpBWz4)

Nota. Abajo en la sección comandos tienes una lista de instrucciones más habituales para empezar a usar Docker desde línea de comando.

---


https://learn.microsoft.com/es-es/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&pivots=cs1-bash
https://learn.microsoft.com/es-es/sql/linux/quickstart-sql-server-containers-azure?view=sql-server-ver16&tabs=kubectl




Para instalar SQL Server en Docker necesitas:

- Elegir la imagen de SQL Server.
- Definir los parámetros de la instalación, e iniciar el contenedor.
- Comprobar imágenes y contenedores en marcha.

### Elegir la imagen de SQL Server.

Las imagenes se [bajan de Docker](https://hub.docker.com/).
Elige la [imagen de SQL Server](https://hub.docker.com/_/microsoft-mssql-server) que quieres instalar.

Puedes descargar la imagen con este comando:

```powershell
docker pull mcr.microsoft.com/mssql/server:2022-latest
```

Si no descargas la imagen y haces referencia a ella, el comando docker run se encarga de bajarla automáticamente.

### Definir los parámetros de la instalación, e iniciar el contenedor.

En esa misma página en la sección **Configuration** tienas las instrucciones para arrancar la máquina correspondiente. Un ejemplo:

```powershell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Developer" -p 1440:1433  --name sql22c --hostname sql22c -d mcr.microsoft.com/mssql/server:2022-latest
```

Si necesitas conectar a SQL Server vía sqlcmd, utiliza este comando:

```powershell
docker exec -it <container_id|container_name> /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P <your_password>
```

Si ves algún comportamiento no esperado - bug - revisa [este foro en GitHub](https://github.com/Microsoft/mssql-docker). 



### Comprobar imágenes y contenedores en marcha.

Listar imagenes descargadas:

```powershell
docker images
```

| REPOSITORY                  | TAG         | IMAGE ID    | CREATED      | SIZE   |
|-----------------------------|-------------|-------------|--------------|--------|
| ubuntu                      | latest      | 3db8720ecbf5| 11 days ago  | 77.9MB |
| mcr.microsoft.com/mssql/server | 2019-latest | 2849e65c6fc6| 3 weeks ago  | 1.47GB |
| mcr.microsoft.com/mssql/server | latest      | ffdd6981a89e| 3 months ago | 1.58GB |


Estos comandos te servirán para gestionar contenedores e imágenes:

```powershell
echo '---'
echo 'listado de imagenes'
echo '---'
docker images
```
![listado de imagenes](../docker-I/imagenes.png)
---

```powershell
echo '---'
echo 'listado de contenedores activos y parados'
docker ps -a
```
![](../docker-I/contenedores.png)
El contenedor sql22b está parado, mientras que los contenedores sql22a, y sql_server_demo están activos. Los dos primeros son de SQL Server 2022, mientras que el tercero es de SQL 2019.
Además, nota los puertos por los que atiende peticiones el contenedor. sql22a "desvia" el puerto por defecto de SQL Server 1433, al puerto 1436. Si tratas de iniciar un contenedor con un puerto ya existente, dará un error como este:

```powershell
docker: Error response from daemon: driver failed programming external connectivity on endpoint sql22c (680c6859adef528f11119b64c72bc8115d38f3e5bf5f0a7dcd5ade2af60ddd1b): Bind for 0.0.0.0:1436 failed: port is already allocated.
```

Ten en cuenta que aunque no se haya iniciado el contenedor, si aparece como creado - pero en estado parado:
![alt text](../docker-I/imagePS2.png)

Si quieres cambiar el puerto de un contenedor, no está soportado en Docker porque es que el mapeo de puertos es parte de la configuración inmutable del contenedor, definida en el momento de su creación. Esta configuración incluye, entre otras cosas, la imagen base del contenedor, sus variables de entorno, volúmenes, y los puertos expuestos y mapeados. Una vez que un contenedor se crea con una determinada configuración, ciertos aspectos de esta, como el mapeo de puertos, no pueden modificarse.

Hay métodos no recomendados por Docker, como la edición directa de archivos de configuración internos de Docker (por ejemplo, los archivos hostconfig.json dentro de /var/lib/docker/containers/<container-id>/), pero no lo uses.

Para este caso, tendrás que eliminar el contenedor erróneo:

```powershell
echo '---'
echo 'eliminar contenedor '
docker rm sql22c
```

y volver a crearlo con el puerto correcto:

```powershell
echo '---'
echo 'crear e iniciar contenedor'
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Developer" -p 1440:1433  --name sql22c --hostname sql22c -d mcr.microsoft.com/mssql/server:2022-latest
```

Quedando algo como esto:

![alt text](../docker-I/imagePS3.png)


## Ver contenedores e imágenes desde Docker Desktop
Si empiezas con Docker, es probable que te cueste coger soltura con las instrucciones de línea de comando al principio.
Es bueno que las domines bien. También tienes aplicación de escritorio (Docker Desktop) para gestionar los contenedores:

![alt text](../docker-I/dockerDesktop.png)

En el menú Action, puedes acceder a los logs de cada contenedor, donde podrás ver todos los parámetros de inicio de la instancia de SQL Server como muestra la imagen:

![alt text](../docker-I/dockerDesktopContainerLog.png)



## Cuestiones avanzadas

### Asignar memoria máxima al contenedor
Por defecto, la memoria asignada al contenedor no está limitada. Si quieres configurar memoria máxima para el contenedor utiliza el argumento -m:

```powershell
echo '---'
echo ' memoria máxima'
docker run -m 2000m <resto de parámetros>
```
Para comprobar la memoria asginada, utiliza el argumento inspect:


```bash
{% raw %}
echo '---'
echo ' comprobar memoria asignada'
docker inspect --format='{{json .HostConfig.Memory}}' $ContainerName
{% endraw %}
```

Para comprobar toda la configuración:
```bash
{% raw %}
echo '---'
echo ' comprobar memoria asignada'
docker inspect --format='{{json .}}' $ContainerName
{% endraw %}
```

![alt text](../docker-I/dockerInspect.png)

Donde ves, que la memoria asignada es 2GB.
Más info de la opción [inspect](https://docs.docker.com/reference/cli/docker/inspect/). Donde en la documantación aparece esta info que me ha ayudado a encontrar la opción de consultarlo por código:
![alt text](../docker-I/dockerInspectHelp.png)
Inspect también se puede ver en el GUI:
![alt text](../docker-I/dockerInspectDesktop.png)


### Estructura de capas y ubicación de los archivos

La estructura de capas de archivos en Docker es un concepto fundamental que permite a Docker ser eficiente en el almacenamiento y la distribución de imágenes. Esta estructura se basa en la idea de que las imágenes se construyen en capas superpuestas unas sobre otras, donde cada capa representa un conjunto de diferencias o cambios respecto a la capa inferior. Veamos cómo funciona esto en detalle:

### Imágenes y Capas

- **Imágenes Base**: Todo comienza con una imagen base, que es la primera capa de una imagen y no tiene una capa subyacente. Por ejemplo, una imagen de Ubuntu podría servir como una imagen base sobre la cual se construyen otras imágenes.
- **Capas Subsiguientes**: Cada comando en un Dockerfile crea una nueva capa en la imagen. Por ejemplo, instalar un paquete de software, copiar archivos al contenedor o configurar ajustes crea una nueva capa que solo contiene las diferencias respecto a la capa anterior.
- **Reutilización de Capas**: Docker reutiliza las capas entre imágenes siempre que sea posible. Si dos imágenes se construyen a partir de la misma imagen base y comparten algunos comandos de configuración, esas capas comunes se almacenan una sola vez en el disco, lo que ahorra espacio y acelera las descargas y las construcciones de imágenes.

### Cómo Funciona

1. **Construcción de Imágenes**: Cuando construyes una imagen con un Dockerfile, cada instrucción en el Dockerfile crea una nueva capa. Por ejemplo:
   - `FROM ubuntu:18.04` comienza con una imagen base de Ubuntu.
   - `RUN apt-get update && apt-get install -y nginx` instala Nginx y crea una nueva capa con los cambios.
   - `COPY . /var/www/html` copia archivos desde tu directorio actual al contenedor, creando otra capa.

2. **Almacenamiento de Capas**: Internamente, Docker almacena cada capa en una ubicación del sistema de archivos del host y gestiona un índice que mapea las capas a las imágenes. Las capas son inmutables, lo que significa que una vez creadas, no se modifican. Si necesitas hacer un cambio, Docker crea una nueva capa con ese cambio.

3. **Ejecución de Contenedores**: Cuando inicias un contenedor a partir de una imagen, Docker utiliza un sistema de archivos unión para superponer todas las capas de la imagen de forma que aparezcan como un solo sistema de archivos coherente. Los cambios realizados en el contenedor en ejecución se almacenan en una nueva capa de escritura que es específica del contenedor, permitiendo que el contenedor modifique archivos y directorios como si estuviera en un sistema de archivos normal, sin alterar las capas subyacentes de la imagen.

### Ventajas

- **Eficiencia en el Almacenamiento**: Al compartir capas entre imágenes, Docker puede minimizar el uso del espacio en disco.
- **Rapidez**: La reutilización de capas también acelera la descarga de imágenes y la creación de contenedores, ya que solo se necesitan transferir o construir las capas que faltan.
- **Gestión de Versiones**: Las capas proporcionan una forma natural de gestionar versiones de una imagen, donde cada capa representa un cambio o actualización.







docker run -m 500m -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Developer" -p 1440:1433  --name sql22c --hostname sql22c -d mcr.microsoft.com/mssql/server:2022-latest


También puedes  varias instrucciones de línea de comando sean un poco com



## Conectar desde Azure Data Studio






docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Developer" -p 1436:1433  --name sql22a --hostname sql22a -d mcr.microsoft.com/mssql/server:2022-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Standard" -p 1438:1433  --name sql22b --hostname sql22b -d mcr.microsoft.com/mssql/server:2022-latest




Iniciar Docker con una imagen que no está des








- Bajar la imagen de 



\\wsl.localhost\docker-desktop-data\data\docker\overlay2\d6bf0ab9f5134a046b6bf9509c7b81e58fc3e9ca8683a324a4d67ac3a5e6161f\diff\var\opt\mssql\data





## Comandos Docker

| Categoría          | Comando y Ejemplo                                   | Descripción                                                                                                                                |
|--------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| **Gestión de imágenes** | `docker build -t mi-imagen:tag`                   | Construye una imagen Docker llamada `mi-imagen` con el tag `tag` desde un Dockerfile en el directorio actual.                              |
|                    | `docker pull ubuntu`                                | Descarga la imagen `ubuntu` desde Docker Hub.                                                                                              |
|                    | `docker push mi-imagen:tag`                         | Sube la imagen `mi-imagen` con el tag `tag` a Docker Hub o a otro registro configurado.                                                    |
|                    | `docker images`                                     | Lista todas las imágenes Docker disponibles localmente.                                                                                    |
|                    | `docker rmi mi-imagen:tag`                          | Elimina la imagen `mi-imagen` con el tag `tag`.                                                                                            |
| **Gestión de contenedores** | `docker run -d --name mi-contenedor nginx`          | Crea y arranca un contenedor en modo detenido (background) llamado `mi-contenedor` usando la imagen `nginx`.                               |
|                    | `docker stop mi-contenedor`                         | Detiene el contenedor llamado `mi-contenedor`.                                                                                             |
|                    | `docker start mi-contenedor`                        | Arranca el contenedor detenido llamado `mi-contenedor`.                                                                                    |
|                    | `docker restart mi-contenedor`                      | Reinicia el contenedor llamado `mi-contenedor`.                                                                                            |
|                    | `docker rm mi-contenedor`                           | Elimina el contenedor llamado `mi-contenedor`.                                                                                             |
|                    | `docker ps`<br>`docker ps -a`                       | Lista los contenedores en ejecución. Usar `docker ps -a` para listar todos los contenedores, incluyendo los detenidos.                     |
| **Gestión de redes**  | `docker network create mi-red`                      | Crea una red llamada `mi-red`.                                                                                                             |
|                    | `docker network ls`                                 | Lista todas las redes Docker disponibles.                                                                                                  |
|                    | `docker network rm mi-red`                          | Elimina la red llamada `mi-red`.                                                                                                           |
|                    | `docker network connect mi-red mi-contenedor`       | Conecta el contenedor `mi-contenedor` a la red `mi-red`.                                                                                   |
|                    | `docker network disconnect mi-red mi-contenedor`    | Desconecta el contenedor `mi-contenedor` de la red `mi-red`.                                                                               |
| **Gestión de volúmenes** | `docker volume create mi-volumen`                   | Crea un volumen llamado `mi-volumen`.                                                                                                      |
|                    | `docker volume ls`                                  | Lista todos los volúmenes Docker disponibles.                                                                                              |
|                    | `docker volume rm mi-volumen`                       | Elimina el volumen llamado `mi-volumen`.                                                                                                   |
| **Docker Compose**  | `docker-compose up`                                 | Arranca los servicios definidos en el archivo `docker-compose.yml` en el directorio actual.                                                |
|                    | `docker-compose down`                               | Detiene y elimina los recursos (contenedores, redes, volúmenes) definidos en `docker-compose.yml`.                                         |
|                    | `docker-compose logs`                               | Muestra los logs de todos los servicios definidos en `docker-compose.yml`.                                                                 |
|                    | `docker-compose build`                              | Construye o reconstruye servicios especificados en `docker-compose.yml`.                                                                   |







## Videos adicionales

- [Curso de 16 videos de Docker](https://www.youtube.com/playlist?list=PLQpe1zyko1phYOibEoOksV4QX0R0WpVln)



