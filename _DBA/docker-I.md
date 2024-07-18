---
title: SQL Server en Docker
layout: default
nav_order: 1
parent: Data - Dev

---

# SQL Server en Docker

<details open markdown="block">
  <summary>Tabla de Contenidos</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Motivación

La importancia de Docker radica en:

- **Portabilidad y Consistencia**: Docker asegura que las aplicaciones se ejecuten de manera consistente en cualquier entorno, eliminando discrepancias entre entornos de desarrollo, prueba y producción.

- **Eficiencia de Recursos**: Los contenedores Docker utilizan menos recursos que las máquinas virtuales tradicionales al compartir el sistema operativo del host, lo que resulta en un mejor aprovechamiento de los recursos del sistema.

- **Facilita la Integración y Entrega Continuas (CI/CD)**: Docker simplifica el proceso de CI/CD permitiendo a los desarrolladores empaquetar aplicaciones con todas sus dependencias en contenedores, lo que acelera las pruebas y el despliegue.

- **Ecosistema Amplio**: Docker proporciona un ecosistema robusto con herramientas como Docker Compose y Docker Hub, facilitando la gestión de contenedores y el intercambio de imágenes.

- **Demanda del Mercado**: La adopción generalizada de Docker en la industria tecnológica ha incrementado la demanda de profesionales con conocimientos en Docker, convirtiéndolo en una habilidad valiosa para el desarrollo de carreras en tecnología.

En función del perfil profesional (Data Developer o Data Admin):

### Para un Data Developer.

- **Entorno de Desarrollo Consistente**: Docker asegura que todos los miembros del equipo trabajen en un entorno de desarrollo uniforme y aislado, lo que minimiza las discrepancias entre los entornos de desarrollo, pruebas y producción, y facilita la integración y entrega continuas.

- **Rapidez en Prototipado y Pruebas**: Permite la creación rápida de instancias de SQL Server para probar nuevas características, modelos de datos o migraciones, sin afectar las configuraciones locales o requerir hardware adicional.

- **Flexibilidad y Escalabilidad**: Facilita la experimentación con arquitecturas de datos y modelos de escalabilidad, permitiendo desplegar y gestionar fácilmente múltiples contenedores para pruebas de carga y rendimiento.

### Para un Data Admin.

- **Gestión Eficiente de Recursos**: Docker ofrece una manera eficiente de desplegar y escalar servicios de SQL Server en entornos de producción, optimizando el uso de recursos a través del aislamiento de contenedores y la gestión centralizada.

- **Seguridad y Aislamiento**: Mejora la seguridad mediante el aislamiento de instancias de base de datos, lo que permite ejecutar múltiples versiones o instancias de SQL Server en el mismo host físico sin riesgo de interferencia, facilitando también la separación entre entornos de desarrollo, pruebas y producción.

- **Simplificación de Despliegues y Migraciones**: Docker simplifica el proceso de actualización y migración de bases de datos al permitir el despliegue de contenedores con nuevas versiones de SQL Server de manera rápida y consistente, reduciendo el tiempo de inactividad y los errores asociados con las migraciones tradicionales.


## Herrramientas necesarias.

