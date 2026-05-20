# Práctica 6: WordPress con Docker Compose YML

---

## 1. Título

Implementación de WordPress, MySQL y phpMyAdmin usando Docker Compose en formato YML

---

## 2. Tiempo de duración

50 minutos

---

## 3. Fundamentos

Docker Compose es una herramienta que permite definir y ejecutar aplicaciones formadas por varios contenedores mediante un archivo de configuración en formato YAML. Esto facilita la administración de proyectos porque evita tener que crear cada contenedor de forma manual con comandos separados. En lugar de ejecutar varios comandos para levantar WordPress, MySQL y phpMyAdmin, se puede escribir toda la configuración dentro de un solo archivo llamado `docker-compose.yml`.

En esta práctica se trabajó con tres servicios principales: WordPress, MySQL y phpMyAdmin. WordPress funciona como un sistema de gestión de contenidos que permite crear y administrar sitios web. MySQL se utiliza como base de datos para almacenar la información del sitio, como usuarios, publicaciones, configuraciones y otros datos internos. Por otro lado, phpMyAdmin permite administrar la base de datos desde el navegador, usando una interfaz gráfica más sencilla que trabajar únicamente desde la terminal.

El uso de Docker Compose es importante porque permite organizar todos los elementos de una aplicación en un mismo archivo. En el archivo YML se pueden definir las imágenes que usará cada servicio, los nombres de los contenedores, los puertos que se van a exponer, las variables de entorno, las redes y los volúmenes. Gracias a esto, el proyecto queda más ordenado y puede ejecutarse con un solo comando: `docker compose up -d`.

También se definió una red personalizada para permitir la comunicación entre los contenedores. Esta red permite que WordPress se conecte con MySQL usando el nombre del servicio, sin necesidad de configurar una dirección IP manual. Esto es útil porque Docker se encarga de resolver la comunicación interna entre los servicios.

Además, se creó un volumen para MySQL. Los volúmenes son importantes porque permiten conservar la información de la base de datos aunque el contenedor sea detenido o eliminado. Sin un volumen, los datos podrían perderse si se elimina el contenedor. En cambio, al usar un volumen, la información se guarda de forma persistente dentro del sistema Docker.

En esta práctica se utilizó Arch Linux como sistema operativo anfitrión. Se verificó que Docker estuviera instalado y activo, se creó la carpeta del proyecto, se construyó el archivo `docker-compose.yml`, se levantaron los servicios y se comprobó su funcionamiento desde el navegador. Finalmente, se accedió a WordPress mediante el puerto `8080` y a phpMyAdmin mediante el puerto `8081`, verificando que el entorno multicontenedor funcionara correctamente.

De acuerdo con la documentación oficial de Docker, Docker Compose permite definir aplicaciones multicontenedor mediante un archivo YAML, donde se organizan servicios, redes y volúmenes. Esto se relaciona directamente con la práctica realizada, porque WordPress necesita conectarse con una base de datos MySQL y, además, phpMyAdmin permite administrar esa base de datos desde el navegador. Por otra parte, los volúmenes permiten conservar información fuera del ciclo de vida del contenedor, lo cual es importante en este caso porque los datos de MySQL deben mantenerse aunque el contenedor sea detenido o recreado. Por esta razón, Docker Compose no solo facilita la ejecución del entorno, sino que también permite trabajar de una forma más ordenada, repetible y cercana a un proyecto real.
---

## 4. Conocimientos previos

- Uso básico de la terminal en Linux.
- Comandos básicos de Docker.
- Conceptos básicos de contenedores.
- Conocimiento general del formato YAML.
- Manejo básico de redes y puertos.
- Conceptos básicos de bases de datos.
- Uso de navegador web para acceder a servicios locales.

---

## 5. Objetivos a alcanzar

- Construir un archivo `docker-compose.yml` usando el formato YAML.
- Crear un entorno multicontenedor con WordPress, MySQL y phpMyAdmin.
- Definir una red personalizada para la comunicación entre servicios.
- Definir un volumen para la persistencia de datos de MySQL.
- Levantar los servicios usando Docker Compose.
- Verificar el funcionamiento de WordPress desde el navegador.
- Verificar el acceso a la base de datos mediante phpMyAdmin.
- Comprobar los contenedores, redes, volúmenes y logs del proyecto.

