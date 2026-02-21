
## ¿Qué es un repositorio en Git?

Un repositorio en Git es un proyecto que está bajo control de versiones.
Permite guardar el historial de cambios, volver a versiones anteriores y trabajar en equipo.

### Diferencia entre proyecto normal y repositorio

Un proyecto normal solo contiene archivos.

Un repositorio contiene:
- Archivos
- Historial de cambios
- Información del autor
- Ramas (branches)
- Control de versiones

## Las tres áreas principales de Git

Git trabaja con tres áreas fundamentales que permiten controlar los cambios en un proyecto:

---

### 1 Working Directory (Directorio de Trabajo)

Es el área donde modificamos los archivos del proyecto.

Aquí es donde:
- Creamos archivos
- Editamos código
- Eliminamos archivos

Los cambios que hacemos aquí todavía NO están guardados en el historial de Git.

Ejemplo:
Cuando modificamos un archivo y ejecutamos:

git status

Git detecta que el archivo fue modificado en el Working Directory.

---

### 2️ Staging Area (Index)

Es el área intermedia donde preparamos los archivos antes de guardarlos definitivamente.

Aquí decidimos qué cambios queremos incluir en el próximo commit.

Se utiliza el comando:

git add nombre_archivo

O para agregar todo:

git add .

En esta etapa los cambios están listos para confirmarse, pero aún no están guardados en el historial.

---

### 3️ Repository (Repositorio)

Es el área donde Git guarda permanentemente los cambios confirmados.

Aquí se almacena:
- El historial de versiones
- El autor de cada cambio
- La fecha de cada modificación

Se guarda usando:

git commit -m "Mensaje del cambio"

Después podemos subir los cambios a GitHub con:

git push

---

## Resumen del flujo de trabajo

Working Directory → Staging Area → Repository

Modificar → Agregar → Confirmar

Este flujo permite tener control total sobre las versiones del proyecto.


## ¿Cómo representa Git los cambios internamente?

Git no guarda los cambios como diferencias tradicionales.
Internamente, Git almacena información como una base de datos de objetos.

Los cuatro tipos principales de objetos en Git son:

---

### 1️ Blob (Binary Large Object)

Un blob representa el contenido de un archivo.

- No guarda el nombre del archivo.
- Solo guarda el contenido.
- Cada blob tiene un identificador único (hash SHA-1 o SHA-256).

Si modificas un archivo, Git crea un nuevo blob con el nuevo contenido.

---

### 2️ Tree

Un tree representa la estructura de directorios.

- Contiene referencias a blobs (archivos).
- Contiene referencias a otros trees (subcarpetas).
- Guarda nombres de archivos y permisos.

Es como una "foto" de la estructura del proyecto en un momento determinado.

---

### 3️ Commit

Un commit representa un punto en la historia del proyecto.

Contiene:
- Referencia a un objeto tree
- Autor
- Fecha
- Mensaje del commit
- Referencia al commit anterior (padre)

Cada commit tiene un hash único que lo identifica.

El commit conecta todos los cambios y forma la línea del tiempo del proyecto.

---

### 4️ Tag

Un tag es una referencia especial que apunta a un commit específico.

Se usa normalmente para:
- Marcar versiones importantes
- Crear versiones como v1.0, v2.0, etc.

Ejemplo:
git tag v1.0

El tag no contiene cambios, solo apunta a un commit.

---

## Relación entre los objetos

Archivo → Blob  
Carpeta → Tree  
Estado del proyecto → Commit  
Versión marcada → Tag  

---

## En resumen

Git funciona como una base de datos de objetos enlazados.

Cada vez que haces un commit:
1. Se crean blobs para los archivos.
2. Se crea un tree para la estructura.
3. Se crea un commit que apunta al tree.
4. Opcionalmente se puede crear un tag que apunte al commit.

Esta estructura permite que Git sea rápido, eficiente y seguro.


## Diferencia entre git pull y git fetch

Ambos comandos se usan para traer cambios desde el repositorio remoto (por ejemplo, GitHub),
pero funcionan de manera diferente.

---

### 1️ git fetch

El comando:

git fetch

