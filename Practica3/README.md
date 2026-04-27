# Práctica: Persistencia de datos con volúmenes en Docker (PostgreSQL)

---

## 1. Titulo

Implementación de volúmenes en Docker para la persistencia de datos en PostgreSQL

---

## 2. Tiempo de duración

50 minutos

---

## 3. Fundamentos

Docker es una plataforma que permite ejecutar aplicaciones dentro de contenedores, los cuales son entornos aislados que incluyen todo lo necesario para su funcionamiento. Estos contenedores son ligeros y rápidos en comparación con las máquinas virtuales, ya que comparten el kernel del sistema operativo anfitrión.

Sin embargo, uno de los principales problemas al trabajar con contenedores es que su sistema de archivos es temporal. Esto significa que cualquier dato que se genere dentro de un contenedor puede perderse cuando este es eliminado. En el caso de bases de datos, esto representa un gran problema, ya que la información almacenada es crítica.

Para solucionar este inconveniente, Docker proporciona el uso de volúmenes. Un volumen es un mecanismo que permite almacenar datos fuera del contenedor, directamente en el sistema anfitrión. De esta manera, aunque el contenedor sea eliminado, los datos permanecen intactos.

Existen diferentes tipos de almacenamiento en Docker, pero los volúmenes son los más recomendados para bases de datos porque permiten persistencia, seguridad y facilidad de uso. Además, los volúmenes pueden reutilizarse en múltiples contenedores, lo que facilita la migración y respaldo de información.

En esta práctica se trabajó con PostgreSQL dentro de Docker para demostrar la diferencia entre ejecutar una base de datos sin volumen (donde los datos se pierden) y con volumen (donde los datos se conservan). Esto permite comprender la importancia de la persistencia en entornos reales de desarrollo.

<img width="6084" height="3042" alt="image" src="https://github.com/user-attachments/assets/dbf30da4-f7d9-4355-a987-18ea9d45a58d" />


<p align="center">
  <em>Figura 1. Funcionamiento de contenedores y volúmenes en Docker</em>
</p>

---

## 4. Conocimientos previos

- Comandos básicos de Linux  
- Uso de Docker  
- Manejo de terminal  
- Conceptos básicos de bases de datos  

---

## 5. Objetivos a alcanzar

- Comprender el funcionamiento de contenedores Docker  
- Implementar PostgreSQL en Docker  
- Analizar la pérdida de datos sin volumen  
- Aplicar volúmenes para persistencia de datos  
- Verificar la conservación de información  

---

## 6. Equipo necesario

- Computador con Linux (Arch Linux)  
- Docker instalado  
- Terminal (zsh o bash)  
- PostgreSQL en contenedor  

---

## 7. Material de apoyo

- Documentación oficial de Docker  
- Documentación de PostgreSQL  
- Guía de la asignatura  

---

## 8. Procedimiento

### 🔹 Parte 1: Base de datos sin volumen

**Paso 1: Crear contenedor PostgreSQL**

<img width="1491" height="627" alt="image" src="https://github.com/user-attachments/assets/338fdcbd-b41d-4bf7-995c-56281b22b11b" />


**Paso 2: Crear base de datos**

<img width="673" height="324" alt="image" src="https://github.com/user-attachments/assets/ada4c35f-2e3e-4446-a1b3-7f41a1b0d974" />


**Paso 3: Crear tabla e insertar datos**

<img width="618" height="588" alt="image" src="https://github.com/user-attachments/assets/22de4b4c-4ba1-402e-9165-a76d22b1deb8" />


**Paso 4: Eliminar contenedor**

<img width="737" height="752" alt="image" src="https://github.com/user-attachments/assets/07b046fd-1466-4219-ba56-a7bc2558eb5c" />


**Paso 5: Volver a crear contenedor y verificar pérdida de datos**

<img width="1413" height="524" alt="image" src="https://github.com/user-attachments/assets/a67d7236-d324-4b01-8cd8-75cf13970305" />


---

### 🔹 Parte 2: Base de datos con volumen

**Paso 6: Crear volumen**

<img width="853" height="280" alt="image" src="https://github.com/user-attachments/assets/01244e50-bc07-41e0-a253-9e129713768a" />


**Paso 7: Crear contenedor con volumen**

<img width="1423" height="711" alt="image" src="https://github.com/user-attachments/assets/f5b495c1-2776-4464-9939-467422e05131" />


**Paso 8: Crear base de datos y registros**

<img width="1423" height="711" alt="image" src="https://github.com/user-attachments/assets/b0f55a8e-e99c-4a30-941e-849fe69d32f5" />


**Paso 9: Eliminar contenedor**

<img width="1468" height="288" alt="image" src="https://github.com/user-attachments/assets/edff8da0-d77f-4017-9a68-68253832575b" />


**Paso 10: Recrear contenedor y verificar persistencia**

<img width="1484" height="674" alt="image" src="https://github.com/user-attachments/assets/266239f0-fe07-488a-89f5-ecaa84634696" />


---

## 9. Resultados esperados

En la primera parte se comprobó que los datos almacenados en el contenedor se pierden al eliminarlo, ya que no existe persistencia.

En la segunda parte, al utilizar un volumen, los datos se conservaron incluso después de eliminar y recrear el contenedor, demostrando la importancia de los volúmenes en Docker para aplicaciones reales.

---

## 10. Bibliografía

- Docker Inc. (2024). *¿Qué es Docker?* https://www.docker.com/resources/what-container  
- Red Hat. (2023). *Contenedores y Docker*. https://www.redhat.com/es/topics/containers  
- PostgreSQL Global Development Group. (2024). *Documentación PostgreSQL*. https://www.postgresql.org/docs/  
