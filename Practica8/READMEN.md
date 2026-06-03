# Práctica 8: Automatización de despliegue backend con Docker y Docker Compose

---

## 1. Título

Automatización del despliegue de una aplicación backend utilizando Docker, Docker Compose, PostgreSQL y pgAdmin

---

## 2. Tiempo de duración

50 minutos

---

## 3. Fundamentos

Docker es una herramienta que permite ejecutar aplicaciones dentro de contenedores. Un contenedor puede entenderse como un entorno separado donde una aplicación funciona con todo lo que necesita para ejecutarse, como dependencias, configuraciones, librerías y variables necesarias. Esto es importante porque muchas veces una aplicación funciona correctamente en una computadora, pero en otra puede fallar por versiones diferentes del sistema, del lenguaje de programación o de las herramientas instaladas. Con Docker se busca evitar ese problema, ya que la aplicación se empaqueta dentro de una imagen y luego se ejecuta como contenedor.

En esta práctica se trabajó con una aplicación backend desarrollada con Spring Boot y Kotlin. El objetivo principal fue automatizar su despliegue en un entorno local utilizando Docker y Docker Compose. Para esto no solo se levantó el backend, sino también una base de datos PostgreSQL y un panel de administración llamado pgAdmin. Esto se parece más a un entorno real, porque una aplicación backend normalmente no trabaja sola, sino conectada a otros servicios como bases de datos, sistemas de autenticación o herramientas de administración.

Docker Compose permite definir varios servicios dentro de un solo archivo llamado `docker-compose.yml`. En lugar de levantar cada contenedor manualmente, se puede definir toda la estructura de la aplicación en ese archivo. En esta práctica se configuraron tres servicios principales: PostgreSQL, pgAdmin y la aplicación backend. PostgreSQL se usó como base de datos, pgAdmin como herramienta visual para administrar esa base de datos, y el backend como la aplicación principal que se conecta a PostgreSQL.

También se utilizaron volúmenes y redes. Los volúmenes son importantes porque permiten guardar la información de forma persistente. Esto significa que, aunque un contenedor se apague o se elimine, los datos pueden mantenerse guardados. En el caso de PostgreSQL, esto es muy importante porque ahí se almacena la información de la base de datos. Por otro lado, las redes permiten que los contenedores se comuniquen entre sí. Por ejemplo, pgAdmin pudo conectarse a PostgreSQL usando el nombre del servicio `postgres`, ya que ambos contenedores estaban dentro de la misma red de Docker Compose.

Otro aspecto importante fue el uso del archivo `.env`. Este archivo permite guardar variables de entorno como el nombre de la base de datos, el usuario, la contraseña y los datos de conexión. Esto ayuda a separar las credenciales del código fuente y hace que la configuración sea más ordenada. En proyectos reales, los archivos `.env` que contienen información sensible no deben subirse a repositorios públicos, ya que podrían exponer contraseñas o claves privadas.

Finalmente, se investigó e implementó una configuración multi-stage en Docker. Esta técnica permite dividir la construcción de la imagen en varias etapas. En la primera etapa se compila la aplicación y se genera el archivo `.jar`; en la segunda etapa se copia únicamente ese archivo final para ejecutarlo. Esto permite que la imagen final sea más limpia y ligera, porque no contiene todas las herramientas usadas para compilar. En conclusión, esta práctica permitió comprender cómo Docker y Docker Compose ayudan a automatizar el despliegue de una aplicación backend con varios servicios conectados dentro de un entorno local.

---

## 4. Conocimientos previos

- Uso básico de la terminal en Linux.
- Comandos básicos de Git.
- Conceptos básicos de Docker.
- Conceptos básicos de Docker Compose.
- Uso de archivos `.env`.
- Manejo básico de PostgreSQL.
- Uso básico de pgAdmin.
- Conceptos básicos de backend con Spring Boot.
- Manejo de puertos locales.
- Uso del navegador para verificar servicios web.

---

## 5. Objetivos a alcanzar