Descarga los cambios del repositorio remoto,
pero NO los mezcla automáticamente con tu trabajo actual.

- Actualiza la información del repositorio remoto.
- No modifica tus archivos.
- Es más seguro cuando quieres revisar los cambios antes de integrarlos.

Después de hacer fetch, puedes comparar cambios con:

git diff
git log

Para integrar los cambios manualmente puedes usar:

git merge

---

### 2️ git pull

El comando:

git pull

Hace dos cosas automáticamente:

1. Ejecuta git fetch
2. Ejecuta git merge

Es decir, descarga los cambios y los mezcla inmediatamente con tu rama actual.

Esto puede generar conflictos si hay cambios incompatibles.

---

## Diferencia principal

git fetch → Solo descarga cambios (no modifica tu trabajo).
git pull  → Descarga y mezcla automáticamente.

---

## ¿Cuándo usar cada uno?

Usa git fetch cuando:
- Quieres revisar cambios antes de integrarlos.
- Trabajas en equipo y quieres más control.

Usa git pull cuando:
- Quieres actualizar rápidamente tu proyecto.
- Sabes que no habrá conflictos.

---

## Resumen

git fetch = Descargar  
git pull  = Descargar + Integrar

## ¿Qué es un branch en Git y cómo gestiona Git los punteros a commits?

### ¿Qué es un branch?

Un branch (rama) en Git es simplemente un puntero móvil que apunta a un commit.

No es una copia del proyecto.
No duplica archivos.
Solo es una referencia a un punto específico en la historia del repositorio.

Por ejemplo:

main → A → B → C

Aquí, la rama "main" apunta al último commit (C).

Si creamos una nueva rama:

git branch nueva-rama

Git crea un nuevo puntero que apunta al mismo commit actual.

---

### ¿Cómo funcionan los punteros?

Internamente, cada rama es un archivo que contiene el hash del último commit al que apunta.

Cuando haces un nuevo commit:

1. Se crea un nuevo objeto commit.
2. El puntero de la rama actual se mueve automáticamente a ese nuevo commit.

Ejemplo:

Antes del commit:
main → A → B → C

Después de un nuevo commit:
main → A → B → C → D

El puntero se actualiza de C a D.

---

### ¿Qué es HEAD?

HEAD es un puntero especial que indica en qué rama estás trabajando actualmente.

Si estás en la rama main:

HEAD → main → último commit

Cuando cambias de rama con:

git checkout otra-rama

HEAD ahora apunta a esa nueva rama.

---

### ¿Por qué es eficiente el sistema de ramas?

Porque crear una rama no copia archivos.
Solo crea un nuevo puntero.

Esto hace que las ramas en Git sean:
- Rápidas
- Livianas
- Fáciles de crear y eliminar

---

## Resumen

- Un branch es un puntero a un commit.
- Cada commit apunta a su commit padre.
- HEAD indica la rama activa.
- Al hacer un commit, el puntero de la rama se mueve hacia adelante.

Gracias a este sistema de punteros, Git puede gestionar múltiples líneas de desarrollo de manera eficiente.


## ¿Cómo se realiza un merge en Git?

Un merge (fusión) se utiliza para integrar los cambios de una rama en otra.

### Pasos para hacer un merge

1️ Cambiar a la rama donde quieres integrar los cambios:

git checkout main

2️ Ejecutar el merge indicando la rama que quieres fusionar:

git merge nombre-rama

Ejemplo:

git merge desarrollo

Git intentará combinar automáticamente los cambios.

Si no hay conflictos, el merge se realiza de forma automática.

---

## ¿Qué conflictos pueden surgir?

Un conflicto ocurre cuando Git no puede decidir automáticamente qué cambios conservar.

Esto sucede cuando:

- Dos ramas modifican la misma línea de un archivo.
- Una rama elimina un archivo que otra modificó.
- Se cambian estructuras similares en el mismo archivo.

Ejemplo típico:

Rama A:
Hola mundo

Rama B:
Hola Git

Git no sabe cuál versión dejar.

---

## ¿Cómo se ve un conflicto?

Git marca el archivo con indicadores como estos:

<<<<<<< HEAD
Hola mundo
=======
Hola Git
>>>>>>> desarrollo

Esto significa:

- HEAD → contenido de la rama actual
- desarrollo → contenido de la rama que se intenta fusionar

---

## ¿Cómo se resuelven los conflictos?

1️ Abrir el archivo con conflicto.
2️ Editar manualmente el contenido.
3️ Eliminar los marcadores:
   <<<<<<<
   =======
   >>>>>>>
4️ Dejar solo la versión correcta.

Por ejemplo:

Hola mundo y Git

5️ Guardar el archivo.
6️ Agregar el archivo resuelto:

git add nombre_archivo

7️ Finalizar el merge:

git commit

---

## Tipos de merge

- Fast-forward: ocurre cuando no hay divergencia entre ramas.
- Merge con commit: ocurre cuando ambas ramas tienen cambios distintos.

---

## Resumen

- git merge combina ramas.
- Los conflictos ocurren cuando hay cambios incompatibles.
- Se resuelven editando manualmente los archivos.
- Después de resolverlos, se hace git add y git commit.

El manejo correcto de conflictos es fundamental cuando se trabaja en equipo.



## ¿Cómo funciona el área de Staging (git add) y qué pasa si se omite?

### ¿Qué es el Staging Area?

El Staging Area (también llamado Index) es el área intermedia entre el Working Directory y el Repository.

Funciona como una zona de preparación donde decides qué cambios incluir en el próximo commit.

El comando que envía archivos al Staging Area es:

git add nombre_archivo

O para agregar todos los cambios:

git add .

---

### ¿Cómo funciona internamente?

Cuando ejecutas git add:

1️ Git toma el estado actual del archivo.
2️ Lo guarda como un snapshot en el Staging Area.
3️ Ese snapshot será el que se incluya en el próximo commit.

Importante:
El commit no guarda lo que está en tu carpeta directamente.
Guarda lo que está en el Staging Area.

---

### Flujo completo

Working Directory → Staging Area → Repository

Modificar → git add → git commit

---

### Ejemplo práctico

1️ Modificas un archivo.
2️ Ejecutas:

git status

Aparece como "modified".

3️ Ejecutas:

git add archivo.txt

Ahora el archivo está preparado para el commit.

4️ Ejecutas:

git commit -m "Mensaje"

El cambio queda guardado en el historial.

---

### ¿Qué pasa si omites git add?

Si modificas un archivo y ejecutas directamente:

git commit -m "Mensaje"

Git NO incluirá los cambios que no estén en staging.

El commit no guardará nada nuevo.

Verás un mensaje como:

nothing to commit

Esto ocurre porque Git solo confirma lo que está en el Staging Area.

---

### ¿Por qué es importante el Staging Area?

Permite:

- Seleccionar cambios específicos.
- Hacer commits más organizados.
- Separar cambios grandes en partes pequeñas.
- Tener mayor control del historial.

---

## Resumen

- git add prepara los cambios.
- git commit guarda lo que está en staging.
- Si omites git add, los cambios no se incluirán en el commit.
- El Staging Area da control y organización al historial del proyecto.


## ¿Qué es el archivo .gitignore y cómo influye en el seguimiento de archivos?

### ¿Qué es .gitignore?

El archivo .gitignore es un archivo de configuración que le indica a Git qué archivos o carpetas NO debe rastrear (trackear).

Se coloca en la raíz del proyecto.

Su función es evitar que ciertos archivos innecesarios o sensibles se incluyan en el repositorio.

---

### ¿Qué tipo de archivos se suelen ignorar?

- Archivos temporales
- Archivos de configuración local
- Dependencias descargadas automáticamente
- Archivos compilados
- Credenciales o información sensible

Ejemplos comunes:

node_modules/
*.log
*.class
.env
dist/

---

### ¿Cómo funciona?

Cuando agregas reglas al archivo .gitignore:

Git deja de mostrar esos archivos como "Untracked" en git status.

Es decir:
- No los incluye en git add .
- No los sube al repositorio.
- No los guarda en commits.

---

### Ejemplo

Si tienes este .gitignore:

*.log

