---
{"dg-publish":true,"permalink":"/sistemas-informaticos/00-general/proyecto-anual-game-over/","tags":["asignatura/sistemas-informaticos","bash","proyecto"],"dgShowToc":true,"noteIcon":"","dg-note-properties":{"tipo":"proyecto","asignatura":"Sistemas Informáticos","tags":["asignatura/sistemas-informaticos","bash","proyecto"],"status":"En progreso"}}
---


# 🎮 Proyecto anual "Game Over"

↑ [[Sistemas Informáticos/Sistemas Informáticos\|Sistemas Informáticos]]

> [!note] Descripción (programación del módulo)
> Proyecto paralelo a las unidades didácticas que consiste en **versionar juegos tradicionales como videojuegos** utilizando **scripts de Bash**. Con cierta periodicidad se dedican sesiones al estudio de las herramientas de Bash (**variables, bucles, funciones…**) para llevarlas a la práctica y que el alumnado pueda seguir trabajando en su videojuego.

## 📐 Tu primer script de Bash: ¡Hola, Mundo!

Fuente: infografía "Tu primer script de Bash".

1. **Crea el archivo `saludo.sh`** con cualquier editor de texto (por ejemplo, `nano`).
2. **Escribe el código:** la primera línea es el *shebang* y la segunda el `echo`.
3. **Dale permisos de ejecución** desde la terminal, en la carpeta del archivo.
4. **Ejecuta el script.**

```bash
nano saludo.sh
```

```bash
#!/bin/bash
echo "¡Hola, Mundo!"
```

```bash
chmod 777 ./saludo.sh
./saludo.sh
```

> [!tip] Permisos
> La fuente usa `chmod 777`. Los permisos en Linux se estudian en [[Sistemas Informáticos/Unidad 4 Sistemas operativos. Gestión de usuarios y procesos/Apuntes/3. Permisos locales, máscara y perfiles de usuario\|3. Permisos locales, máscara y perfiles de usuario]] (UD4); allí se verá que basta con dar permiso de ejecución al propietario (`chmod u+x saludo.sh`, conocimiento general, verificar).

## 🛠️ Herramientas de Bash

> [!warning] Falta fuente: variables
> No hay material sobre variables en Bash en las fuentes.

> [!warning] Falta fuente: estructuras condicionales y bucles

> [!warning] Falta fuente: funciones

> [!warning] Falta fuente: lectura de teclado, números aleatorios y otras utilidades para juegos

## 🔗 Relación con las unidades

| UD | Relación con el proyecto |
| :-: | --- |
| [[Sistemas Informáticos/Unidad 3 Sistemas operativos. Gestión de archivos y almacenamiento/Apuntes/3. Gestión de archivos por comandos y entorno gráfico\|UD3]] | Comandos de gestión de archivos, redirecciones y procesamiento de textos (Tema 3, §2.4) |
| [[Sistemas Informáticos/Unidad 4 Sistemas operativos. Gestión de usuarios y procesos/Apuntes/3. Permisos locales, máscara y perfiles de usuario\|UD4]] | Permisos (`chmod`), procesos y scripts de automatización de tareas (Tema 4, §1.1.6) y [[Sistemas Informáticos/Unidad 3 Sistemas operativos. Gestión de archivos y almacenamiento/Apuntes/9. Automatización y programación de tareas\|9. Automatización y programación de tareas]] (UD3) |

> [!warning] Falta fuente: enunciado del proyecto
> No hay en las fuentes la lista de juegos, los hitos ni la rúbrica del proyecto.
