# Práctica 9: Contenerización de Aplicación Frontend con React y Comunicación con API Backend

---

## 1. Título

Contenerización de una aplicación frontend desarrollada con React para visualizar datos desde una API REST alojada en un contenedor backend independiente

---

## 2. Tiempo de duración

50 minutos

---

## 3. Fundamentos

Docker es una plataforma de contenedorización que permite empaquetar aplicaciones junto con todas sus dependencias dentro de entornos aislados llamados contenedores. A diferencia de las máquinas virtuales, los contenedores comparten el kernel del sistema operativo anfitrión, lo cual los hace más livianos, portables y rápidos de iniciar. Esta característica es fundamental en el desarrollo moderno de software, donde se busca que una aplicación funcione de la misma manera en cualquier entorno, ya sea en una laptop de desarrollo, un servidor de pruebas o un entorno de producción en la nube.

En esta práctica se trabajó con dos aplicaciones separadas: un backend desarrollado con Node.js y Express que expone una API REST, y un frontend desarrollado con React y Vite que consume esa API para mostrar datos en una tabla. Ambas aplicaciones fueron contenerizadas por separado usando Dockerfile individuales, y luego orquestadas en conjunto mediante Docker Compose.

La comunicación entre contenedores es uno de los conceptos más importantes en arquitecturas de microservicios. Cuando dos contenedores se levantan con Docker Compose sin una red personalizada, Docker crea automáticamente una red bridge por defecto que les permite comunicarse entre sí usando el nombre del servicio como hostname. En este caso, el frontend pudo comunicarse con el backend a través de la red interna de Docker Compose.

Docker Compose es una herramienta que permite definir y ejecutar aplicaciones multi-contenedor mediante un archivo `docker-compose.yml`. En este archivo se describen todos los servicios, sus imágenes o contextos de construcción, los puertos expuestos y las dependencias entre servicios. La directiva `depends_on` es particularmente útil para garantizar que un servicio no intente iniciarse antes de que sus dependencias estén disponibles.

El frontend fue construido con React y Vite. Vite es un bundler moderno que ofrece tiempos de desarrollo extremadamente rápidos gracias a su arquitectura basada en módulos ES nativos. Para que el servidor de desarrollo de Vite sea accesible desde fuera del contenedor, es necesario iniciarlo con el flag `--host`, ya que por defecto solo escucha en `localhost` dentro del contenedor, lo que impediría el acceso desde el navegador del anfitrión.

El backend fue construido con Node.js y Express, un framework minimalista para crear servidores HTTP y APIs REST. Se utilizó el paquete `cors` para permitir que el frontend, servido desde un origen diferente (puerto 5173), pueda realizar peticiones HTTP al backend (puerto 3000) sin que el navegador las bloquee por la política de Same-Origin. Esta es una configuración habitual cuando el frontend y el backend corren en puertos distintos durante el desarrollo.

En conclusión, esta práctica permitió comprender de manera práctica cómo contenerizar aplicaciones frontend y backend de forma independiente, cómo lograr que se comuniquen dentro de una red Docker Compose y cómo gestionar sus Dockerfiles para construir imágenes personalizadas para cada servicio.

---

## 4. Conocimientos previos

- Uso básico de la terminal en Linux.
- Comandos básicos de Docker.
- Conceptos básicos de Docker Compose.
- Fundamentos de Node.js y npm.
- Conceptos básicos de React y Vite.
- Comprensión de peticiones HTTP y APIs REST.
- Manejo de puertos locales.
- Uso del navegador para verificar servicios web.
- Conceptos básicos de CORS.

---

## 5. Objetivos a alcanzar