---

## 6. Equipo necesario

- Computador con Arch Linux.
- Docker instalado.
- Docker Compose instalado.
- Terminal zsh o bash.
- Navegador web.
- Conexión a internet para descargar las imágenes Docker.

---

## 7. Material de apoyo

- Documentación oficial de Docker.
- Documentación oficial de Docker Compose.
- Imagen oficial de WordPress en Docker Hub.
- Imagen oficial de MySQL en Docker Hub.
- Imagen oficial de phpMyAdmin en Docker Hub.
- Guía de la asignatura.

---

## 8. Procedimiento

### Paso 1: Verificar Docker y Docker Compose

Primero se verificó que Docker estuviera instalado y que el servicio se encontrara activo en el sistema Arch Linux. También se revisó la versión de Docker Compose para comprobar que se podía trabajar con el comando `docker compose`.

```bash
docker --version
docker compose version
sudo systemctl status docker
```

<img width="1192" height="1684" alt="practica6_pagina_1" src="https://github.com/user-attachments/assets/eca5d44c-fc26-4209-a72d-8e5bcca8184a" />

<p align="center">
  <em>Figura 1. Verificación de Docker, ubicación del proyecto y edición inicial del archivo docker-compose.yml.</em>
</p>

---

### Paso 2: Crear la carpeta del proyecto

Se creó una carpeta llamada `wordpress-docker`, donde se almacenó el archivo principal de configuración del proyecto. Luego se verificó la ubicación actual con el comando `pwd`.

```bash
mkdir wordpress-docker
cd wordpress-docker
pwd
```

En esta parte se confirmó que el proyecto se encontraba dentro de la ruta:

```bash
/home/danipadi/wordpress-docker
```

---

### Paso 3: Crear el archivo docker-compose.yml

Después de ubicarse dentro de la carpeta del proyecto, se creó el archivo `docker-compose.yml` usando el editor de texto `nano`.

```bash
nano docker-compose.yml
```

Dentro de este archivo se definieron los tres servicios solicitados: `wordpress`, `mysql` y `phpmyadmin`.

---

### Paso 4: Configurar los servicios en formato YML

El archivo `docker-compose.yml` quedó configurado de la siguiente manera:

```yaml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql_wordpress
    restart: always
    environment:
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wordpress_user
      MYSQL_PASSWORD: wordpress_pass
      MYSQL_ROOT_PASSWORD: root_pass
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - wordpress_network

  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_NAME: wordpress_db
      WORDPRESS_DB_USER: wordpress_user
      WORDPRESS_DB_PASSWORD: wordpress_pass
    depends_on:
      - mysql
    networks:
      - wordpress_network

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin_app
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    depends_on:
      - mysql
    networks:
      - wordpress_network

networks:
  wordpress_network:
    driver: bridge

volumes:
  mysql_data:
```

<img width="1192" height="1684" alt="practica6_pagina_2" src="https://github.com/user-attachments/assets/7744294f-1197-4535-a61b-0c79a568df03" />

<p align="center">
  <em>Figura 2. Configuración del archivo docker-compose.yml, ejecución de Docker Compose y verificación de contenedores.</em>
</p>

---

### Paso 5: Levantar los servicios con Docker Compose

Una vez guardado el archivo YML, se ejecutó el siguiente comando para descargar las imágenes necesarias y levantar los contenedores en segundo plano:

```bash
docker compose up -d
```

Este comando permitió crear y ejecutar los servicios de WordPress, MySQL y phpMyAdmin.

---

### Paso 6: Verificar los contenedores activos

Luego se comprobó que los contenedores estuvieran funcionando correctamente con el comando:

```bash
docker ps
```

En la salida del comando se observaron los tres contenedores activos:

- `wordpress_app`
- `phpmyadmin_app`
- `mysql_wordpress`