- Clonar el repositorio base de la aplicación backend.
- Verificar que Docker y Docker Compose estén instalados correctamente.
- Crear un archivo `.env` con las variables de entorno necesarias.
- Crear un servicio de PostgreSQL utilizando Docker Compose.
- Crear un servicio de pgAdmin para administrar la base de datos.
- Configurar volúmenes para garantizar la persistencia de datos.
- Configurar una red para permitir la comunicación entre contenedores.
- Crear una imagen Docker personalizada para el backend.
- Construir y ejecutar el contenedor de la aplicación backend.
- Verificar que pgAdmin pueda conectarse correctamente a PostgreSQL.
- Comprobar que el backend se ejecute correctamente en el puerto configurado.
- Investigar e implementar una configuración multi-stage.
- Documentar el proceso mediante capturas de pantalla.

---

## 6. Equipo necesario

- Computador con Arch Linux.
- Docker instalado y activo.
- Docker Compose instalado.
- Git instalado.
- Terminal zsh o bash.
- Navegador web.
- Conexión a internet.
- Repositorio base de la práctica.
- Editor de texto como nano o Visual Studio Code.

---

## 7. Material de apoyo

- Repositorio base: `https://github.com/maguaman2/tendencias-mar22-security.git`
- Documentación oficial de Docker.
- Documentación oficial de Docker Compose.
- Documentación oficial de PostgreSQL.
- Documentación oficial de pgAdmin.
- Documentación sobre multi-stage builds en Docker.

---

## 8. Procedimiento

### Paso 1: Verificar la instalación de Docker y Docker Compose

Primero se verificó que Docker y Docker Compose estuvieran instalados correctamente en Arch Linux. Para esto se usaron los comandos `docker --version`, `docker compose version` y `docker ps`. Con estos comandos se confirmó la versión de Docker, la versión de Docker Compose y que no existían contenedores activos al inicio de la práctica.

```bash
docker --version
docker compose version
docker ps
```

<img width="899" height="432" alt="image" src="https://github.com/user-attachments/assets/104a0b72-aee3-43e1-8e66-36f41873c043" />

<p align="center">
  <em>Figura 1. Verificación de Docker, Docker Compose y estado inicial de contenedores.</em>
</p>

---

### Paso 2: Clonar el repositorio base de la práctica

Después se creó una carpeta llamada `practicas` y se clonó el repositorio base desde GitHub. Luego se ingresó a la carpeta del proyecto y se listaron los archivos para confirmar que el repositorio se descargó correctamente. En la estructura del proyecto se observó el archivo `pom.xml`, la carpeta `src`, el archivo `mvnw` y otros elementos propios de una aplicación backend con Maven.

```bash
mkdir -p ~/practicas
cd ~/practicas
git clone https://github.com/maguaman2/tendencias-mar22-security.git
cd tendencias-mar22-security
ls -la
```

<img width="1013" height="808" alt="image" src="https://github.com/user-attachments/assets/7c79f826-022c-4e4a-8872-be367f72239c" />

<p align="center">
  <em>Figura 2. Clonación del repositorio tendencias-mar22-security y verificación de archivos.</em>
</p>

---

### Paso 3: Crear el archivo de variables de entorno `.env`

Luego se creó el archivo `.env`, donde se definieron las variables necesarias para PostgreSQL, pgAdmin y la conexión del backend con la base de datos. Este archivo permite separar las credenciales y configuraciones del código fuente. En esta práctica se usaron datos locales para la base de datos, el usuario administrador y la conexión interna entre contenedores.

```bash
nano .env
ls -la
cat .env
```

El contenido usado fue el siguiente:

```env
POSTGRES_DB=securitydb
POSTGRES_USER=admin
POSTGRES_PASSWORD=admin123

PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=admin123

DB_SERVER=postgres
DB_PORT=5432
DB_NAME=securitydb
DB_USER=admin
DB_PASSWORD=admin123
```

<img width="885" height="906" alt="image" src="https://github.com/user-attachments/assets/e50e6a93-ebcc-4a88-a3bc-edb01f64352a" />

<p align="center">
  <em>Figura 3. Creación y verificación del archivo .env con las variables de entorno.</em>
</p>

---

### Paso 4: Crear el Dockerfile de la aplicación backend

