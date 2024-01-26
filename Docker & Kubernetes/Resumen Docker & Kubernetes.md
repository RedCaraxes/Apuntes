---
tags:
  - Docker
  - DockerHub
  - SK8
  - Kubernetes
aliases:
  - Comandos basicos de Docker y Kubernetes
---

# Docker CLI - Running and Stopping
| Commands                              | Description                                                                                             |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| docker pull [ImageName]               | pull an image froma registry                                                                            |
| docker run [ImageName]                | Run containers                                                                                          |
| docker run -d [ImageName:tag][comand] | detached mode, command is not mandatory, is we dont have to put tag by default the tag y latest version |
| docker start [ContainerName]          | Start stopped containers                                                                                |
| docker ps                             | List running containers                                                                                 |
| docker ps -a                          | List running containers and stopped containers                                                          |
| docker stop [ContainerName]           | Stop containers                                                                                         |
| docker kill [ContainerName]           | Kill containers                                                                                         |
| Docker image inspect [ImageName]      | Get image info                                                                                          |
| Docker exec [name][comand]            | execute a command on the container                                                                      |
| Docker log [name]                     | Use for debug container                                                                                 |
| docker log -f [name]                  | Debug in realtime                                                                                       |
|                                       |                                                                                                         |
Ejemplo: `docker run -d --name webapp nginx:1.14-alpine`
Para detener todos los container usar `docker stop $(docker ps -aq)`
![[Pasted image 20240101195346.png|500x300]]

# Docker CLI - Attach shell
| Commands                                                   | Description                   |
| ---------------------------------------------------------- | ----------------------------- |
| docker pull -it nginx -- /bin/bash                         | Attach shell                  |
| docker run -it -- microsoft/powershell:nanoserver pwsh.exe | Attach powershell             |
| docker container exec -it [ContainerName]                  | Attach to a running container |
>[!info] Recuerda
>el flag -i: modo interactivo, el flag -t: terminal.
>![[Pasted image 20240103112556.png|600x500]]
# Docker CLI - Cleaning up
| Commands                    | Description                                     |
| --------------------------- | ----------------------------------------------- |
|                             |                                                 |
| docker rm [containerName]   | Removes stopped containers                      |
| docker rm $(docker ps -aq) | Removes all stopped containers                  |
| docker images               | Lists images                                    |
| docker rmi [imageName]      | Deletes the image                               |
| docker system prune -a      | Removes all images not in use by any containers |
>[!warning] Recuerda
>rm funciona solo si el container esta detenido, el campo de nombre del contenedor puede sustituirse por el ID del mismo, recordar que el nombre es el nombre que se le asigna con el flag --name o sino el aplicativo le data uno al azar.
>Eliminar todas las imágenes de docker `docker rmi $(docker images -aq)`
>Desplegar un container en background, con mapeo de puertos local de 8080 y el container de 38282, la instancia especifica es `kodekloud/simple-webapp` usar el tag `blue`-> `docker run -p 38282:8080 -d kodekloud/simple-webapp:blue`
>![[Pasted image 20240103123821.png|800x500]]

# Docker CLI - Building
| Commands                                 | Description                                                      |
| ---------------------------------------- | ---------------------------------------------------------------- |
| docker build -t [name:tag] .             | Builds an image using a Dockerfile located in the same folder    |
| docker build -t [name:tag] -f [fileName] | Builds an image using a Dockerfile located in a different folder |
| docker tag [imageName] [name:tag]        | Tag an existing image                                            |
>[!Info] Recuerda
>el campo "tag" es generalmente para especificar la versión (se puede omitir), el campo nombre es a decisión puede ser el que queramos
![[Pasted image 20240101204512.png|500x300]]
![[Pasted image 20240103210651.png|600x400]]
# Docker CLI - Volumes
| Commands                           | Description                     |
| ---------------------------------- | ------------------------------- |
| docker create volume [volumeName]  | Creates a new volume            |
| docker volume ls                   | Lists the volumes               |
| docker volume inspect [volumeName] | Display the volume info         |
| docker volume rm [volumeName]      | Deletes a volume                |
| docker volume prune                | Deletes all volumes not mounted |

>[!Info] Ruta por defecto
>Los volumes creados se encuentran en /var/lib/docker/volumes
![[Pasted image 20240102122509.png|700x400]]
![[Pasted image 20240102122613.png|700x400]]
![[Pasted image 20240103124947.png|800x500]]
![[Pasted image 20240106193608.png|800x500]]

# Enviroment variable
![[Pasted image 20240103203939.png|800x500]]
![[Pasted image 20240103203953.png|800x500]]
# Dockerfile
![[Pasted image 20240103192703.png|800x500]]
![[Pasted image 20240103201744.png|800x500]]
![[Pasted image 20240103202919.png|800x500]]
![[Pasted image 20240103203132.png|800x500]]

# Docker network
![[Pasted image 20240103211059.png|800x500]]
![[Pasted image 20240103211233.png|800x500]]
>[!Info] Ejemplo de creación de network
>`docker network create --driver bridge --subnet 182.18.0.1/24 --gateway 182.18.0.1 wp-mysql-network`
>Ejemplo de link
>`docker run -d --name=clickcounter --link redis:redis -p 8085:5000 kodekloud/click-counter`
![[Pasted image 20240103211248.png|800x500]]
>[!Info] Ejemplo de creación de network
>web es accesible desde afuera en el puerto 8080, DB puede ver web por el puerto 80, web puede ver db pero nadie fuera puede hacerlo
>![[Pasted image 20240108175318.png]]
![[Pasted image 20240108175534.png]]
# Resource Limits
![[Pasted image 20240108174841.png]]

# YAML Format
![[Pasted image 20240108112034.png]]
>[!Info] Link verificador de formato
>https://www.yamllint.com/

# Docker compose
![[Pasted image 20240102130631.png|800x500]]
![[Pasted image 20240106210040.png|800x500]]
![[Pasted image 20240106210806.png|800x500]]


| Commands                                               | Description                   |
| ------------------------------------------------------ | ----------------------------- |
| docker compose build                                   | build the images              |
| docker compose start                                   | Start the containers          |
| docker compose stop                                    | Stop the containers           |
| docker compose up -d                                   | Build and start               |
| docker compose ps                                      | List what's running           |
| docker compose rm                                      | Remove from memory            |
| docker compose down                                    | Stop and remove               |
| docker compose logs                                    | Get the logs                  |
| docker compose exec [container] bash                   | Run a command in a container  |
| docker compose --project-name test1 up -d              | Run an instance as a project  |
| docker compose -p test2 up -d                          | Shortcut for the last command |
| docker compose ls                                      | List running projects         |
| docker compose cp [containerID]:[SRC_PATH] [DEST_PATH] | Copy files from the container |
| docker compose cp [SRC_PATH] [containerID]:[DEST_PATH] | Copy files to the container   |

## Dependencia
![[Pasted image 20240108175706.png|800x500]]
## Volumes
![[Pasted image 20240108175819.png|800x500]]

# Docker registry
![[Pasted image 20240108183940.png|800x500]]