- Crear un backend con Node.js y Express que exponga una API REST.
- Verificar el funcionamiento del backend localmente antes de contenedorizarlo.
- Crear un frontend con React y Vite que consuma la API del backend.
- Crear un Dockerfile para el backend.
- Crear un Dockerfile para el frontend.
- Construir las imágenes de ambos servicios con Docker.
- Crear un archivo `docker-compose.yml` que orqueste ambos servicios.
- Levantar ambos contenedores con Docker Compose.
- Verificar que los contenedores estén activos y en los puertos correctos.
- Comprobar que el frontend muestre correctamente los datos del backend desde el navegador.

---

## 6. Equipo necesario

- Computador con Arch Linux.
- Docker instalado y activo.
- Docker Compose instalado.
- Node.js y npm instalados.
- Terminal zsh o bash.
- Navegador web.
- Conexión a internet.
- Editor de texto nano o Visual Studio Code.

---

## 7. Material de apoyo

- Documentación oficial de Docker: https://docs.docker.com
- Documentación oficial de Docker Compose: https://docs.docker.com/compose/
- Documentación oficial de Node.js: https://nodejs.org/es/docs
- Documentación oficial de Express: https://expressjs.com/es/
- Documentación oficial de React: https://es.react.dev
- Documentación oficial de Vite: https://vitejs.dev/guide/

---

## 8. Procedimiento

### Paso 1: Crear la carpeta de trabajo y verificar herramientas instaladas

Primero se creó la carpeta principal del proyecto llamada `PracticaSemana8` y se accedió a ella. Luego se verificaron las versiones de Docker, Docker Compose, Node.js y npm instaladas en el sistema para confirmar que todas las herramientas estaban disponibles antes de comenzar.

```bash
mkdir ~/PracticaSemana8
cd ~/PracticaSemana8
pwd
docker --version
docker compose version
node -v
npm -v
```

<img width="567" height="289" alt="image" src="https://github.com/user-attachments/assets/2b5d5640-0596-42a9-abca-69f0b284eee0" />


<p align="center">
  <em>Figura 1. Creación de la carpeta de trabajo y verificación de versiones de Docker, Node.js y npm.</em>
</p>

---

### Paso 2: Crear el proyecto backend con Node.js

Dentro de la carpeta principal se creó la subcarpeta `backend` y se inicializó un nuevo proyecto Node.js con `npm init -y`. Esto generó el archivo `package.json` con la configuración básica del proyecto.

```bash
mkdir backend
cd backend
npm init -y
```

<img width="561" height="384" alt="image" src="https://github.com/user-attachments/assets/bb6a6233-83c2-4567-8dce-8f5917b3b7df" />

<img width="702" height="647" alt="image" src="https://github.com/user-attachments/assets/649ec55b-9458-44b6-be09-fdcceb385f19" />

<p align="center">
  <em>Figura 2. Inicialización del proyecto backend con npm init.</em>
</p>

---

### Paso 3: Instalar dependencias del backend

Se instalaron los paquetes `express` y `cors`, que son las dos dependencias necesarias para el servidor. Express permite crear el servidor HTTP y definir rutas de la API, mientras que cors habilita las peticiones cruzadas desde el frontend.

```bash
npm install express cors
```

<img width="701" height="374" alt="image" src="https://github.com/user-attachments/assets/70313ef7-a7a9-48ae-9a41-9dea78e83563" />


<p align="center">
  <em>Figura 3. Instalación de las dependencias express y cors en el proyecto backend.</em>
</p>

---

### Paso 4: Crear el archivo server.js del backend

Se creó el archivo principal `server.js` con nano. Dentro de este archivo se definieron los datos de los parlantes como un arreglo de objetos en memoria, se habilitó CORS y se configuró una ruta GET en `/parlantes` que devuelve esos datos en formato JSON. El servidor queda a la escucha en el puerto 3000.

```bash
nano server.js
```

Contenido del archivo:

```javascript
const express = require('express');
const cors = require('cors');

const app = express();

app.use(cors());

const parlantes = [
  { id: 1, nombre: "SARA Speaker 1", bateria: 95, volumen: 80 },
  { id: 2, nombre: "SARA Speaker 2", bateria: 70, volumen: 50 },
  { id: 3, nombre: "SARA Speaker 3", bateria: 85, volumen: 65 }
];

app.get('/parlantes', (req, res) => {
  res.json(parlantes);
});

app.listen(3000, () => {
  console.log('Servidor iniciado en puerto 3000');
});
```

<img width="948" height="929" alt="image" src="https://github.com/user-attachments/assets/cc91b0f6-7577-40d2-a63e-2f0690047383" />


<p align="center">
  <em>Figura 4. Contenido del archivo server.js con la API REST de parlantes.</em>
</p>

---

### Paso 5: Probar el backend localmente

Antes de contenedorizarlo, se probó el servidor de forma local ejecutando `node server.js`. El servidor indicó que inició correctamente en el puerto 3000.

```bash
node server.js
```

<img width="639" height="165" alt="image" src="https://github.com/user-attachments/assets/dc8b1749-53a0-448d-b33f-82ea272e3145" />


<p align="center">
  <em>Figura 5. Servidor backend iniciado localmente en el puerto 3000.</em>
</p>

---

### Paso 6: Verificar la API con curl

Para confirmar que la API respondía correctamente, se realizó una petición con `curl` al endpoint `/parlantes`. La respuesta mostró el arreglo JSON con los tres parlantes definidos en el servidor.

```bash
curl http://localhost:3000/parlantes
```

<img width="955" height="300" alt="image" src="https://github.com/user-attachments/assets/5e0a2c6e-fbdb-4dd7-9d2e-6d6d2b12c201" />


<p align="center">
  <em>Figura 6. Verificación de la API mediante curl mostrando los datos JSON de los parlantes.</em>
</p>

---

### Paso 7: Crear el Dockerfile del backend

Se creó el archivo `Dockerfile` dentro de la carpeta `backend`. Este archivo usa la imagen base `node:20`, define el directorio de trabajo como `/app`, copia el `package*.json` para instalar dependencias, luego copia el resto del código, expone el puerto 3000 y ejecuta el servidor con Node.js.

```bash
nano Dockerfile
```

Contenido del archivo:

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node","server.js"]
```

<img width="930" height="481" alt="image" src="https://github.com/user-attachments/assets/436add93-e330-450a-8488-cea9dc1ec06e" />

<p align="center">
  <em>Figura 7. Dockerfile del backend para construir la imagen del servidor Node.js.</em>
</p>

---

### Paso 8: Construir la imagen del backend y verificarla

Se construyó la imagen del backend con el comando `docker build`. El proceso completó todos los pasos correctamente. Luego se ejecutó `docker images` para confirmar que la imagen `backend-api:latest` fue creada con un tamaño de 1.59 GB.

```bash
docker build -t backend-api .
docker images
```

<img width="658" height="466" alt="image" src="https://github.com/user-attachments/assets/fc4ae9aa-a5be-415d-b202-4500847f2193" />


<p align="center">
  <em>Figura 8. Construcción exitosa de la imagen backend-api y listado de imágenes disponibles.</em>
</p>

---

### Paso 9: Crear el proyecto frontend con React y Vite

De vuelta en la carpeta principal `PracticaSemana8`, se accedió a la carpeta `frontend` (previamente generada con Vite) y se ejecutó `npm install` para instalar todas las dependencias del proyecto.

```bash
cd frontend
npm install
```

<img width="650" height="506" alt="image" src="https://github.com/user-attachments/assets/d765cbc6-0cf0-468e-bc06-82301d0e1fe1" />


<p align="center">
  <em>Figura 9. Instalación de dependencias del proyecto frontend con React y Vite.</em>
</p>

---

### Paso 10: Instalar axios en el frontend

Se instaló el paquete `axios` en el proyecto frontend. Axios es una librería que simplifica las peticiones HTTP desde React, permitiendo consumir la API del backend de forma sencilla.

```bash
npm install axios
```

<img width="689" height="368" alt="image" src="https://github.com/user-attachments/assets/5fdf82b9-7342-4eb0-856d-8a9b824b1a81" />


<p align="center">
  <em>Figura 10. Instalación de axios como dependencia del frontend.</em>
</p>

---

### Paso 11: Modificar el componente App.jsx

Se editó el archivo `src/App.jsx` para que consuma la API del backend y muestre los datos en una tabla HTML. Se usaron los hooks `useState` y `useEffect` de React para hacer la petición con axios al iniciar el componente y almacenar los datos en el estado.

```bash
nano src/App.jsx
```

Contenido del archivo:

```jsx
import { useEffect, useState } from "react";
import axios from "axios";

