[[Comandos básicos de Linux Ubuntu Server]]

**NodeSource es el repositorio de nodos de nivel empresarial propio de la compañía que mantienen y contiene las últimas versiones de NodeJS**. Desde NodeSource vamos a poder instalar una versión específica de NodeJS.

Para instalar NodeJS desde NodeSource, simplemente habrá que ejecutar alguno de los siguientes comandos para agregar la versión específica que nos interese. Para hacerlo **tendremos que disponer de curl instalado**. Si no cuentas con esta herramienta todavía, puedes instalarla con el comando:

```
sudo apt install curl
```


```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

Deberemos instalar Node
```
sudo apt install nodejs
```

Podemos comprobar la version de node con
```
node --version
```

```
npm --version
```

Instalamos firebase CLI

```
npm install -g firebase-tools
```


### Accede a Firebase CLI y pruébala

Debes autenticarte después de instalar la CLI. Para confirmar la autenticación, puedes enumerar tus proyectos de Firebase.

1.  Accede a Firebase con tu Cuenta de Google ejecutando el siguiente comando:

```
firebase login
```

Este comando conecta tu máquina local a Firebase y te otorga acceso a los proyectos de Firebase.

Enumera tus proyectos de Firebase para probar que se instaló correctamente la CLI y que accediste a tu cuenta. Ejecuta el siguiente comando:

```
firebase projects:list
```


La lista que se muestra debe ser la misma que los proyectos de Firebase enumerados en [Firebase console](https://console.firebase.google.com/?hl=es-419).

## Inicializa un proyecto de Firebase

Muchas de las tareas comunes que se realizan con la CLI, como implementarla en un proyecto de Firebase, requieren un **directorio del proyecto**. Este directorio se establece con el comando `firebase init`. Por lo general, el directorio del proyecto es el mismo que la raíz del control de fuente y, después de ejecutar `firebase init`, este contendrá un archivo de configuración [`firebase.json`](https://firebase.google.com/docs/cli?hl=es-419#the_firebasejson_file).

Para inicializar un proyecto de Firebase nuevo, ejecuta el siguiente comando desde el directorio de tu app:


```
firebase init
```


**Nota:** El comando `firebase init` no crea un directorio nuevo. Si estás iniciando una app nueva, primero debes crear un directorio y, luego, ejecutar `firebase init` desde ese directorio.

El comando `firebase init` te guía paso a paso en la configuración del directorio del proyecto y algunos productos de Firebase.

Aprende más en:
https://firebase.google.com/docs/cli?hl=es-419#linux
