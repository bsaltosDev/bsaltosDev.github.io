---
layout: post
title:  "Cambiar directorio por defecto de docker-data (WSL Windows)"
date:   2022-10-27 22:45 -0500
categories: CONFIG DOCKER WINDOWS WSL
---

Es esta entrada quisiera conversar un poco de cómo mover el directorio por defecto de la instalación de Docker sobre Windows con WSL.

### Requisitos
- Windows 10
- WSL 2.0
- Docker

### El problema
Quizá no a todos les haya sucedido, creo que depende mucho de la capacidad del disco que maneje su ambiente de desarrollo en Windows, pero por mi parte manejo dos discos en mi instalación de Windows 10, el disco C: y el disco D:, en el disco C: no dispongo de tanta capacidad de almacenamiento, por lo que el disco D: tiene la gran parte de data que uso en el día a día. Por otro lado Docker no permite configurar por defecto el Disco donde se instalará, al menos desde la interfaz gráfica de instalación, también vale mencionar que como pre requisito necesitamos de la instalación previa de WSL que se integrará con Docker.

En el día a día del trabajo con Docker, se empieza a ocupar rápidamente la capacidad del espacio del disco de la instalación por defecto de Docker y WSL con imágenes, contenedores, y volúmenes. Como medidas paliativas tomé la opción de vaciar o purgar la caché y el espacio de trabajo de Docker pero con la desventaja de volver a descargar una y otra vez las imágenes previamente usadas.
 
Así que se hizo indispensable poder mover el directorio de almacenamiento de Docker de WSL a otro disco.

### La estrategia
Revisando varios recursos se encontró dos estrategias para poder solventar el problema (ambas funcionan correctamente, una la tengo configurada en el equipo de trabajo y la otra en un equipo personal):

1. Exportar el volumen de almacenamiento de WSL y moverlo al disco requerido.
2. Crear un enlace simbólico a la nueva ruta del directorio de WSL previamente migrado o copiado (Ctrl+c y Ctrl-v).

#### 1. Exportar volumen de WSL
Para esta opción requerimos de las herramientas propias de wsl instaladas en el equipo junto con este programa, para ello usaremos la exportación e importación del disco virtual de trabajo con docker.

Podemos verificar la ubicación de la data de docker en la ruta:
```
C:\Users\[Usuario]\AppData\Local\Docker\wsl\
```
Para proceder con la migración de esta estrategia seguiremos los siguientes pasos:
1. Detener el servicio de wsl, para ello podemos detener Docker-Desktop o usar el comando:
```
wsl --shutdown
```
2. Para verificar el status de wsl usamos el comando
```
wsl -l -v
```
3. En el mismo podemos observar el subsistema de nombre docker-desktop-data que es el subsistema que se migrará al nuevo directorio.
<img class="img-post" src="/assets/post/dockerwsl/wslStop.png" alt="WSL command status"/>
1. Para la migración usaremos el comando export para la generación respectiva del respaldo. Preferiblemente usar ya el directorio que se usará como nuevo espacio de almacenamiento de Docker.
```
wsl --export docker-desktop-data "[Path_Respaldo]\docker-desktop-data.tar"
```
5. Una vez realizado el respaldo procedemos a eliminar el registro del subsistema, para poder generar el nuevo registro desde la nueva ruta.
```
wsl --unregister docker-desktop-data
```
6. Procedemos a hacer el import de docker-desktop-data pero ubicado en la nueva ruta del disco con mayor capacidad. Preferible usar el mismo estándar de WSL ([Nuevo_Path]\wsl\data).
```
wsl --import docker-desktop-data "[Nuevo_Path]\wsl\data" "[Path_Respaldo]\docker-desktop-data.tar" --version 2
```
7. Iniciamos Docker-Desktop para verificar el funcionamiento.

#### 2. Enlace simbólico
Para esta estrategia, se podría decir que usaremos un hack sobre Windows usando un enlace simbólico fuerte para la nueva ubicación del directorio de la data de docker.

Procederemos con los siguientes pasos:
1. Crearemos un directorio con el mismo nombre que almacena la data de docker. 
```
[Nuevo_PAth]\wsl\
```
2. Detenemos el servicio de wsl, para ello podemos usar el comando:
```
wsl --shutdown
```
3. Copiamos la data del directorio antiguo al nuevo directorio. La data del Docker esta almacenada en el path.
```
C:\Users\[Usuario]\AppData\Local\Docker\wsl\
```
4. Borramos el contenido del directorio antiguo o migrado.
5. Con una consola con permisos de administrador procedemos a crear el enlace simbólico fuerte del directorio antiguo hacia el nuevo directorio.
```
cd C:\Users\[Usuario]\AppData\Local\Docker\
mklink /D /J wsl [Nuevo_PAth]\wsl\
```

### Recomendaciones
Es esta sección apunto algunas recomendaciones a considerar para la migración del directorio de la data de wsl de Docker.

1. Preferiblemente realizar este proceso antes de empezar a crear contenedores, ya que cuando exista alguna data el proceso de copia, exportación e importación dependerá del tamaño de los contenedores, imágenes y volúmenes que se estén manejando.
2. Si ya se tiene data previa, y es posible eliminarla para agilizar el proceso de exportación, importación o copia de directorios. Se puede usar alguno comandos de docker para limpiar la data innecesaria. Estos comandos son del tipo docker [option] prune. Como ejemplo dejo el siguiente comando que elimina los contenedores, imágenes y volúmenes sin usar. Leer la [Documentación Oficial](https://docs.docker.com/engine/reference/commandline/system_prune/) para mayor referencia.
```
docker system prune
```   