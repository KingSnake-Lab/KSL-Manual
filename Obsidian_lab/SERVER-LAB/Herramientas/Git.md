Hoy en día, Git es, con diferencia, el sistema de control de versiones moderno más utilizado del mundo. Git es un proyecto de código abierto maduro y con un mantenimiento activo que desarrolló originalmente Linus Torvalds, el famoso creador del kernel del sistema operativo Linux, en 2005. Un asombroso número de proyectos de software dependen de Git para el control de versiones, incluidos proyectos comerciales y de código abierto. Los desarrolladores que han trabajado con Git cuentan con una buena representación en la base de talentos disponibles para el desarrollo de software, y este sistema funciona a la perfección en una amplia variedad de sistemas operativos e IDE (entornos de desarrollo integrados).


## Configuración inicial

### Tu Identidad

Lo primero que deberás hacer cuando instales Git es establecer tu nombre de usuario y dirección de correo electrónico. Esto es importante porque los "commits" de Git usan esta información, y es introducida de manera inmutable en los commits que envías:

```console
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

## Crear un repositorio
En comparación con SVN, el comando `git init` permite crear de manera increíblemente sencilla nuevos proyectos con control de versiones. Con Git, no es necesario que crees un repositorio, importes los archivos ni extraigas una copia de trabajo. Además, Git no precisa de la existencia previa de ningún servidor o privilegios de administrador. Basta con que utilices el comando cd en el subdirectorio de tu proyecto y ejecutes `git init` para que tengas un repositorio de Git totalmente funcional

```
git init
```


## Status
El comando de git status nos da toda la información necesaria sobre la rama actual.

```
git status
```
Podemos encontrar información como:

-   Si la rama actual está actualizada
-   Si hay algo para confirmar, enviar o recibir (pull).
-   Si hay archivos en preparación (staged), sin preparación(unstaged) o que no están recibiendo seguimiento (untracked)
-   Si hay archivos creados, modificados o eliminados

## Agregar cambios al repositorio

Cuando creamos, modificamos o eliminamos un archivo, estos cambios suceden en local y no se incluirán en el siguiente commit (a menos que cambiemos la configuración).

Necesitamos usar el comando git add para incluir los cambios del o de los archivos en tu siguiente commit.

**Añadir un único archivo:**

```
git add <archivo>
```

**Añadir todo de una vez:**

```
git add -A
```


Este sea quizás el comando más utilizado de Git. Una vez que se llega a cierto punto en el desarrollo, queremos guardar nuestros cambios (quizás después de una tarea o asunto específico).  

Git commit es como establecer un punto de control en el proceso de desarrollo al cual puedes volver más tarde si es necesario.

También necesitamos escribir un mensaje corto para explicar qué hemos desarrollado o modificado en el código fuente.

```
git commit -m "mensaje de confirmación"
```


## Subir un repositorio

Puedes agregar el origen a donde se subirá tu repositorio:
```
git remote add <nombre del origen> <url>
```

Después de haber confirmado tus cambios, el siguiente paso que quieres dar es enviar tus cambios al servidor remoto. Git push envía tus commits al repositorio remoto.

```
git push -u origin <nombre-de-tu-rama>
```

La estructura es la siguiente:
```
git push -u <origen> <nombre-de-tu-rama>
```


Posteriormente te pedirá tú usuario y tu token como contraseña.
Link: [[Obtención de Token con GitHub]]


## Clonar un repositorio

Git clone es un comando para descargarte el código fuente existente desde un repositorio remoto (como Github, por ejemplo). En otras palabras, Git clone básicamente realiza una copia idéntica de la última versión de un proyecto en un repositorio y la guarda en tu ordenador.

Hay un par de formas de descargar el código fuente, pero principalmente yo prefiero **clonar de la forma con https**:

```
git clone <https://link-con-nombre-del-repositorio>
```

## Ramas

Crear una rama
```
git branch <nombre-de-la-ram
```

Ver las ramas
```
git branch
```

```
git branch --list
```

Borrar una rama
```
git branch -d <nombre-de-la-rama>
```

Cambiar de rama
```
git switch <nombre-de-la-ram
```

Cuando ya hayas completado el desarrollo de tu proyecto en tu rama y todo funcione correctamente, el último paso es fusionar la rama con su rama padre (dev o master). Esto se hace con el comando `git merge`.

Git merge básicamente integra las características de tu rama con todos los commits realizados a las ramas dev (o master).  Es importante que recuerdes que tienes que estar en esa rama específica que quieres fusionar  con tu rama de características.

Por ejemplo, cuando quieres fusionar tu rama de características en la rama dev:

**Primero, debes cambiarte a la rama dev:**

```
git checkout dev
```

**Antes de fusionar, debes actualizar tu rama dev local:**

```
git fetch
```

**Por último, puedes fusionar tu rama de características en la rama dev:**

```
git merge <nombre-de-la-rama>
```

## Actualizar repositorios remotos

El comando ****git pull**** se utiliza para recibir actualizaciones del repositorio remoto. Este comando es una combinación del ****git fetch**** y del ****git merge**** lo cual significa que cundo usemos el git pull recogeremos actualizaciones del repositorio remoto (git fetch) e inmediatamente aplicamos estos últimos cambios en local (git merge).

```
git pull <nombre-remoto>
```