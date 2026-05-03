# Práctica: MySQL y phpMyAdmin con Docker

---

## 1. Título

Implementación de contenedores Docker para MySQL y phpMyAdmin con red personalizada

---

## 2. Tiempo de duración

50 minutos

---

## 3. Fundamentos

Docker es una plataforma de código abierto que permite crear, desplegar y ejecutar aplicaciones dentro de contenedores. Un contenedor es un entorno aislado y ligero que incluye todo lo necesario para que una aplicación funcione: código, librerías, configuraciones y dependencias. A diferencia de las máquinas virtuales tradicionales, los contenedores no requieren un sistema operativo completo para cada instancia, sino que comparten el kernel del sistema operativo anfitrión. Esto los hace significativamente más rápidos en su arranque y más eficientes en el consumo de recursos como memoria y almacenamiento.

Una de las grandes ventajas de Docker es la portabilidad. Un contenedor creado en un equipo con Linux puede ejecutarse sin modificaciones en otro equipo con el mismo sistema, garantizando que el comportamiento de la aplicación sea idéntico en cualquier entorno. Esto elimina el clásico problema de "en mi máquina funciona", muy frecuente en equipos de desarrollo de software.

En el ámbito del desarrollo, es muy común necesitar un sistema gestor de bases de datos (SGBD) disponible de forma rápida y sin instalar software directamente en el sistema operativo principal. Docker permite levantar contenedores con bases de datos como MySQL en cuestión de segundos, facilitando la creación de entornos de desarrollo limpios, reproducibles y fácilmente eliminables una vez que ya no se necesitan.

MySQL es uno de los sistemas de gestión de bases de datos relacionales más utilizados en el mundo. Fue desarrollado originalmente por MySQL AB y actualmente es mantenido por Oracle Corporation. Es ampliamente adoptado en aplicaciones web, sistemas empresariales y proyectos de todo tamaño gracias a su rendimiento, estabilidad y facilidad de uso. MySQL organiza la información en tablas relacionadas entre sí mediante claves primarias y foráneas, siguiendo el modelo relacional basado en el lenguaje SQL (Structured Query Language).

phpMyAdmin es una herramienta de administración web para MySQL y MariaDB, escrita en PHP. Permite gestionar bases de datos, tablas, registros, usuarios y permisos desde una interfaz gráfica intuitiva accesible desde el navegador, sin necesidad de usar la línea de comandos directamente. Es ampliamente utilizada en entornos educativos y de desarrollo por su facilidad de uso.

Para que dos contenedores Docker puedan comunicarse entre sí, es necesario conectarlos a una red compartida. Docker ofrece distintos tipos de redes, siendo la red de tipo bridge la más utilizada para comunicación entre contenedores en un mismo host. Al crear una red personalizada y conectar los contenedores a ella, Docker permite que se referencien entre sí usando el nombre del contenedor como hostname, lo que simplifica la configuración de la conexión entre servicios.

En esta práctica se implementaron dos contenedores: uno con MySQL como motor de base de datos y otro con phpMyAdmin como interfaz de administración, conectados mediante una red personalizada de Docker llamada `redinterna`, demostrando cómo orquestar múltiples servicios de forma sencilla y organizada.

<p align="center">
  <img width="492" height="385" alt="image" src="https://github.com/user-attachments/assets/d8f7c3ea-f6c2-4d02-b365-c0a7e0e7e627" />
  <br/>
  <em>Figura 1. Arquitectura de contenedores Docker con red personalizada</em>
</p>

---

## 4. Conocimientos previos

- Comandos básicos de Linux
- Uso de Docker
- Manejo de terminal
- Conceptos básicos de bases de datos relacionales
- Conocimiento básico de redes

---

## 5. Objetivos a alcanzar

- Comprender el funcionamiento de contenedores Docker
- Implementar MySQL dentro de un contenedor Docker
- Implementar phpMyAdmin dentro de un contenedor Docker
- Crear y gestionar una red personalizada en Docker
- Conectar ambos contenedores a la red creada
- Administrar una base de datos MySQL desde la interfaz de phpMyAdmin