Esto confirmó que los servicios se levantaron correctamente y que los puertos quedaron asignados de la siguiente forma:

- WordPress: `localhost:8080`
- phpMyAdmin: `localhost:8081`
- MySQL: puerto interno `3306`

---

### Paso 7: Verificar la red creada

Se verificó la red personalizada creada por Docker Compose usando el siguiente comando:

```bash
docker network ls
```

Después se inspeccionó la red del proyecto con:

```bash
docker network inspect wordpress-docker_wordpress_network
```

<img width="1192" height="1684" alt="practica6_pagina_3" src="https://github.com/user-attachments/assets/81d935bf-9969-495a-93bc-48c25029c4d3" />

<p align="center">
  <em>Figura 3. Verificación e inspección de la red personalizada creada por Docker Compose.</em>
</p>

En la inspección de la red se pudo observar que los contenedores estaban conectados a la misma red. Esto permite que WordPress se comunique con MySQL usando el nombre del servicio `mysql`.

---

### Paso 8: Verificar el volumen creado

También se verificó el volumen creado para MySQL. Para ello se ejecutó:

```bash
docker volume ls
```

Luego se inspeccionó el volumen con:

```bash
docker volume inspect wordpress-docker_mysql_data
```

<img width="1192" height="1684" alt="practica6_pagina_4" src="https://github.com/user-attachments/assets/b8f05ed6-8965-40dd-8b38-15813378853a" />

<p align="center">
  <em>Figura 4. Verificación del volumen mysql_data y acceso a la instalación inicial de WordPress.</em>
</p>

El volumen `mysql_data` permite conservar la información de la base de datos MySQL aunque el contenedor sea eliminado o reiniciado.

---

### Paso 9: Acceder a WordPress desde el navegador

Después de confirmar que los contenedores estaban activos, se ingresó a WordPress desde el navegador usando la siguiente dirección:

```text
http://localhost:8080
```

En esta pantalla se completó la instalación inicial de WordPress, ingresando los datos del sitio, usuario administrador, contraseña y correo electrónico.

---

### Paso 10: Finalizar la instalación de WordPress

Luego de completar el formulario de instalación, WordPress mostró el mensaje de instalación correcta. Esto confirmó que WordPress pudo conectarse con la base de datos MySQL definida en el archivo `docker-compose.yml`.

<img width="1192" height="1684" alt="practica6_pagina_5" src="https://github.com/user-attachments/assets/985b9272-6fd1-4045-9722-6bedd6028e4e" />

<p align="center">
  <em>Figura 5. Instalación completada de WordPress y acceso al panel de administración.</em>
</p>

---

### Paso 11: Acceder al panel de administración de WordPress

Después de finalizar la instalación, se accedió al escritorio principal de WordPress. En esta pantalla se pudo observar el panel administrativo, desde donde se pueden crear páginas, entradas, usuarios y configurar el sitio web.

Esto comprobó que el servicio de WordPress estaba funcionando correctamente en el puerto `8080`.

---

### Paso 12: Acceder a phpMyAdmin

Luego se accedió a phpMyAdmin desde el navegador usando la dirección:

```text
http://localhost:8081
```

Para iniciar sesión se utilizaron las credenciales definidas en el archivo `docker-compose.yml`.

Usuario:

```text
root
```

Contraseña:

```text
root_pass
```

<img width="1192" height="1684" alt="practica6_pagina_6" src="https://github.com/user-attachments/assets/c540e164-1aaa-4c46-9e79-710ee43bcc98" />

<p align="center">
  <em>Figura 6. Inicio de sesión en phpMyAdmin y visualización de la base de datos de WordPress.</em>
</p>

---

### Paso 13: Verificar la base de datos en phpMyAdmin

Una vez dentro de phpMyAdmin, se verificó la existencia de la base de datos `wordpress_db`. También se pudo observar que WordPress generó sus tablas internas dentro de la base de datos.

Esto demuestra que WordPress, MySQL y phpMyAdmin se comunicaron correctamente dentro de la red definida por Docker Compose.

