# Practica: Creación y personalización de servidores web con Docker (Nginx)

---

## 1. Titulo

Implementación de servidores web mediante contenedores Docker utilizando Nginx

---

## 2. Tiempo de duración

40 minutos

---

## 3. Fundamentos

Docker es una plataforma que permite ejecutar aplicaciones dentro de contenedores, los cuales son entornos aislados que incluyen todo lo necesario para su funcionamiento. Esto evita problemas de compatibilidad entre sistemas y facilita el desarrollo de software.

A diferencia de las máquinas virtuales, los contenedores no requieren un sistema operativo completo, ya que comparten el kernel del sistema anfitrión. Esto los hace más ligeros, rápidos y eficientes en el uso de recursos.

En esta práctica se utilizó la imagen de Nginx para crear dos contenedores independientes. Cada uno se ejecuta en un puerto distinto, lo que permite acceder a ambos desde el navegador sin conflictos.

Además, se utilizó el comando `docker cp` para copiar archivos desde el contenedor al sistema anfitrión y viceversa. Esto permitió editar el archivo `index.html` y personalizar el contenido de cada servidor.

Docker es ampliamente utilizado en el desarrollo moderno porque permite trabajar con entornos consistentes, facilitando la implementación y el despliegue de aplicaciones en diferentes sistemas.

<img width="1414" height="595" alt="image" src="https://github.com/user-attachments/assets/30b62aea-d164-468d-b462-127ec6728ac4" />


<p align="center">
  <em>Figura 1-1. Creación de contenedores nginx</em>
</p>

---

## 4. Conocimientos previos

- Uso de la terminal en Linux  
- Comandos básicos de Docker  
- Manejo de archivos  
- Uso de navegador web  
- Edición de archivos con nano  

---

## 5. Objetivos a alcanzar

- Crear contenedores con Docker  
- Ejecutar múltiples servicios web  
- Exponer puertos correctamente  
- Copiar y modificar archivos dentro de contenedores  
- Personalizar páginas web  

---

## 6. Equipo necesario

- Computador con Linux (Arch Linux)  
- Docker instalado  
- Terminal (zsh o bash)  
- Navegador web  
- Editor de texto (nano)  
- Cuenta en GitHub  

---

## 7. Material de apoyo

- https://docs.docker.com  
- Guía de la asignatura  
- Recursos de clase  

---

## 8. Procedimiento

### Paso 1: Crear contenedores

Se ejecutaron los siguientes comandos:

docker run --name nginx1 -d -p 8089:80 nginx  
docker run --name nginx2 -d -p 8090:80 nginx  

<img width="1414" height="595" alt="image" src="https://github.com/user-attachments/assets/1bdfdccc-5274-4471-b726-b1091a8de90c" />


<p align="center">
  <em>Figura 1-2. Creación de contenedores</em>
</p>

---

### Paso 2: Verificar contenedores activos

docker ps  

<img width="1414" height="595" alt="image" src="https://github.com/user-attachments/assets/addce58b-4333-472b-ba91-5faadd1903c9" />


<p align="center">
  <em>Figura 1-3. Contenedores en ejecución</em>
</p>

---

### Paso 3: Copiar archivo desde contenedor

docker cp nginx1:/usr/share/nginx/html/index.html ./index1.html  

<img width="766" height="73" alt="image" src="https://github.com/user-attachments/assets/921f93ff-2990-4094-9a40-5d42b4c690a2" />


<p align="center">
  <em>Figura 1-4. Copia del archivo index.html</em>
</p>

---

### Paso 4: Editar archivo

Se utilizó nano para modificar el contenido del archivo:

nano index1.html  

<img width="1243" height="713" alt="image" src="https://github.com/user-attachments/assets/68293f4e-6294-4604-8679-ba67daa0e068" />


<p align="center">
  <em>Figura 1-5. Edición del archivo HTML</em>
</p>

---

### Paso 5: Subir archivo al contenedor

docker cp index1.html nginx1:/usr/share/nginx/html/index.html  

---

### Paso 6: Verificar en navegador (nginx1)

http://localhost:8089  

<img width="854" height="699" alt="image" src="https://github.com/user-attachments/assets/3aa469f2-822c-4c62-b3c1-5537c0804566" />


<p align="center">
  <em>Figura 1-6. Servidor nginx1 personalizado</em>
</p>

---

### Paso 7: Repetir proceso con nginx2

docker cp nginx2:/usr/share/nginx/html/index.html index2.html  
nano index2.html  
docker cp index2.html nginx2:/usr/share/nginx/html/index.html  

<img width="707" height="377" alt="image" src="https://github.com/user-attachments/assets/ad6de254-bd9c-4dd8-8294-25557e11f17c" />


<p align="center">
  <em>Figura 1-7. Edición de datos del estudiante</em>
</p>

---

### Paso 8: Verificar en navegador (nginx2)

http://localhost:8090  

<img width="570" height="404" alt="image" src="https://github.com/user-attachments/assets/d8130926-cb97-4a9a-874c-bf50b0d4c529" />


<p align="center">
  <em>Figura 1-8. Servidor nginx2 personalizado</em>
</p>

---

## 9. Resultados esperados

Se logró crear dos contenedores funcionales con Nginx, cada uno accesible mediante un puerto diferente. Además, se modificó correctamente el contenido de cada servidor web, mostrando información institucional en uno y datos personales en el otro.

Esto demuestra el uso práctico de Docker para desplegar servicios web y manipular archivos dentro de contenedores.

---

## 10. Bibliografía

- Docker Inc. (2024). ¿Qué es Docker? https://www.docker.com/resources/what-container  
- IBM. (2023). Contenedores Docker. https://www.ibm.com/es-es/topics/containers  
- Red Hat. (2023). Contenedores. https://www.redhat.com/es/topics/containers  
