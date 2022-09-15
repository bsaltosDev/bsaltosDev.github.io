---
layout: post
title:  "Cambiar directorio por defecto de docker-data (WSL Windows)"
date:   2022-03-26 18:57 -0500
categories: CONFIG, DOCKER, WINDOWS, WSL
---

Es esta entrada quisiera conversar un poco de cómo mover el directorio por defecto de la instalación de Docker sobre Windows con WSL.

### Requisitos
- Windows 10
- WSL 2.0
- Docker

### El problema
Quizá no a todos les haya sucedido, creo que depende mucho de la capacidad del disco que maneje su ambiente de desarrollo en Windows, pero por mi parte manejo dos discos en mi instalación de Windows 10, el disco C: y el disco D:, en el disco C: no dispongo de tanta capacidad de almacenamiento, por lo que el disco D: tiene la gran parte de data que uso en el día a día. Por otro lado Docker no permite configurar por defecto el Disco donde se instalará, al menos desde la interfaz gráfica de instalación, también vale mencionar que como pre requisito necesita de la instalación previa de WSL que se ejecuta con un comando sin establecer mayores configuraciones por lo que se va por defecto prácticamente toda la instalación de Docker.

Y en el día a día del trabajo con Docker, el mismo se empieza a llenar de imágenes, contenedores, y volúmenes que ocupan rápidamente la capacidad de la instalación por defecto de Docker y WSL. Tomé medidas paliativas como vaciar o purgar la caché de Docker pero con la desventaja de volver a descargar una y otra vez las imágenes ya disponibles en la misma.

Así que se hizo indispensable poder mover el directorio de almacenamiento de Docker de WSL.

### La estrategia
Revisando varios recursos se encontró dos estrategias para poder solventar el problema (ambas funcionan correctamente, una la tengo configurada en el equipo de trabajo y la otra en un equipo personal):

1. Exportar el volumen de almacenamiento de WSL y moverlo al disco requerido.
2. Crear un enlace simbólico a la nueva ruta del directorio de WSL previamente migrado o copiado (Ctrl+c y Ctrl-v).

#### 1. Exportar volumen de WSL

#### 2. Enlace simbólico