Después se otorgaron permisos de ejecución al archivo `mvnw`, ya que el proyecto utiliza Maven Wrapper para construir la aplicación. Luego se creó el archivo `Dockerfile`, encargado de construir la imagen del backend. En este archivo se utilizó una imagen base de Java 21, se copió el proyecto al contenedor, se ejecutó el proceso de compilación y finalmente se indicó el comando para ejecutar el archivo `.jar`.

```bash
chmod +x mvnw
nano Dockerfile
cat Dockerfile
```

El contenido usado fue el siguiente:

```dockerfile
FROM eclipse-temurin:21-jdk

WORKDIR /app

COPY . .

RUN chmod +x mvnw
RUN ./mvnw clean package -DskipTests

EXPOSE 8081

CMD ["java", "-jar", "target/security-0.0.1-SNAPSHOT.jar"]
```

<img width="861" height="719" alt="image" src="https://github.com/user-attachments/assets/a074fe98-7130-4c55-80d5-db942c83fede" />

<p align="center">
  <em>Figura 4. Creación del Dockerfile para construir la imagen de la aplicación backend.</em>
</p>

---

### Paso 5: Crear el archivo `docker-compose.yml`

Luego se creó el archivo `docker-compose.yml`, donde se definieron los servicios de PostgreSQL, pgAdmin y backend. También se configuraron los volúmenes para persistencia de datos y una red para la comunicación entre contenedores. En este archivo se usaron las variables definidas previamente en el archivo `.env`.

```bash
nano docker-compose.yml
cat docker-compose.yml
```

El contenido principal usado fue el siguiente:

```yaml
services:
  postgres:
    image: postgres:16
    container_name: postgres_security
    restart: always
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - security_network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 5

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin_security
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD}
    ports:
      - "5050:80"
    volumes:
      - pgadmin_data:/var/lib/pgadmin
    networks:
      - security_network
    depends_on:
      - postgres

  backend:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: backend_security
    restart: always
    environment:
      DB_SERVER: ${DB_SERVER}
      DB_PORT: ${DB_PORT}
      DB_NAME: ${DB_NAME}
      DB_USER: ${DB_USER}
      DB_PASSWORD: ${DB_PASSWORD}
    ports:
      - "8081:8081"
    networks:
      - security_network
    depends_on:
      postgres:
        condition: service_healthy

volumes:
  postgres_data:
  pgadmin_data:

networks:
  security_network:
    driver: bridge
```

<img width="945" height="966" alt="image" src="https://github.com/user-attachments/assets/7dba3b04-e731-48a5-9122-897d00ce3f6a" />

<p align="center">
  <em>Figura 5. Configuración del archivo docker-compose.yml con PostgreSQL, pgAdmin y backend.</em>
</p>

---

### Paso 6: Levantar los servicios con Docker Compose

Una vez configurados los archivos necesarios, se ejecutó el comando `docker compose up -d --build`. Este comando construyó la imagen del backend y levantó los servicios definidos en el archivo `docker-compose.yml`. Durante el proceso se descargaron las imágenes necesarias, se creó la red, se crearon los volúmenes y se iniciaron los contenedores.

```bash
docker compose up -d --build
docker ps
```

<img width="1600" height="643" alt="image" src="https://github.com/user-attachments/assets/cb3f57bc-64b9-444b-b81f-5c61b18a1865" />

<p align="center">
  <em>Figura 6. Construcción y levantamiento de los servicios con Docker Compose.</em>
</p>

---

### Paso 7: Verificar los contenedores activos

Después se usó el comando `docker ps` para comprobar que los contenedores de PostgreSQL, pgAdmin y backend estuvieran ejecutándose correctamente. En la salida se observó que PostgreSQL estaba en estado saludable, pgAdmin estaba activo y el backend estaba publicado en el puerto `8081`.

```bash
docker ps
```

<img width="1600" height="643" alt="image" src="https://github.com/user-attachments/assets/6017e9e0-02ed-4e34-abbd-feff3cab8fbe" />

<p align="center">
  <em>Figura 7. Verificación de los contenedores activos de PostgreSQL, pgAdmin y backend.</em>
</p>

---

### Paso 8: Revisar los logs del backend