Cualquier archivo que termine en .log será ignorado.

Si tienes:

node_modules/

Toda esa carpeta será ignorada.

---

### Importante

.gitignore solo afecta archivos que NO han sido añadidos previamente.

Si un archivo ya fue agregado al repositorio y luego lo agregas al .gitignore,
Git seguirá rastreándolo.

Para dejar de rastrearlo debes usar:

git rm --cached nombre_archivo

---

### ¿Por qué es importante?

- Mantiene limpio el repositorio.
- Reduce el tamaño del proyecto.
- Evita subir información sensible.
- Hace el proyecto más profesional.

---

## Resumen

- .gitignore le dice a Git qué no debe rastrear.
- Evita subir archivos innecesarios o sensibles.
- No elimina archivos, solo evita que Git los controle.
- Mejora la organización del proyecto.

## Diferencia entre git commit --amend y un nuevo commit

En Git existen dos maneras de registrar cambios recientes:
1. Modificando el último commit (amend)
2. Creando un nuevo commit

---

### 1️ git commit --amend

El comando:

git commit --amend

Permite modificar el último commit realizado.

Se puede usar para:

- Corregir el mensaje del commit.
- Agregar archivos que olvidaste incluir.
- Hacer pequeños ajustes recientes.

Cuando usas --amend:
- No se crea un commit adicional.
- Se reemplaza el commit anterior.
- Se genera un nuevo hash para ese commit.

Ejemplo:

git add archivo.txt
git commit --amend -m "Mensaje corregido"

---

### 2️ Crear un nuevo commit

El comando normal:

git commit -m "Nuevo cambio"

Crea un nuevo punto en el historial.

Cada nuevo commit:
- Tiene su propio hash.
- Mantiene intactos los commits anteriores.
- Amplía la línea del tiempo del proyecto.

Ejemplo:

A → B → C → D

Cada letra representa un commit distinto.

---

## Diferencia principal

git commit --amend:
- Modifica el último commit.
- Reescribe el historial.
- No añade un nuevo punto en la línea de tiempo.

git commit normal:
- Crea un nuevo commit.
- Mantiene el historial completo.
- Es la forma estándar de guardar cambios.

---

## ¿Cuándo usar cada uno?

Usa --amend cuando:
- Acabas de hacer un commit.
- Olvidaste agregar un archivo.
- Necesitas corregir el mensaje.
- Aún no has hecho push al repositorio remoto.

Usa un nuevo commit cuando:
- Ya compartiste el commit con otros.
- Quieres mantener el historial claro.
- El cambio es significativo.

---

## Advertencia importante

Si ya hiciste git push y usas --amend,
estarás reescribiendo el historial remoto,
lo que puede causar conflictos en trabajo en equipo.

---

## Resumen

- --amend modifica el último commit.
- Un commit normal crea uno nuevo.
- --amend reescribe el historial.
- Crear un nuevo commit mantiene el historial intacto.



## ¿Cómo se utiliza git stash y en qué escenarios es útil?

### ¿Qué es git stash?

git stash es un comando que permite guardar temporalmente los cambios que aún no han sido confirmados (commits),
para poder cambiar de rama o realizar otra tarea sin perder el trabajo actual.

Es como poner tus cambios en "pausa".

---

## ¿Cómo funciona?

Cuando ejecutas:

git stash

Git:
1️ Guarda los cambios modificados y agregados.
2️ Limpia el Working Directory.
3️ Regresa el proyecto al último commit confirmado.

Tus cambios no se pierden, solo quedan almacenados en una pila (stack).

---

## Comandos principales

### Guardar cambios

git stash

O con mensaje:

git stash save "mensaje descriptivo"

---

### Ver los stashes guardados

git stash list

---

### Recuperar el último stash

git stash apply

Esto aplica los cambios, pero los mantiene en la lista.

---

### Recuperar y eliminar el stash

git stash pop

Aplica los cambios y elimina ese stash de la lista.

---

### Eliminar un stash específico

git stash drop stash@{0}

---

### Limpiar todos los stashes

git stash clear

---

## ¿En qué escenarios es útil?

