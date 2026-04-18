# Practica: Manipulación de archivos y directorios de linux

## 1. Titulo

Creación y manipulación de archivos y directorios en Linux mediante comandos de terminal

## 2. Tiempo de duración

30 minutos


## 3. Fundamentos:

Linux es un sistema operativo que permite trabajar mediante una interfaz de línea de comandos (CLI), donde el usuario puede interactuar directamente con el sistema utilizando instrucciones específicas. Esta forma de trabajo es ampliamente utilizada en entornos profesionales, especialmente en servidores y desarrollo de software.

En esta práctica se utilizaron comandos básicos de Linux para la gestión de archivos y carpetas. El comando mkdir permite crear directorios, mientras que cd se utiliza para navegar entre ellos. También se empleó echo para escribir contenido dentro de archivos de texto desde la terminal.

Para la manipulación de archivos se utilizaron comandos como cp, que permite copiar archivos, mv para moverlos de una ubicación a otra, y rm para eliminarlos. Estos comandos son fundamentales para organizar la información dentro del sistema operativo.

Además, se aplicó el uso de redirección de salida. El operador > permite guardar la salida de un comando en un archivo, sobrescribiendo su contenido, mientras que >> permite añadir información sin borrar lo anterior. Esto es útil para construir archivos de manera progresiva.

Finalmente, se utilizó el comando history junto con tuberías (|) para almacenar los comandos ejecutados en un archivo de texto.

Además, el uso de la terminal en Linux permite automatizar tareas mediante scripts, lo cual es muy útil en entornos de desarrollo y administración de sistemas. A diferencia de las interfaces gráficas, la línea de comandos ofrece mayor control y eficiencia, permitiendo ejecutar múltiples acciones con una sola instrucción. 

Este tipo de habilidades son altamente valoradas en el ámbito profesional, ya que facilitan la gestión de servidores, la manipulación de archivos y la resolución de problemas técnicos de forma rápida y precisa.



## 4. Conocimientos previos.
   
- Comandos básicos de Linux  
- Uso de la terminal  
- Manejo de archivos y carpetas  

---

## 5. Objetivos a alcanzar
   
- Crear estructuras de carpetas mediante comandos Linux  
- Manipular archivos desde la terminal  
- Aplicar redirección de salida  
- Copiar, mover y eliminar archivos correctamente  
- Guardar el historial de comandos  

---

## 6. Equipo necesario:
  
- Computador con sistema operativo Linux (Arch Linux)  
- Terminal (zsh o bash)  
- Editor de texto (nano o vim)  
- Cuenta en GitHub  

---

## 7. Material de apoyo.
   
- Documentación oficial de Linux  
- Guía de la asignatura  
- Cheat sheet de comandos Linux  

---

## 8. Procedimiento

Paso 1: Crear la estructura de carpetas  
mkdir -p proyecto_comandos/{documentos,imagenes,scripts}

Paso 2: Crear archivo y agregar contenido  
cd proyecto_comandos/documentos  
echo "Linea 1" > notas.txt  
echo "Linea 2" >> notas.txt  
echo "Linea 3" >> notas.txt  

Paso 3: Copiar archivo  
cp notas.txt ../scripts/backup_notas.txt  

Paso 4: Mover archivo  
mv ../scripts/backup_notas.txt ../imagenes/  

Paso 5: Redirección y concatenación  
touch resumen.txt  
cat notas.txt > resumen.txt  
echo "Linea adicional" >> resumen.txt  

Paso 6: Eliminación  
rm ../imagenes/backup_notas.txt  
rmdir ../imagenes  

Paso 7: Guardar historial  
history | tail -n 30 > tarea-s1-edisson_padilla.txt  

Figura 1-1. Estructura de carpetas creada mediante comandos Linux.

---

## 9. Resultados esperados:
    
Se logró crear correctamente la estructura de carpetas, manipular archivos mediante comandos y aplicar redirección de datos. Además, se generó un archivo con el historial de comandos ejecutados.

---

## 10. Bibliografía

- Red Hat. (2023). Introducción a Linux. https://www.redhat.com/es/topics/linux/what-is-linux  
- GNU. (2023). Manual de Bash. https://www.gnu.org/software/bash/manual/bash.html  