---

## 6. Equipo necesario

- Computador en mi caso con Linux (Arch Linux)
- Docker instalado y en ejecución
- Terminal (zsh o bash)
- Navegador web (Firefox, Chromium, etc.)

---

## 7. Material de apoyo

- Documentación oficial de Docker: https://docs.docker.com
- Documentación oficial de MySQL: https://dev.mysql.com/doc/
- Documentación oficial de phpMyAdmin: https://www.phpmyadmin.net/docs/
- Guía de la asignatura

---

## 8. Procedimiento

### Paso 1 — Verificar que Docker está limpio

Se verificó que Docker no tenía imágenes ni contenedores previos, asegurando un entorno limpio para la práctica.

```bash
docker image ls
```
---

### Paso 2 — Crear el contenedor de MySQL

Se creó el contenedor de MySQL definiendo las credenciales necesarias: contraseña de root, nombre de base de datos, usuario y contraseña. Se mapeó el puerto `3307` del host al `3306` del contenedor.

```bash
docker run -d --name dbmysql -e MYSQL_ROOT_PASSWORD=root123 -e MYSQL_DATABASE=testdb -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin123 -p 3307:3306 mysql:latest
```

<img width="1464" height="513" alt="image" src="https://github.com/user-attachments/assets/fa1af8b6-ee44-49b7-b6ee-df5745992e08" />
<em>Figura 3. Contenedor MySQL creado exitosamente</em></p>

---

### Paso 3 — Crear el contenedor de phpMyAdmin

Se creó el contenedor de phpMyAdmin configurando la variable `PMA_ARBITRARY=1` que permite ingresar manualmente el servidor al que conectarse. Se expuso en el puerto `8090`.

```bash
docker run -d --name phpmyadmin -e PMA_ARBITRARY=1 -p 8090:80 phpmyadmin:latest
```

<img width="940" height="674" alt="image" src="https://github.com/user-attachments/assets/40182520-8786-4e6a-902e-f3a7175150e8" />
<em>Figura 4. Contenedor phpMyAdmin creado exitosamente</em></p>

---

### Paso 4 — Verificar que ambos contenedores están corriendo

Se verificó con `docker ps` que los dos contenedores estaban activos con estado `Up`.

```bash
docker ps
```

<img width="1490" height="258" alt="image" src="https://github.com/user-attachments/assets/ab80b7c3-d8a0-4ed5-aeba-593ecf1b5e45" />
<em>Figura 5. Ambos contenedores corriendo correctamente</em></p>

---

### Paso 5 — Crear la red personalizada

Se creó una red interna y personalizada llamada `redinterna` de tipo bridge que permite la comunicación entre los contenedores.

```bash
docker network create --attachable redinterna
```

<img width="706" height="231" alt="image" src="https://github.com/user-attachments/assets/686155cc-a200-44d6-9721-23df16bf1823" />
<em>Figura 6. Red personalizada "redinterna" creada</em></p>

---

### Paso 6 — Conectar los contenedores a la red

Se conectaron ambos contenedores a la red `redinterna` para que puedan comunicarse entre sí.

```bash
docker network connect redinterna dbmysql
```

```bash
docker network connect redinterna phpmyadmin
```
<img width="540" height="280" alt="image" src="https://github.com/user-attachments/assets/8862176f-bbc9-4321-8a61-e1f063419d49" />
<em>Figura 7. Contenedores conectados a la red personalizada</em></p>

---

### Paso 7 — Verificar la red

Se inspeccionó la red para confirmar que ambos contenedores estaban correctamente conectados.

```bash
docker network inspect redinterna
```

<img width="1010" height="930" alt="image" src="https://github.com/user-attachments/assets/d399af54-8704-44c9-96d0-ebe590fd4114" />
<em>Figura 8. Inspección de red mostrando ambos contenedores conectados</em></p>