- Visual Studio Code [Instalación Visual Studio Code.](https://code.visualstudio.com/download)
- Docker. [Descarga.](https://docs.docker.com/engine/install/)
- SQL Server Management Studio. (opcional) [Descarga.](https://learn.microsoft.com/es-es/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)
- Azure Data Studio. (opcional) [Descarga.](https://learn.microsoft.com/es-es/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall#download-azure-data-studio)


## Configuración y arrancar instancia de SQL Server en Docker.

Estos tres enlaces te servirán para introducirte en Docker y sus beneficios:
- [Qué es Docker.](https://learn.microsoft.com/es-es/dotnet/architecture/microservices/container-docker-introduction/docker-defined)
- [Terminología Docker.](https://learn.microsoft.com/es-es/dotnet/architecture/microservices/container-docker-introduction/docker-terminology)
- [Contenedores, imágenes, y registros de Docker.](https://learn.microsoft.com/es-es/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)
- [Pdf sobre arquitecturas de micro servicios en Azure.](https://github.com/dotnet-architecture/eBooks/raw/main/current/microservices/NET-Microservices-Architecture-for-Containerized-NET-Applications.pdf?WT.mc_id=dotnet-35129-website) (Opcional)


Estos videos también te pueden servir (para empezar no son necesarios todos):

- [Docker para principiantes - inglés.](https://www.youtube.com/watch?v=pg19Z8LL06w)
- [Docker para principiantes - español.](https://www.youtube.com/watch?v=SMqdC6g6Y2o&t=5159s)
- [Ejecutar SQL Server en Docker.](https://www.youtube.com/watch?v=mgS_AdpBWz4)

Nota. Abajo en la sección "Comandos Docker" tienes una lista de instrucciones más habituales para empezar a usar Docker desde línea de comando.

---
Para montar un contenedor de SQL Server en Docker necesitas:

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

Estos comandos te servirán para gestionar contenedores e imágenes:

Listar imagenes descargadas:


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

![alt text](../docker-I/imagenPS3.png)


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


![alt text](../docker-I/dockerInspect.png)

Donde ves, que la memoria asignada es 2GB.

Si quieres ver toda la configuración cambia la opción json:
```bash
{% raw %}
echo '---'
echo ' archivo de configuración completo en json'
docker inspect --format='{{json .}}' $ContainerName
{% endraw %}
```


Más info de la opción [inspect](https://docs.docker.com/reference/cli/docker/inspect/). Donde en la documantación aparece esta info que me ha ayudado a encontrar la opción de consultarlo por código:
![alt text](../docker-I/dockerInspectHelp.png)
Inspect también se puede ver en el GUI:
![alt text](../docker-I/dockerInspectDesktop.png)


### Estructura de capas y ubicación de los archivos.

La estructura de capas de archivos en Docker es un concepto fundamental que permite a Docker ser eficiente en el almacenamiento y la distribución de imágenes. Esta estructura se basa en la idea de que las imágenes se construyen en capas superpuestas unas sobre otras, donde cada capa representa un conjunto de diferencias o cambios respecto a la capa inferior. Veamos cómo funciona esto en detalle:

### Imágenes y capas.

- **Imágenes Base**: Todo comienza con una imagen base, que es la primera capa de una imagen y no tiene una capa subyacente. Por ejemplo, una imagen de Ubuntu podría servir como una imagen base sobre la cual se construyen otras imágenes.
- **Capas Subsiguientes**: Cada comando en un Dockerfile crea una nueva capa en la imagen. Por ejemplo, instalar un paquete de software, copiar archivos al contenedor o configurar ajustes crea una nueva capa que solo contiene las diferencias respecto a la capa anterior.
- **Reutilización de Capas**: Docker reutiliza las capas entre imágenes siempre que sea posible. Si dos imágenes se construyen a partir de la misma imagen base y comparten algunos comandos de configuración, esas capas comunes se almacenan una sola vez en el disco, lo que ahorra espacio y acelera las descargas y las construcciones de imágenes.

### Cómo Funciona.

1. **Construcción de Imágenes**: Cuando construyes una imagen con un Dockerfile, cada instrucción en el Dockerfile crea una nueva capa. Por ejemplo:
   - `FROM ubuntu:18.04` comienza con una imagen base de Ubuntu.
   - `RUN apt-get update && apt-get install -y nginx` instala Nginx y crea una nueva capa con los cambios.
   - `COPY . /var/www/html` copia archivos desde tu directorio actual al contenedor, creando otra capa.

2. **Almacenamiento de Capas**: Internamente, Docker almacena cada capa en una ubicación del sistema de archivos del host y gestiona un índice que mapea las capas a las imágenes. Las capas son inmutables, lo que significa que una vez creadas, no se modifican. Si necesitas hacer un cambio, Docker crea una nueva capa con ese cambio.

3. **Ejecución de Contenedores**: Cuando inicias un contenedor a partir de una imagen, Docker utiliza un sistema de archivos unión para superponer todas las capas de la imagen de forma que aparezcan como un solo sistema de archivos coherente. Los cambios realizados en el contenedor en ejecución se almacenan en una nueva capa de escritura que es específica del contenedor, permitiendo que el contenedor modifique archivos y directorios como si estuviera en un sistema de archivos normal, sin alterar las capas subyacentes de la imagen.

### WSL2 y Docker.

Docker utiliza el Subsistema de Windows para Linux (WSL); docker necesita de WSL en Windows para dar una experiencia más nativa y coherente al trabajar con contenedores Docker. Añadir WSL fué fundamental para que Docker en Windows funcione de manera más parecida a como lo hace en Linux, mejorando tanto el rendimiento como la usabilidad para desarrolladores y administradores de sistemas.

Si accedes en el explorador de Windows a \\\WSL, verás esto:

![alt text](../docker-I/WSLExplorador.png)


que vía WSL2 expone el contenido de este vhdx: 
![alt text](../docker-I/DockerVhdx.png)




### Sistema de archivos.








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




https://learn.microsoft.com/es-es/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&pivots=cs1-bash
https://learn.microsoft.com/es-es/sql/linux/quickstart-sql-server-containers-azure?view=sql-server-ver16&tabs=kubectl