function App() {
  const [parlantes, setParlantes] = useState([]);

  useEffect(() => {
    axios
      .get("http://localhost:3000/parlantes")
      .then((response) => {
        setParlantes(response.data);
      })
      .catch((error) => {
        console.error(error);
      });
  }, []);

  return (
    <div>
      <h1>Parlantes S.A.R.A.</h1>
      <table border="1">
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Batería</th>
            <th>Volumen</th>
          </tr>
        </thead>
        <tbody>
          {parlantes.map((p) => (
            <tr key={p.id}>
              <td>{p.id}</td>
              <td>{p.nombre}</td>
              <td>{p.bateria}%</td>
              <td>{p.volumen}%</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default App;
```

<img width="944" height="1030" alt="image" src="https://github.com/user-attachments/assets/f2810082-5a05-4d12-8fbf-ae0872855b2b" />


<p align="center">
  <em>Figura 11. Componente App.jsx configurado para consumir la API y mostrar los datos en tabla.</em>
</p>

---

### Paso 12: Probar el frontend localmente

Se ejecutó el servidor de desarrollo de Vite para verificar que el frontend funcionara correctamente antes de contenedorizarlo.

```bash
npm run dev
```

<img width="924" height="409" alt="image" src="https://github.com/user-attachments/assets/00ed8bfa-5d54-4124-a252-926d71cf8d5a" />


<p align="center">
  <em>Figura 12. Servidor de desarrollo Vite iniciado en localhost:5173.</em>
</p>

---

### Paso 13: Verificar la tabla en el navegador sin Docker

Con ambos servidores corriendo localmente, se abrió el navegador en `localhost:5173` y se confirmó que la tabla de Parlantes S.A.R.A. mostraba correctamente los tres registros devueltos por la API.

<img width="1440" height="427" alt="image" src="https://github.com/user-attachments/assets/01f0bfb3-c41d-4403-8d32-06cbb8c87ba4" />


<p align="center">
  <em>Figura 13. Frontend mostrando la tabla de parlantes con datos obtenidos desde la API local.</em>
</p>

---

### Paso 14: Crear el Dockerfile del frontend

Se creó el archivo `Dockerfile` dentro de la carpeta `frontend`. Este Dockerfile usa la imagen base `node:20`, instala las dependencias y ejecuta el servidor de Vite con el flag `--host` para que sea accesible desde fuera del contenedor.

```bash
nano Dockerfile
```

Contenido del archivo:

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm","run","dev","--","--host"]
```

<img width="953" height="531" alt="image" src="https://github.com/user-attachments/assets/8f6be2b1-53e9-4140-b788-e030a81d7cde" />


<p align="center">
  <em>Figura 14. Dockerfile del frontend para construir la imagen del servidor Vite.</em>
</p>

---

### Paso 15: Construir la imagen del frontend y verificar imágenes

Se construyó la imagen del frontend con `docker build`. Al terminar, se ejecutó `docker images` para confirmar que tanto `backend-api:latest` como `frontend-react:latest` estaban disponibles. La imagen del frontend ocupó 1.86 GB.

```bash
docker build -t frontend-react .
docker images
```

<img width="936" height="710" alt="image" src="https://github.com/user-attachments/assets/6a41b3b8-6c1c-4cf1-9262-0a42156d49f1" />


<p align="center">
  <em>Figura 15. Construcción exitosa de la imagen frontend-react y listado de ambas imágenes.</em>
</p>

---

### Paso 16: Crear el archivo docker-compose.yml

De regreso en la carpeta raíz `PracticaSemana8`, se creó el archivo `docker-compose.yml` con la configuración de los dos servicios. El servicio `backend` construye desde la carpeta `./backend` y expone el puerto 3000. El servicio `frontend` construye desde `./frontend`, expone el puerto 5173 y depende del servicio backend.

```bash
nano docker-compose.yml
```

Contenido del archivo:

```yaml
services:
  backend:
    build: ./backend
    container_name: backend-api
    ports:
      - "3000:3000"

  frontend:
    build: ./frontend
    container_name: frontend-react
    ports:
      - "5173:5173"
    depends_on:
      - backend
```

<img width="971" height="588" alt="image" src="https://github.com/user-attachments/assets/7dd9d3f3-1d71-4e49-8713-a4dcf623a762" />


<p align="center">
  <em>Figura 16. Archivo docker-compose.yml con la configuración de los servicios backend y frontend.</em>
</p>

---

### Paso 17: Levantar los servicios con Docker Compose

Se ejecutó `docker compose up --build` para construir y levantar ambos contenedores. En la salida se observó que las imágenes fueron construidas correctamente, la red `practicasemana8_default` fue creada, los contenedores `backend-api` y `frontend-react` se iniciaron, y tanto el servidor Express como el servidor Vite quedaron activos.

```bash
docker compose up --build
```

<img width="966" height="1021" alt="image" src="https://github.com/user-attachments/assets/9903442f-c8ca-4328-8ad1-608387e72b3b" />


<p align="center">
  <em>Figura 17. Construcción y levantamiento de ambos contenedores con Docker Compose.</em>
</p>

---

### Paso 18: Verificar los contenedores activos

En otra terminal se ejecutó `docker ps` para confirmar que ambos contenedores estuvieran corriendo correctamente. Se observó que `frontend-react` estaba activo en el puerto `5173` y `backend-api` estaba activo en el puerto `3000`.

```bash
docker ps
```

<img width="944" height="412" alt="image" src="https://github.com/user-attachments/assets/94d07517-d67f-4ff0-807d-f153ebecf29a" />


<p align="center">
  <em>Figura 18. Verificación de los contenedores activos de backend y frontend con docker ps.</em>
</p>

---

### Paso 19: Verificar el frontend desde el navegador con Docker

Finalmente, se abrió el navegador en `localhost:5173` con ambos contenedores en ejecución. La tabla de Parlantes S.A.R.A. se mostró correctamente con los tres registros, confirmando que el frontend containerizado pudo comunicarse con el backend containerizado y obtener los datos de la API.

<img width="1600" height="515" alt="image" src="https://github.com/user-attachments/assets/48c2ec9a-f3da-45ec-8b75-75f175bd5335" />

<p align="center">
  <em>Figura 19. Frontend React en contenedor mostrando datos de la API backend desde Docker Compose.</em>
</p>

---

## 9. Resultados esperados

Al finalizar la práctica se logró contenerizar de forma exitosa una aplicación frontend desarrollada con React y Vite, y comunicarla con un backend Node.js y Express alojado en un contenedor independiente. Primero se verificaron las herramientas instaladas en el sistema, incluyendo Docker, Docker Compose, Node.js y npm. Luego se construyó el backend con una API REST que expone datos de parlantes en formato JSON, y se verificó su correcto funcionamiento tanto de forma local como mediante una petición con curl.

Después se construyó el frontend con React y Vite, configurando el componente principal para consumir la API del backend con axios y mostrar los datos en una tabla HTML. Ambas aplicaciones fueron probadas localmente antes de ser contenerizadas.

Se crearon Dockerfiles independientes para cada servicio y se construyeron sus respectivas imágenes con Docker. Luego se creó el archivo `docker-compose.yml` para orquestar ambos contenedores, definiendo sus puertos y la dependencia del frontend con respecto al backend. Al ejecutar `docker compose up --build`, ambos servicios se iniciaron correctamente dentro de una red Docker automática, y el frontend pudo mostrar los datos del backend en el navegador desde `localhost:5173`.

---

## 10. Conclusiones

- Se creó correctamente un backend con Node.js y Express que expone una API REST en el puerto 3000.
- Se verificó el funcionamiento de la API de forma local antes de contenedizarla.
- Se construyó un frontend con React y Vite que consume la API mediante axios.
- Se confirmó que el frontend muestra correctamente los datos en una tabla HTML.
- Se crearon Dockerfiles independientes para el backend y el frontend.
- Se construyeron exitosamente las imágenes Docker de ambos servicios.
- Se configuró un archivo `docker-compose.yml` para orquestar los dos contenedores.
- Se verificó que Docker Compose crea automáticamente una red que permite la comunicación entre contenedores.
- Se comprobó con `docker ps` que ambos contenedores estaban activos en sus puertos correspondientes.
- Se confirmó desde el navegador que el frontend containerizado muestra los datos del backend containerizado.
- El uso del flag `--host` en Vite fue necesario para exponer el servidor fuera del contenedor.
- La directiva `depends_on` en Docker Compose garantizó el orden correcto de inicio de los servicios.

---

## 11. Recomendaciones

- Verificar que Docker esté activo antes de ejecutar cualquier comando de Docker Compose.
- Siempre probar los servicios de forma local antes de contenedorizarlos para detectar errores más fácilmente.
- Usar el flag `--host` al ejecutar Vite dentro de un contenedor, de lo contrario no será accesible desde el navegador del anfitrión.
- Configurar `cors` en el backend para evitar errores de política de mismo origen al hacer peticiones desde el frontend.
- Usar `depends_on` en `docker-compose.yml` para definir el orden de inicio de los servicios.
- Nombrar los contenedores de forma descriptiva para facilitar su identificación con `docker ps`.
- Revisar los logs de Docker Compose si algún servicio no inicia correctamente.
- Utilizar `docker images` para verificar que las imágenes fueron construidas antes de levantarlas.
- En producción, reemplazar el servidor de desarrollo de Vite por una imagen de Nginx que sirva el build estático, lo que resulta en imágenes más pequeñas y seguras.
- Mantener los Dockerfiles simples y bien documentados para facilitar el mantenimiento del proyecto.

---

## 12. Bibliografía

Docker Inc. (s.f.). *Descripción general de Docker Compose*. Docker Documentation. https://docs.docker.com/compose/

Docker Inc. (s.f.). *Referencia de Dockerfile*. Docker Documentation. https://docs.docker.com/reference/dockerfile/

Docker Inc. (s.f.). *Redes en Docker Compose*. Docker Documentation. https://docs.docker.com/compose/networking/

OpenJS Foundation. (s.f.). *Documentación oficial de Node.js*. Node.js. https://nodejs.org/es/docs

Express.js. (s.f.). *Guía de inicio con Express*. Express. https://expressjs.com/es/starter/hello-world.html

Meta Open Source. (s.f.). *Inicio rápido con React*. React. https://es.react.dev/learn

Evan You y equipo de Vite. (s.f.). *Guía oficial de Vite*. Vitejs. https://vitejs.dev/guide/

npm, Inc. (s.f.). *Documentación de axios*. npm. https://www.npmjs.com/package/axios

npm, Inc. (s.f.). *Documentación de cors*. npm. https://www.npmjs.com/package/cors
