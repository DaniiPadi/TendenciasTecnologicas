# Práctica: Creación de imagen Docker para aplicación React

---

## 1. Título

Generación de una imagen Docker a partir de una aplicación React usando Dockerfile

---

## 2. Tiempo de duración

50 minutos

---

## 3. Fundamentos

Docker es una herramienta que permite ejecutar aplicaciones dentro de contenedores. Un contenedor puede entenderse como un espacio separado donde una aplicación funciona con todo lo que necesita para ejecutarse. Esto ayuda bastante porque muchas veces una aplicación funciona en una computadora, pero en otra puede fallar por versiones diferentes de Node.js, npm, dependencias o configuraciones del sistema. Con Docker se busca evitar ese problema, ya que la aplicación se prepara dentro de una imagen y luego se ejecuta como contenedor.

En esta práctica se trabajó con una aplicación frontend desarrollada en React. Primero se probó el proyecto de manera local para confirmar que funcionaba correctamente antes de crear la imagen Docker. También se utilizó un backend simulado llamado `mockAPI`, el cual funciona con JSON Server y entrega datos como estudiantes y docentes. Este backend fue necesario porque el frontend consume esa información para mostrar los usuarios en la pantalla.

Para crear la imagen personalizada se usó un archivo llamado `Dockerfile`. Este archivo contiene las instrucciones que Docker debe seguir para preparar la aplicación. En este caso, primero se usó una imagen de Node.js para instalar las dependencias del proyecto y construir la aplicación. Después se usó una imagen de Nginx para servir los archivos finales generados por React. Esto es importante porque una aplicación React, cuando se prepara para producción, genera archivos estáticos que pueden ser mostrados desde un servidor web.

También se creó un archivo `.dockerignore`, que sirve para evitar copiar archivos innecesarios dentro de la imagen, como `node_modules`, `dist` o archivos internos de Git. Esto ayuda a que la imagen sea más limpia y que el proceso de construcción no incluya carpetas que pueden ocupar espacio o causar problemas.

El comando principal usado para construir la imagen fue `docker build`. Este comando lee el Dockerfile y genera una imagen con el nombre que se le indique. Luego, con `docker run`, se creó un contenedor usando esa imagen y se publicó el puerto `8080` para poder abrir la aplicación desde el navegador. Finalmente, se comprobó que la aplicación funcionara desde `localhost:8080` y que, al encender el backend simulado, los datos se mostraran correctamente.

Esta práctica permitió entender mejor cómo Docker ayuda a ejecutar aplicaciones de forma más ordenada y reutilizable. También se pudo ver la diferencia entre probar una aplicación localmente con `npm run dev` y ejecutarla desde un contenedor Docker. En general, Dockerfile facilita que el proyecto tenga una configuración clara, que se pueda compartir y que funcione de forma parecida en distintos equipos.

---

## 4. Conocimientos previos

- Uso básico de la terminal en Linux.
- Comandos básicos de Git.
- Uso básico de Node.js y npm.
- Conceptos básicos de React.
- Conceptos básicos de Docker.
- Manejo de puertos locales.
- Uso del navegador para probar aplicaciones.

---

## 5. Objetivos a alcanzar

- Clonar el repositorio del frontend React.
- Clonar y ejecutar el backend simulado necesario para la aplicación.
- Instalar las dependencias del frontend y del backend.
- Ejecutar el proyecto React de forma local.
- Crear un archivo Dockerfile para contenerizar la aplicación.
- Crear un archivo `.dockerignore`.
- Construir una imagen Docker personalizada.
- Crear y ejecutar un contenedor a partir de la imagen.
- Verificar el funcionamiento de la aplicación desde el navegador.
- Documentar el proceso mediante capturas de pantalla.

---

## 6. Equipo necesario

- Computador con Arch Linux.
- Docker instalado y activo.
- Git instalado.
- Node.js instalado.
- npm instalado.
- Terminal zsh o bash.
- Navegador web.
- Conexión a internet.

---

## 7. Material de apoyo

- Repositorio frontend: `https://github.com/Daviddotcoms/suda-frontend-s6`
- Repositorio backend simulado: `https://github.com/Daviddotcoms/mockAPI`
- Documentación oficial de Docker.
- Documentación de Dockerfile.
- Documentación de Vite.
- Imagen oficial de Nginx en Docker Hub.

---

## 8. Procedimiento

### Paso 1: Crear la carpeta de trabajo

Primero se creó una carpeta llamada `Tarea-Docker-React`, donde se guardaron los repositorios del frontend y del backend simulado. Luego se verificó la ubicación actual usando el comando `pwd`.

```bash
mkdir -p ~/Tarea-Docker-React
cd ~/Tarea-Docker-React
pwd
```

<img width="649" height="304" alt="image" src="https://github.com/user-attachments/assets/16b2cdbe-ca62-4db0-bf8c-94b98874f8d3" />

<p align="center">
  <em>Figura 1. Creación de la carpeta de trabajo y verificación de la ruta del proyecto.</em>
</p>

---

### Paso 2: Clonar los repositorios necesarios