---

### Paso 8 — Ingresar a phpMyAdmin desde el navegador

Se accedió a la interfaz web de phpMyAdmin desde el navegador en `http://localhost:8090`. Se utilizó `dbmysql` como nombre del servidor (nombre del contenedor dentro de la red), `root` como usuario y `root123` como contraseña.

<img width="1600" height="898" alt="image" src="https://github.com/user-attachments/assets/61d42b22-b4f9-4788-95cb-5c4bad96d82d" />
<em>Figura 9. Interfaz de login de phpMyAdmin</em></p>

---

### Paso 9 — Crear la base de datos de prueba

Desde el panel de phpMyAdmin se creó la base de datos `testSemana4` usando la opción "Nueva" del panel izquierdo.

<img width="1600" height="855" alt="image" src="https://github.com/user-attachments/assets/77e6d8fe-7f8a-42ec-9fc2-9df8c6487257" />
<em>Figura 10. Base de datos "testSemana4" creada exitosamente</em></p>

---

### Paso 10 — Crear tabla e insertar datos con SQL

Desde la pestaña **SQL** de phpMyAdmin se creó la tabla `estudiantes` con sus respectivos campos.

```sql
CREATE TABLE estudiantes (id INT AUTO_INCREMENT PRIMARY KEY, nombre VARCHAR(100), carrera VARCHAR(100), semestre INT);
```

Luego se insertaron registros de prueba:

```sql
INSERT INTO estudiantes (nombre, carrera, semestre) VALUES ('Edisson Padilla', 'Desarrollo de Software', 6), ('Juan Buri', 'Desarrollo de Software', 6), ('David Mogrovejo', 'Desarrollo de Software', 6);
```

<img width="1600" height="903" alt="image" src="https://github.com/user-attachments/assets/6c1e6810-853d-4f03-8f2a-4b5575a31e89" />
<em>Figura 11. Datos insertados correctamente en la tabla estudiantes</em></p>

---

### Paso 11 — Consultar los datos insertados

Se ejecutó una consulta SELECT para verificar que los datos se almacenaron correctamente.

```sql
SELECT * FROM estudiantes;
```
<img width="1600" height="903" alt="image" src="https://github.com/user-attachments/assets/f2732be9-0e50-470b-a81f-30df8f3bbbd9" />
<em>Figura 12. Consulta SELECT mostrando los datos insertados</em></p>

---

### Paso 12 — Limpieza del entorno

Al finalizar la práctica se eliminaron todos los contenedores, imágenes, redes y volúmenes generados.

```bash
docker system prune -a --volumes -f
```

Se verificó que el entorno quedó completamente limpio:

```bash
docker ps
```

```bash
docker image ls
```

---

## 9. Resultados esperados

Se logró implementar exitosamente dos contenedores Docker comunicados entre sí mediante una red personalizada. El contenedor de MySQL funcionó como motor de base de datos y el contenedor de phpMyAdmin como interfaz gráfica de administración. Desde phpMyAdmin fue posible crear la base de datos `testSemana4`, definir la tabla `estudiantes` e insertar registros mediante sentencias SQL, comprobando el correcto funcionamiento de la conexión entre ambos contenedores a través de la red `redinterna`.

---

## 10. Bibliografía

- Docker Inc. (2024). *Documentación oficial de Docker*. Recuperado de https://docs.docker.com/get-started/

- Oracle Corporation. (2024). *Manual de referencia de MySQL 8.0*. Recuperado de https://dev.mysql.com/doc/refman/8.0/es/

- The phpMyAdmin Project. (2024). *Documentación de phpMyAdmin*. Recuperado de https://www.phpmyadmin.net/docs/

- Red Hat. (2023). *¿Qué es Docker?* Recuperado de https://www.redhat.com/es/topics/containers/what-is-docker

- OpenWebinars. (2023). *Redes en Docker: cómo funcionan y cómo configurarlas*. Recuperado de https://openwebinars.net/blog/redes-en-docker/
