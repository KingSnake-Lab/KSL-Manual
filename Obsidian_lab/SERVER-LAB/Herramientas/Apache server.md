#Apache
¿Que es?
El servidor HTTP Apache es el más usado del mundo. Ofrece muchas características potentes, entre las que se incluyen módulos que se cargan de forma dinámica, una sólida compatibilidad con medios y amplia integración con otras herramientas de software populares.

Requerimientos:
Se requiere manejar algunos comandos básicos de linux. Puedes revisar el documento que contiene esa documentación. Link -> [[Comandos básicos de Linux Ubuntu Server]]

## Instalar apache en Ubuntu 
#instalacionApache
Comencemos actualizando el índice de paquetes locales para que reflejen los últimos cambios anteriores:

``` 
sudo apt update
```

A continuación, instale el paquete apache2:

```
sudo apt install apache2
```



## Paso 2: Ajustar el firewall

Antes de probar Apache, es necesario modificar los ajustes de firewall para permitir el acceso externo a los puertos web predeterminados. Suponiendo que siguió las instrucciones de los requisitos previos, debería tener un firewall UFW configurado para que restrinja el acceso a su servidor.

Durante la instalación, Apache se registra con UFW para proporcionar algunos perfiles de aplicación que pueden utilizarse para habilitar o deshabilitar el acceso a Apache a través del firewall.

Enumere los perfiles de aplicación `ufw` escribiendo lo siguiente:
```
sudo ufw app list
```

Obtendrá una lista de los perfiles de aplicación:

```
OutputAvailable applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```

Como lo indica el resultado, hay tres perfiles disponibles para Apache:

-   **Apache**: este perfil abre solo el puerto 80 (tráfico web normal no cifrado)
-   **Apache Full**: este perfil abre el puerto 80 (tráfico web normal no cifrado) y el puerto 443 (tráfico TLS/SSL cifrado)
-   **Apache Secure**: este perfil abre solo el puerto 443 (tráfico TLS/SSL cifrado)

Se recomienda habilitar el perfil más restrictivo, que de todos modos permitirá el tráfico que configuró. Debido a que en esta guía aún no configuramos SSL para nuestro servidor, solo deberemos permitir el tráfico en el puerto 80:

```
sudo ufw allow 'Apache'

```

Puede verificar el cambio escribiendo lo siguiente:

```
sudo ufw status

```

El resultado proporcionará una lista del tráfico de HTTP que se permite:

```
OutputStatus: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Apache                     ALLOW       Anywhere                
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Apache (v6)                ALLOW       Anywhere (v6)
```

Como lo indica el resultado, el perfil se activó para permitir el acceso al servidor web Apache.


## Paso 3: Comprobar su servidor web

Al final del proceso de instalación, Ubuntu 20.04 inicia Apache. El servidor web ya debería estar activo.

Realice una verificación con el sistema init `systemd` para saber si se encuentra en ejecución el servicio escribiendo lo siguiente:

```
sudo systemctl status apache2

```

```
Output● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2020-04-23 22:36:30 UTC; 20h ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 29435 (apache2)
      Tasks: 55 (limit: 1137)
     Memory: 8.0M
     CGroup: /system.slice/apache2.service
             ├─29435 /usr/sbin/apache2 -k start
             ├─29437 /usr/sbin/apache2 -k start
             └─29438 /usr/sbin/apache2 -k start

```

Como lo confirma este resultado, el servicio se inició correctamente. Sin embargo, la mejor forma de comprobarlo es solicitar una página de Apache.

## ¿Como acceder al servidor?

Puede acceder a la página de destino predeterminada de Apache para confirmar que el software funcione correctamente mediante su dirección IP: Si no conoce la dirección IP de su servidor, puede obtenerla de varias formas desde la línea de comandos.

Intente escribir esto en la línea de comandos de su servidor:

```
hostname -I

```

Obtendrá algunas direcciones separadas por espacios. Puede probar cada uno en el navegador web para determinar si funcionan.

Otra opción es utilizar la herramienta Icanhazip, que debería proporcionarle su dirección IP pública como aparece en otra ubicación en Internet:

```
curl -4 icanhazip.com

```

Cuando tenga la dirección IP de su servidor, introdúzcala en la barra de direcciones de su navegador:

```
http://your_server_ip
```

Debería ver la página web predeterminada de Apache en Ubuntu 20.04:

![Página predeterminada de Apache](https://assets.digitalocean.com/articles/how-to-install-lamp-ubuntu-16/small_apache_default.png)

Esta página indica que Apache funciona correctamente. También incluye información básica sobre archivos y ubicaciones de directorios importantes de Apache.


## Paso 4: Administrar el proceso de Apache

Ahora que el servidor web está listo y en funcionamiento, repasemos algunos comandos de administración básicos con `systemctl`.

Para detener su servidor web, escriba lo siguiente:

```
sudo systemctl stop apache2

```

Para iniciar el servidor web cuando no esté activo, escriba lo siguiente:

```
sudo systemctl start apache2

```

Para detener y luego iniciar el servicio de nuevo, escriba lo siguiente:

```
sudo systemctl restart apache2

```

Si solo realiza cambios de configuración, Apache a menudo puede recargarse sin cerrar conexiones. Para hacerlo, utilice este comando:

```
sudo systemctl reload apache2

```

Por defecto, Apache está configurado para iniciarse automáticamente cuando el servidor lo hace. Si no es lo que quiere, deshabilite este comportamiento escribiendo lo siguiente:

```
sudo systemctl disable apache2

```

Para volver a habilitar el servicio de modo que se cargue en el inicio, escriba lo siguiente:

```
sudo systemctl enable apache2

```

Ahora, Apache debería iniciarse de forma automática cuando el servidor lo haga de nuevo.

## Paso 5: Configurar hosts virtuales (recomendado)

Al emplear el servidor web Apache, puede utilizar _hosts virtuales_ (similares a bloques de servidor de Nginx) para encapsular detalles de configuración y alojar más de un dominio desde un único servidor. Configuraremos un dominio llamado **your_domain**, pero debería **cambiarlo por su propio nombre de dominio**. Si va a configurar un nombre de dominio con DigitalOcean, consulte nuestra [Documentación de red](https://www.digitalocean.com/docs/networking/dns/).

Ubuntu 20.04 tiene habilitado un bloque de servidor por defecto, que está configurado para proporcionar documentos del directorio `/var/www/html`. Si bien esto funciona bien para un solo sitio, puede ser difícil de manejar si aloja varios. En vez de modificar `/var/www/html`, vamos a crear una estructura de directorios dentro de `/var/www` para un sitio **your_domain** y dejaremos `/var/www/html` como directorio predeterminado que se suministrará si una solicitud de cliente no coincide con otros sitios.

Cree el directorio para **your_domain** de la siguiente manera:

```
sudo mkdir /var/www/your_domain

```

A continuación, asigne la propiedad del directorio con la variable de entorno `$USER`:

```
sudo chown -R $USER:$USER /var/www/your_domain

```

Los permisos de los roots web deberían ser correctos si no modificó el valor umask, que establece permisos de archivos predeterminados. Para asegurarse de que sus permisos sean correctos y permitir al propietario leer, escribir y ejecutar los archivos, y a la vez conceder solo permisos de lectura y ejecución a los grupos y terceros, puede ingresar el siguiente comando:

```
sudo chmod -R 755 /var/www/your_domain

```

A continuación, cree una página de ejemplo `index.html` utilizando `nano` o su editor favorito:

```
sudo nano /var/www/your_domain/index.html

```

Dentro de ella, agregue el siguiente ejemplo de HTML:

/var/www/your_domain/index.html

```
<html>
    <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
```

Guarde y cierre el archivo cuando termine.

Para que Apache proporcione este contenido, es necesario crear un archivo de host virtual con las directivas correctas. En lugar de modificar el archivo de configuración predeterminado situado en `/etc/apache2/sites-available/000-default.conf` directamente, vamos a crear uno nuevo en `/etc/apache2/sites-available/your_domain.conf`:

```
sudo nano /etc/apache2/sites-available/your_domain.conf

```

Péguelo en el siguiente bloque de configuración, similar al predeterminado, pero actualizado para nuestro nuevo directorio y nombre de dominio:

/etc/apache2/sites-available/your_domain.conf

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Tenga en cuenta que cambiamos `DocumentRoot` por nuestro nuevo directorio y `ServerAdmin` por un correo electrónico al que pueda acceder el administrador del sitio **your_domain**. También agregamos dos directivas: `ServerName`, que establece el dominio de base que debería coincidir para esta definición de host virtual, y `ServerAlias`, que define más nombres que deberían coincidir como si fuesen el nombre de base.

Guarde y cierre el archivo cuando termine.

Habilitaremos el archivo con la herramienta `a2ensite`:

```
sudo a2ensite your_domain.conf

```

Deshabilite el sitio predeterminado definido en `000-default.conf`:

```
sudo a2dissite 000-default.conf

```

A continuación, realizaremos una prueba para ver que no haya errores de configuración:

```
sudo apache2ctl configtest

```

Debería obtener el siguiente resultado:

```
OutputSyntax OK
```

Reinicie Apache para implementar sus cambios:

```
sudo systemctl restart apache2

```

Con esto, Apache debería ser el servidor de su nombre de dominio. Puede probarlo visitando `http://your_domain`, donde debería ver algo como esto:

![Ejemplo de host virtual de Apache](https://assets.digitalocean.com/articles/apache_virtual_hosts_ubuntu/vhost_your_domain.png)



## Paso 6: Familiarizarse con archivos y direcciones importantes de Apache

Ahora que sabe administrar el propio servicio de Apache, debe tomarse unos minutos para familiarizarse con algunos directorios y archivos importantes.

### Contenido
#directorio #localhost
-   `/var/www/html`: el contenido web real, que por defecto solo consta de la página predeterminada de Apache que vio antes, se proporciona desde el directorio `/var/www/html`. Esto se puede cambiar modificando los archivos de configuración de Apache.

### Configuración del servidor
#config #etc #apacheConf
-   `/etc/apache2`: el directorio de configuración de Apache. En él se encuentran todos los archivos de configuración de Apache.
-   `/etc/apache2/apache2.conf`: el archivo principal de configuración de Apache. Esto se puede modificar para realizar cambios en la configuración general de Apache. Este archivo administra la carga de muchos de los demás archivos del directorio de configuración.
-   `/etc/apache2/ports.conf`: este archivo especifica los puertos en los que Apache escuchará. Por defecto, Apache escucha en el puerto 80. De forma adicional, lo hace en el 443 cuando se habilita un módulo que proporciona capacidades SSL.
-   `/etc/apache2/sites-available/`: el directorio en el que se pueden almacenar hosts por sitio. Apache no utilizará los archivos de configuración de este directorio a menos que estén vinculados al directorio `sites-enabled`. Normalmente, toda la configuración de bloques de servidor se realiza en este directorio y luego se habilita al vincularse al otro directorio con el comando `a2ensite`.
-   `/etc/apache2/sites-enabled/`: el directorio donde se almacenan hosts virtuales por sitio habilitados. Normalmente, se crean vinculando los archivos de configuración del directorio `sites-available` con `a2ensite`. Apache lee los archivos de configuración y los enlaces de este directorio cuando se inicia o se vuelve a cargar para compilar una configuración completa.
-   `/etc/apache2/conf-available/` y `/etc/apache2/conf-enabled/`: estos directorios tienen la misma relación que los directorios `sites-available` y `sites-enabled`, pero se utilizan para almacenar fragmentos de configuración que no pertenecen a un host virtual. Los archivos del directorio `conf-available` pueden habilitarse con el comando `a2enconf` y deshabilitarse con el comando `a2disconf`.
-   `/etc/apache2/mods-available/` y `/etc/apache2/mods-enabled/`: estos directorios contienen los módulos disponibles y habilitados, respectivamente. Los archivos que terminan en `.load` contienen fragmentos para cargar módulos específicos, mientras que los archivos que terminan en `.conf` contienen la configuración para esos módulos. Los módulos pueden habilitarse y deshabilitarse con los comandos `a2enmod` y `a2dismod`.


###  Registros del servidor 
#registros #logs
-   `/var/log/apache2/access.log`: por defecto, cada solicitud enviada a su servidor web se asienta en este archivo de registro a menos que Apache esté configurado para no hacerlo.
-   `/var/log/apache2/error.log`: por defecto, todos los errores se registran en este archivo. La directiva `LogLevel` de la configuración de Apache especifica el nivel de detalle de los registros de error.