Luego se revisaron los logs del contenedor del backend para confirmar que la aplicación Spring Boot se inició correctamente. En los logs se observó el inicio de Spring Boot, la inicialización de Tomcat en el puerto `8081` y la conexión con PostgreSQL mediante HikariPool. Esto confirmó que el backend logró comunicarse con la base de datos.

```bash
docker logs backend_security
```

<img width="1600" height="729" alt="image" src="https://github.com/user-attachments/assets/a608acb2-45cd-44d8-8074-419b4f0a3f13" />

<p align="center">
  <em>Figura 8. Logs del backend mostrando el inicio correcto de Spring Boot y conexión con la base de datos.</em>
</p>

---

### Paso 9: Verificar volúmenes y redes creadas

También se comprobaron los volúmenes y la red creados por Docker Compose. Los volúmenes permiten mantener los datos de PostgreSQL y pgAdmin, mientras que la red permite la comunicación entre los servicios. En este paso se verificó que existieran los volúmenes `postgres_data`, `pgadmin_data` y la red `security_network`.

```bash
docker volume ls
docker network ls
```

<img width="1025" height="513" alt="image" src="https://github.com/user-attachments/assets/f4807962-807c-4988-8e34-d7746af598bc" />

<p align="center">
  <em>Figura 9. Verificación de volúmenes y red creados para la práctica.</em>
</p>

---

### Paso 10: Acceder a pgAdmin desde el navegador

Después se abrió pgAdmin desde el navegador usando el puerto `5050`. Se ingresó con el correo y contraseña definidos en el archivo `.env`. Luego se registró un nuevo servidor dentro de pgAdmin usando los datos de conexión de PostgreSQL.

```text
http://localhost:5050
```

Credenciales usadas:

```text
Correo: admin@admin.com
Contraseña: admin123
```

Datos configurados para el servidor:

```text
Nombre: PostgreSQL Docker
Host: postgres
Puerto: 5432
Base de datos: securitydb
Usuario: admin
Contraseña: admin123
```

<img width="1600" height="864" alt="image" src="https://github.com/user-attachments/assets/c6d2d6cf-5ce7-49e7-9c67-b498d7e8b378" />

<p align="center">
  <em>Figura 10. Configuración de conexión en pgAdmin usando el host postgres.</em>
</p>

---

### Paso 11: Verificar la conexión de pgAdmin con PostgreSQL

Luego de guardar la configuración, pgAdmin logró conectarse correctamente al servidor PostgreSQL. En la parte izquierda se observó el servidor registrado como `PostgreSQL Docker`, confirmando que pgAdmin pudo comunicarse con PostgreSQL dentro de la red creada por Docker Compose.

<img width="1600" height="858" alt="image" src="https://github.com/user-attachments/assets/8e7d548a-d255-4285-83d4-64d7b45214d4" />

<p align="center">
  <em>Figura 11. pgAdmin conectado correctamente al servicio PostgreSQL.</em>
</p>

---

### Paso 12: Verificar la base de datos desde la terminal

También se ingresó directamente al contenedor de PostgreSQL usando `psql`. Desde ahí se ejecutó el comando `\dt`, el cual permitió listar las tablas existentes en la base de datos `securitydb`. En el resultado se observaron tablas como `flyway_schema_history` y `users`, lo que demuestra que la aplicación realizó la migración o creación de tablas correctamente.

```bash
docker exec -it postgres_security psql -U admin -d securitydb
```

Dentro de PostgreSQL se ejecutó:

```sql
\dt
```

<img width="878" height="411" alt="image" src="https://github.com/user-attachments/assets/621e9d31-fb96-4b36-9610-3779933e6948" />

<p align="center">
  <em>Figura 12. Verificación de la base de datos securitydb desde la terminal con psql.</em>
</p>

---

### Paso 13: Probar el backend desde el navegador

Después se abrió el backend desde el navegador usando el puerto `8081`. La aplicación mostró una página de error `Whitelabel Error Page` con estado `404`. Esto no significa que el contenedor esté mal, sino que la ruta raíz `/` no tiene un endpoint definido. Lo importante es que el servidor respondió y la aplicación backend estaba ejecutándose correctamente.