---

### Paso 14: Revisar los logs de los contenedores

Finalmente, se revisaron los logs de los contenedores para comprobar que los servicios estaban respondiendo correctamente.

```bash
docker compose logs
```

<img width="1192" height="1684" alt="practica6_pagina_7" src="https://github.com/user-attachments/assets/d06782cb-503c-495c-8c90-c69facfd92f8" />

<p align="center">
  <em>Figura 7. Revisión de logs finales de los servicios ejecutados con Docker Compose.</em>
</p>

En los logs se observaron solicitudes realizadas desde el navegador hacia WordPress y phpMyAdmin, lo que confirma que los servicios estaban activos y respondiendo correctamente.

---

## 9. Resultados esperados

Al finalizar la práctica se logró implementar correctamente un entorno multicontenedor utilizando Docker Compose. Los servicios configurados fueron WordPress, MySQL y phpMyAdmin.

WordPress quedó disponible desde el navegador mediante el puerto `8080`, mientras que phpMyAdmin quedó disponible mediante el puerto `8081`. Además, MySQL funcionó como servicio interno de base de datos para almacenar la información del sitio web.

También se comprobó la creación de una red personalizada llamada `wordpress_network`, la cual permitió la comunicación entre los contenedores. Gracias a esta red, WordPress pudo conectarse a MySQL utilizando el nombre del servicio definido en el archivo YML.

De igual manera, se verificó la creación del volumen `mysql_data`, utilizado para conservar los datos de MySQL. Esto permite que la información no se pierda al detener o recrear los contenedores.

Finalmente, mediante phpMyAdmin se pudo visualizar la base de datos relacionada con WordPress, y con los logs se comprobó que los servicios respondían correctamente a las solicitudes del navegador.

---

## 10. Conclusiones

- Docker Compose permitió administrar varios contenedores desde un solo archivo `docker-compose.yml`.
- El formato YAML facilitó la organización de servicios, puertos, variables de entorno, redes y volúmenes.
- WordPress se ejecutó correctamente en el puerto `8080`.
- phpMyAdmin se ejecutó correctamente en el puerto `8081`.
- MySQL funcionó como base de datos para almacenar la información de WordPress.
- La red personalizada permitió la comunicación interna entre los contenedores.
- El volumen `mysql_data` permitió mantener la información de MySQL de forma persistente.
- La práctica permitió comprender la importancia de Docker Compose en proyectos que requieren varios servicios conectados entre sí.

---

## 11. Recomendaciones

- Verificar que Docker esté activo antes de ejecutar `docker compose up -d`.
- Revisar bien la indentación del archivo `docker-compose.yml`, porque YAML depende de los espacios.
- Usar nombres claros para servicios, redes y volúmenes.
- No eliminar los volúmenes si se desea conservar la información de la base de datos.
- Revisar los logs con `docker compose logs` si algún servicio no funciona correctamente.
- Usar puertos diferentes si `8080` o `8081` ya están ocupados por otros servicios.
- Mantener Docker y el sistema actualizados para evitar errores de compatibilidad con redes o módulos del kernel.

---

## 12. Bibliografía

Docker Inc. (s.f.). *Docker Compose overview*. Docker Documentation. https://docs.docker.com/compose/

Docker Inc. (s.f.). *Compose file reference*. Docker Documentation. https://docs.docker.com/reference/compose-file/

Docker Inc. (s.f.). *Volumes*. Docker Documentation. https://docs.docker.com/engine/storage/volumes/

Docker Hub. (s.f.). *WordPress official image*. https://hub.docker.com/_/wordpress

Docker Hub. (s.f.). *MySQL official image*. https://hub.docker.com/_/mysql

Docker Hub. (s.f.). *phpMyAdmin official image*. https://hub.docker.com/_/phpmyadmin

phpMyAdmin contributors. (s.f.). *phpMyAdmin: Bringing MySQL to the web*. https://www.phpmyadmin.net/

WordPress.org. (s.f.). *Acerca de WordPress*. https://es.wordpress.org/about/
