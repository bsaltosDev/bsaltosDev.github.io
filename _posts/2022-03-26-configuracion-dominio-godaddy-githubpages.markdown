---
layout: post
title:  "Configuración Dominio GoDaddy - Github Pages"
date:   2022-03-26 19:57 -0500
categories: CONFIG
---
En esta primera entrada del blog me gustaría hablar un poco de lo que me tocó batallar con la configuración del Dominio en Godaddy para que pueda ser tomado correctamente desde Github Pages para la habilitación de este mismo sitio web que ven ahora. Esto debido a que las distintas configuraciones que busqué y configuré no fueron las adecuadas.

### Requisitos previos
+ Cuenta de Github
+ Dominio de Godaddy

### Configuración Dominio
Para configurar adecuadamente las reglas del dominio para habilitar la redirección del mismo hacia GithubPages, vamos a realizar las siguientes configuraciones desde nuestro dominio / Domain Settings / DNS Management.

#### Configurar registros de IPs de GitHub
Debemos registrar las ips respectivas de Github, las mismas nos son especificadas en la documentación respectiva. Añadimos un nuevo DNS record para cada una de las ips solicitadas.

<img class="img-post" src="/assets/post/dodaddydomain/AddRuleGodaddy.png" alt="Agregar DNS record"/>

Para facilidad les dejo una tabla con las configuraciones solicitadas.

| Type | Name |       Value     |   TTL  |
|:----:|:----:|:---------------:|:------:|
|  A   |   @  | 185.199.108.153 | 1 Hour |
|  A   |   @  | 185.199.109.153 | 1 Hour |
|  A   |   @  | 185.199.110.153 | 1 Hour |
|  A   |   @  | 185.199.111.153 | 1 Hour |

#### Configurar registro para el sitio de dominio de GitHub Pages
De manera similar debemos configurar el dominio que se habilita mediante GitHub Pages, para que se redireccione desde el dominio comprado en DoDaddy. Para lo cual configuraremos el siguiente DNS record:

|  Type  | Name |         Value       |   TTL  |
|:------:|:----:|:-------------------:|:------:|
|  CNAME |  www | reponame.github.io  | 1 Hour |

#### ¿Habilitar redirección?
Los configuraciones anteriores, están documentadas en muchos sitios, páginas o foros en internet. Pero lo que me dejo con dudas y no encontré mucha información, y fue lo que me ocasionó problemas en la configuración fue saber si habilito o no la sección de Forwarding. Por lo mismo aquí les dejo el dato **NO LA HABILITEN**. Quedan avisados.

<img class="img-post" src="/assets/post/dodaddydomain/ForwardingDodaddy.png" alt="Configuración sección Forwarding"/>

### Configuración Github
Para nuestro repositorio de GitHub existe amplia documentación, pero con todo les dejo las configuraciones básicas.

- Crear el repositorio con el nombre: [reponame].github.io
- En las configuraciones del repositorio, específicamente en la sección **Pages**. Ingresamos el dominio de Godaddy en la parte de **Custom domain**. Si se configuró adecuadamente el DNS en Godaddy se habilita la configuración de **Enforce HTTPS**, previo análisis de unos minutos en la página de configuración de GitHub.

<img class="img-post" src="/assets/post/dodaddydomain/githubpagesconfig.png" alt="Configuración Dominio personalizado GitHub"/>

### Referencias
- [Documentación oficial de GitHub Pages](https://docs.github.com/es/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [Administración de dominios personalizados en GitHub Pages](https://docs.github.com/es/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)