```text
http://localhost:8081
```

<img width="1059" height="565" alt="image" src="https://github.com/user-attachments/assets/5407f009-1e0e-4f02-9058-b38cf58c27e5" />

<p align="center">
  <em>Figura 13. Prueba del backend en el navegador mediante localhost:8081.</em>
</p>

---

### Paso 14: Crear un Dockerfile con multi-stage build

Como parte de la investigación, se creó un archivo llamado `Dockerfile.multistage`. Este archivo divide el proceso en dos etapas: una etapa de construcción y una etapa de ejecución. En la primera se compila la aplicación y en la segunda se copia solamente el archivo `.jar` final. Esto permite que la imagen final no tenga todos los archivos y herramientas usadas para compilar.

```bash
nano Dockerfile.multistage
cat Dockerfile.multistage
```

El contenido usado fue el siguiente:

```dockerfile
FROM eclipse-temurin:21-jdk AS build

WORKDIR /app

COPY . .

RUN chmod +x mvnw
RUN ./mvnw clean package -DskipTests

FROM eclipse-temurin:21-jre

WORKDIR /app

COPY --from=build /app/target/security-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8081

CMD ["java", "-jar", "app.jar"]
```

<img width="979" height="797" alt="image" src="https://github.com/user-attachments/assets/ba0db288-accd-4951-b38f-d6a70d128891" />

<p align="center">
  <em>Figura 14. Dockerfile.multistage creado para optimizar la construcción de la imagen.</em>
</p>

---

### Paso 15: Reconstruir la aplicación usando multi-stage

Finalmente, se modificó el servicio del backend en `docker-compose.yml` para usar el archivo `Dockerfile.multistage`. Luego se volvió a levantar el entorno usando Docker Compose y se verificó nuevamente que los contenedores estuvieran activos. También se revisaron los logs del backend para comprobar que la aplicación inició correctamente después del cambio.

```bash
docker compose down
docker compose up -d --build
docker ps
docker logs backend_security
```

<img width="1600" height="616" alt="image" src="https://github.com/user-attachments/assets/d1def9fe-548d-4a44-a04b-32b6e575f1d9" />

<p align="center">
  <em>Figura 15. Backend levantado nuevamente después de aplicar la configuración multi-stage.</em>
</p>

---

## 9. Resultados esperados

Al finalizar la práctica se logró automatizar el despliegue de una aplicación backend usando Docker y Docker Compose. Primero se verificó que Docker y Docker Compose estuvieran instalados correctamente en Arch Linux. Después se clonó el repositorio base desde GitHub y se revisó la estructura del proyecto.

También se creó un archivo `.env` para manejar las variables de entorno necesarias, como el nombre de la base de datos, usuario, contraseña y datos de conexión. Luego se creó un archivo `docker-compose.yml` con tres servicios principales: PostgreSQL, pgAdmin y backend. Además, se configuraron dos volúmenes para mantener la información de PostgreSQL y pgAdmin, y una red para permitir la comunicación entre los contenedores.

Se construyó la imagen de la aplicación backend mediante un Dockerfile y se levantó el contenedor usando Docker Compose. Al ejecutar `docker ps`, se comprobó que los tres contenedores estaban activos. También se revisaron los logs del backend, donde se observó que Spring Boot inició correctamente y se conectó a PostgreSQL.

Además, se verificó el funcionamiento de pgAdmin desde el navegador usando `localhost:5050`. Dentro de pgAdmin se registró el servidor PostgreSQL usando el host `postgres`, lo cual confirmó que los servicios podían comunicarse dentro de la red de Docker Compose. También se comprobó la base de datos desde la terminal usando `psql`, donde se visualizaron las tablas creadas.

Finalmente, se implementó un Dockerfile con multi-stage build para optimizar la construcción de la imagen. Esta configuración permitió separar la etapa de compilación de la etapa de ejecución, dejando en la imagen final solamente el archivo necesario para ejecutar la aplicación.

---

## 10. Conclusiones