Después se clonaron los dos repositorios solicitados para la práctica. El primero corresponde al backend simulado y el segundo corresponde al frontend desarrollado en React.

```bash
git clone https://github.com/Daviddotcoms/mockAPI.git
git clone https://github.com/Daviddotcoms/suda-frontend-s6.git
ls
```

<img width="917" height="635" alt="image" src="https://github.com/user-attachments/assets/fa96dc71-7a06-4ff0-9db8-73b5a97aba43" />

<p align="center">
  <em>Figura 2. Clonación de los repositorios mockAPI y suda-frontend-s6.</em>
</p>

---

### Paso 3: Ejecutar el backend simulado

Luego se ingresó a la carpeta del backend simulado, se instalaron las dependencias y se inició el servicio. Este backend se levantó en el puerto `3100`, mostrando los endpoints disponibles para `classmates` y `teachers`.

```bash
cd ~/Tarea-Docker-React/mockAPI
npm install
npm start
```

<img width="648" height="807" alt="image" src="https://github.com/user-attachments/assets/fa0fd64c-06f2-4b69-b98c-6d5174068989" />

<p align="center">
  <em>Figura 3. Backend simulado ejecutándose correctamente en el puerto 3100.</em>
</p>

---

### Paso 4: Ejecutar el frontend de forma local

En otra terminal se ingresó al proyecto frontend, se instalaron las dependencias y se ejecutó la aplicación en modo desarrollo usando Vite.

```bash
cd ~/Tarea-Docker-React/suda-frontend-s6
npm install
npm run dev
```

<img width="1423" height="800" alt="image" src="https://github.com/user-attachments/assets/7d1f434d-bc33-415b-b465-bac2003b577c" />


<p align="center">
  <em>Figura 4. Ejecución local del frontend React con Vite en el puerto 5173.</em>
</p>

---

### Paso 5: Verificar el frontend en el navegador

Después de ejecutar el frontend localmente, se abrió el navegador en la dirección indicada por la terminal.

```text
http://localhost:5173
```

En esta pantalla se verificó que la aplicación cargaba correctamente y mostraba los datos obtenidos desde el backend simulado.

<img width="977" height="973" alt="image" src="https://github.com/user-attachments/assets/0677cd58-9fbd-402e-8848-28d1af29d818" />


<p align="center">
  <em>Figura 5. Aplicación React funcionando localmente desde el navegador.</em>
</p>

---

### Paso 6: Crear el archivo Dockerfile

Una vez verificado el funcionamiento local, se creó un archivo llamado `Dockerfile` dentro de la carpeta del frontend. Este archivo contiene las instrucciones para construir la aplicación React y servirla mediante Nginx.

```bash
cd ~/Tarea-Docker-React/suda-frontend-s6
nano Dockerfile
cat Dockerfile
```

El contenido usado fue el siguiente:

```dockerfile
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

<img width="951" height="803" alt="image" src="https://github.com/user-attachments/assets/645cbd21-6f05-4867-b0c4-8bfcee3b638c" />

<p align="center">
  <em>Figura 6. Creación y verificación del archivo Dockerfile usado para construir la imagen.</em>
</p>

---

### Paso 7: Crear el archivo .dockerignore

También se creó el archivo `.dockerignore`. Este archivo permite excluir carpetas y archivos que no son necesarios dentro de la imagen Docker, como `node_modules`, `dist` o archivos internos de Git.

```bash
nano .dockerignore
cat .dockerignore
```

El contenido usado fue el siguiente:

```dockerignore
node_modules
dist
.git
.gitignore
README.md
npm-debug.log
```

<img width="947" height="358" alt="image" src="https://github.com/user-attachments/assets/b5c16529-0720-405a-b8b4-20ca4d1d994a" />

<p align="center">
  <em>Figura 7. Archivo .dockerignore creado para evitar copiar archivos innecesarios.</em>
</p>

---

### Paso 8: Construir la imagen Docker

Luego se construyó la imagen Docker con el comando `docker build`. Para esta práctica se asignó el nombre `suda-frontend` a la imagen.

```bash
docker build -t suda-frontend .
```

Durante el proceso, Docker descargó las imágenes base necesarias, instaló las dependencias, construyó el proyecto React y finalmente preparó los archivos para ser servidos por Nginx.

<img width="1205" height="1020" alt="image" src="https://github.com/user-attachments/assets/e34a4ebd-ac13-4859-ae64-1780ac8e6228" />

<p align="center">
  <em>Figura 8. Construcción correcta de la imagen Docker suda-frontend.</em>
</p>

---

### Paso 9: Verificar la imagen creada

Después de construir la imagen, se usó el comando `docker images` para comprobar que la imagen apareciera en la lista de imágenes locales.

```bash
docker images
```

En el resultado se observó la imagen `suda-frontend:latest`, lo que confirma que el proceso de construcción terminó correctamente.

<img width="1049" height="432" alt="image" src="https://github.com/user-attachments/assets/732191e3-b3e6-4f8d-b558-15233542a4cf" />

<p align="center">
  <em>Figura 9. Verificación de la imagen suda-frontend creada en Docker.</em>
</p>

---

### Paso 10: Crear y ejecutar el contenedor

Con la imagen ya creada, se ejecutó un contenedor usando el puerto `8080` del equipo local y el puerto `80` del contenedor.

```bash
docker run -d --name suda-frontend-container -p 8080:80 suda-frontend
docker ps
```

El comando `docker ps` permitió comprobar que el contenedor estaba activo y que el puerto quedó publicado correctamente.

<img width="1600" height="308" alt="image" src="https://github.com/user-attachments/assets/de80f65f-3f39-454b-83be-75a0f2b735c0" />

<p align="center">
  <em>Figura 10. Contenedor suda-frontend-container ejecutándose correctamente.</em>
</p>

---

### Paso 11: Verificar la aplicación desde Docker

Finalmente, se abrió la aplicación desde el navegador usando la siguiente dirección:

```text
http://localhost:8080
```

Al inicio la aplicación mostró el mensaje “No hay usuarios” porque el backend simulado se encontraba apagado. Después se volvió a iniciar el backend y se recargó la página. Con esto, la aplicación mostró correctamente la lista de usuarios, estudiantes y docentes.

<img width="1600" height="852" alt="image" src="https://github.com/user-attachments/assets/114fa961-3faf-4ec0-bd62-929a7a86f27e" />

<p align="center">
  <em>Figura 11. Aplicación React ejecutándose desde el contenedor Docker y conectada al backend simulado.</em>
</p>

---

## 9. Resultados esperados

Al finalizar la práctica se logró crear una imagen Docker personalizada para una aplicación React. Primero se comprobó que el proyecto funcionara de forma local en el puerto `5173`. Luego se creó un Dockerfile que permitió construir la aplicación y servirla mediante Nginx.

También se creó el archivo `.dockerignore`, lo cual ayudó a mantener la imagen más limpia, evitando copiar carpetas y archivos innecesarios. Posteriormente, se generó la imagen `suda-frontend:latest` usando el comando `docker build`.

Después se ejecutó un contenedor llamado `suda-frontend-container`, publicando el puerto `8080` del sistema anfitrión hacia el puerto `80` del contenedor. Finalmente, se accedió desde el navegador a `http://localhost:8080` y se comprobó que la aplicación cargaba correctamente.

Además, se volvió a ejecutar el backend simulado `mockAPI`, ya que era necesario para que el frontend pudiera mostrar los datos de estudiantes y docentes. Al recargar la página, la aplicación mostró los usuarios correctamente, confirmando que el frontend y el backend estaban funcionando juntos.

---

## 10. Conclusiones

- Se logró clonar correctamente el frontend React y el backend simulado desde GitHub.
- Se verificó el funcionamiento local del backend en el puerto `3100`.
- Se ejecutó el frontend localmente usando `npm run dev` en el puerto `5173`.
- Se creó un Dockerfile para construir la aplicación React y servirla con Nginx.
- Se creó un archivo `.dockerignore` para evitar copiar archivos innecesarios dentro de la imagen.
- Se construyó correctamente la imagen Docker `suda-frontend:latest`.
- Se creó y ejecutó un contenedor llamado `suda-frontend-container`.
- Se verificó la aplicación desde el navegador usando `localhost:8080`.
- Se comprobó que la aplicación necesita el backend simulado encendido para mostrar los usuarios.
- La práctica ayudó a comprender cómo Docker permite empaquetar una aplicación y ejecutarla de forma más ordenada y reutilizable.

---

## 11. Recomendaciones

- Verificar que Docker esté activo antes de construir la imagen.
- Comprobar que Node.js y npm estén instalados antes de ejecutar los proyectos.
- Mantener encendido el backend simulado mientras se prueba el frontend.
- Revisar que el Dockerfile esté dentro de la carpeta correcta del proyecto.
- Usar `.dockerignore` para evitar que la imagen copie archivos innecesarios.
- Revisar el resultado de `docker images` para confirmar que la imagen fue creada.
- Revisar el resultado de `docker ps` para confirmar que el contenedor está activo.
- Si el navegador muestra “No hay usuarios”, revisar que el backend esté encendido.
- Reiniciar Docker o el sistema si aparecen errores de red al construir la imagen.
- Usar nombres claros para imágenes y contenedores para evitar confusiones.


---

## 13. Bibliografía

Docker Inc. (s.f.). *Dockerfile reference*. Docker Documentation. https://docs.docker.com/reference/dockerfile/

Docker Inc. (s.f.). *Docker Build*. Docker Documentation. https://docs.docker.com/build/

Docker Inc. (s.f.). *Docker image build*. Docker Documentation. https://docs.docker.com/reference/cli/docker/image/build/

Docker Hub. (s.f.). *nginx official image*. https://hub.docker.com/_/nginx

Vite. (s.f.). *Compilación para producción*. Vite Documentation. https://es.vite.dev/guide/build

Vite. (s.f.). *Despliegue de un sitio estático*. Vite Documentation. https://es.vite.dev/guide/static-deploy