🔹 Cuando necesitas cambiar de rama rápidamente.
🔹 Cuando debes hacer un pull urgente.
🔹 Cuando quieres probar algo sin hacer commit.
🔹 Cuando tu trabajo aún no está listo para confirmarse.

---

## Ejemplo práctico

Estás trabajando en una función nueva,
pero necesitas cambiar a la rama main para corregir un error urgente.

Pasos:

1️ Guardas cambios temporales:
git stash

2️ Cambias de rama:
git checkout main

3 Arreglas el error y haces commit.

4️ Regresas a tu rama:
git checkout mi-rama

5️ Recuperas tu trabajo:
git stash pop

---

## Resumen

- git stash guarda cambios temporalmente.
- Limpia el área de trabajo sin hacer commit.
- Permite cambiar de contexto rápidamente.
- Es ideal para trabajo interrumpido.

git stash es una herramienta muy útil cuando trabajas en múltiples tareas al mismo tiempo.


## ¿Qué mecanismos ofrece Git para deshacer cambios?

Git ofrece varios comandos para deshacer cambios dependiendo del nivel en el que se encuentren (working directory, staging o commits).

Los más importantes son:

- git reset
- git revert
- git checkout

---

## 1️ git reset

Se utiliza para mover el puntero de la rama actual a un commit anterior.

Puede afectar:
- El commit
- El staging area
- El working directory

### Tipos de reset

🔹 Reset suave (soft)

git reset --soft HEAD~1

- Deshace el último commit.
- Mantiene los cambios en staging.
- No elimina el trabajo realizado.

🔹 Reset mixto (default)

git reset HEAD~1

- Deshace el commit.
- Quita los archivos del staging.
- Mantiene los cambios en el working directory.

🔹 Reset duro (hard)

git reset --hard HEAD~1

- Elimina el commit.
- Elimina el staging.
- Elimina los cambios del working directory.

⚠️ El modo --hard borra cambios permanentemente.

---

## 2️ git revert

Crea un nuevo commit que deshace los cambios de un commit anterior.

Ejemplo:

git revert HEAD

Diferencia clave:
- No elimina el commit anterior.
- Mantiene el historial intacto.
- Es más seguro cuando ya hiciste push.

Es ideal para trabajo en equipo.

---

## 3️ git checkout

Se usa para:

- Cambiar de rama.
- Restaurar archivos.
- Volver temporalmente a un commit anterior.

Ejemplo para restaurar un archivo:

git checkout -- archivo.txt

Esto descarta cambios no confirmados en ese archivo.

También se puede usar para ir a un commit específico:

git checkout hash_del_commit

Esto coloca el repositorio en estado "detached HEAD".

---

## Diferencias principales

git reset:
- Reescribe el historial.
- Puede eliminar commits.
- Útil antes de hacer push.

git revert:
- Crea un commit nuevo que revierte cambios.
- No reescribe historial.
- Seguro después de hacer push.

git checkout:
- Cambia de rama o restaura archivos.
- No elimina historial.

---

## Resumen

- git reset modifica el historial.
- git revert crea un nuevo commit para deshacer cambios.
- git checkout permite cambiar de contexto o restaurar archivos.
- Elegir el comando correcto depende de si el cambio ya fue compartido o no.

# Configuración de Remotos en Git (origin y upstream)

## ¿Qué es un remoto en Git?

Un remoto es una versión del repositorio que está alojada en un servidor (como GitHub o GitLab) y que se puede conectar con tu repositorio local.

---

# 🔹 origin

`origin` es el nombre que Git asigna por defecto al repositorio remoto cuando haces un clone.

Ejemplo:

```bash


# upstream se usa cuando trabajas con un fork.

Escenario típico:

Existe un repositorio original.

Tú haces un fork.

Clonas tu fork a tu computador.

Cómo configurar upstream

Después de clonar tu fork:
git remote add upstream https://github.com/autor/proyecto-original.git


# Inspección del Historial de Commits en Git

Git ofrece varios comandos para revisar cambios, commits y diferencias entre versiones del código.

---

# 🔹 git log

Muestra el historial de commits del repositorio.

## Uso básico

```bash
git log.