- Se logró clonar correctamente el repositorio base de la aplicación backend.
- Se verificó que Docker y Docker Compose estaban instalados y funcionando en Arch Linux.
- Se creó correctamente el archivo `.env` con las variables de entorno necesarias.
- Se configuró PostgreSQL como servicio de base de datos dentro de Docker Compose.
- Se configuró pgAdmin como panel de administración para PostgreSQL.
- Se crearon volúmenes para mantener la persistencia de datos.
- Se creó una red para conectar los servicios de la aplicación.
- Se construyó la imagen personalizada del backend usando un Dockerfile.
- Se levantó el backend junto con PostgreSQL y pgAdmin usando Docker Compose.
- Se verificó que pgAdmin puede conectarse a PostgreSQL usando el host `postgres`.
- Se comprobó desde la terminal que la base de datos `securitydb` fue creada correctamente.
- Se verificó que el backend responde desde el navegador en el puerto `8081`.
- Se investigó e implementó un Dockerfile con multi-stage build.
- La práctica permitió comprender mejor cómo automatizar el despliegue de una aplicación backend con varios servicios conectados.

---

## 11. Recomendaciones

- Verificar que Docker esté activo antes de ejecutar Docker Compose.
- Revisar que el archivo `.env` esté correctamente escrito y sin espacios innecesarios.
- No subir archivos `.env` con credenciales sensibles a repositorios públicos.
- Usar nombres claros para los contenedores, volúmenes y redes.
- Verificar los logs del backend si la aplicación no inicia correctamente.
- Usar `docker ps` para confirmar que los contenedores estén activos.
- Usar `docker volume ls` para comprobar que los volúmenes fueron creados.
- Usar `docker network ls` para verificar la red creada por Docker Compose.
- En pgAdmin, usar `postgres` como host y no `localhost`, porque el servicio está dentro de la red de Docker.
- Implementar multi-stage builds para reducir el tamaño de la imagen final.
- Revisar el puerto configurado antes de probar la aplicación en el navegador.
- Mantener organizadas las capturas de pantalla dentro de una carpeta llamada `imagenes`.

---

## 12. Resumen del audio

Para la entrega se adjunta un archivo de audio en formato `.mp3` con una duración mayor a 60 segundos. En el audio se explica con palabras propias lo aprendido durante la práctica.

Guion usado para el audio:

En esta semana aprendí a automatizar el despliegue de una aplicación backend utilizando Docker y Docker Compose. Primero cloné el proyecto base desde GitHub y configuré un archivo .env para manejar las variables de entorno de forma separada. Luego creé servicios para PostgreSQL y pgAdmin dentro de un archivo docker-compose.yml, usando volúmenes para mantener los datos y una red para que los contenedores puedan comunicarse entre sí. También construí una imagen para el backend usando un Dockerfile y levanté el contenedor junto con la base de datos. Después verifiqué la conexión desde pgAdmin y también desde la terminal usando psql. Finalmente investigué sobre multi-stage builds, que permiten separar la etapa de compilación de la etapa de ejecución, logrando una imagen más limpia y optimizada. Esta práctica me ayudó a entender mejor cómo se despliega una aplicación real con varios servicios conectados.

Archivo de audio:

```text
audio_semana8.mp3
```

---

## 13. Bibliografía

Docker Inc. (s.f.). *Descripción general de Docker Compose*. Docker Documentation. https://docs.docker.com/compose/

Docker Inc. (s.f.). *Referencia de Dockerfile*. Docker Documentation. https://docs.docker.com/reference/dockerfile/

Docker Inc. (s.f.). *Multi-stage builds*. Docker Documentation. https://docs.docker.com/build/building/multi-stage/

Docker Inc. (s.f.). *Volúmenes*. Docker Documentation. https://docs.docker.com/engine/storage/volumes/

Docker Inc. (s.f.). *Redes en Docker*. Docker Documentation. https://docs.docker.com/engine/network/

PostgreSQL Global Development Group. (s.f.). *PostgreSQL: Documentation*. PostgreSQL. https://www.postgresql.org/docs/

pgAdmin Development Team. (s.f.). *pgAdmin 4 Documentation*. pgAdmin. https://www.pgadmin.org/docs/

Eclipse Foundation. (s.f.). *Eclipse Temurin containers*. Adoptium. https://adoptium.net/es/temurin/
