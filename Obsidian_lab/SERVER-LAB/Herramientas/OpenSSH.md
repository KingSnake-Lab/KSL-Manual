Requisitos: 
Link ->[[Comandos básicos de Linux Ubuntu Server]]

### Instalar el servicio SSH de Ubuntu

Con el comando “install” de Ubuntu para el servicio SSH, ahora puedes instalar OpenSSH en la herramienta de línea de comandos iniciada. El comando es el siguiente:

```mixed
sudo apt install openssh-server
```

Introduce tu contraseña y confirma con la tecla “Enter” para iniciar la instalación del servicio SSH de Ubuntu.

### Comprobar el estado y activar el servidor SSH de Ubuntu

Después de finalizar el proceso de instalación, puedes comprobar con el siguiente comando si el Daemon SSH ya funciona de la manera prevista:

```
sudo systemctl status ssh
```

Si se está ejecutando el servicio SSH de Ubuntu, verás en la respuesta de este comando la entrada “**active (running)**”. Dado que SSH también debería estar disponible al reiniciar el sistema, debería aparecer la entrada “**vendor preset: enabled**” en la línea “**Loaded**”.

Si el **servidor SSH sigue inactivo** y al reiniciarlo no está activado el inicio automático, puedes cambiarlo introduciendo otros dos comandos:

```
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl restart ssh
```

### Abrir el puerto SSH

Para que puedas conectarte a tu sistema Ubuntu desde cualquier lugar a través de SSH, debes liberar también el **puerto del protocolo de red** (por defecto: puerto TCP 22). Solo así puedes **conectarte remotamente** con clientes SSH como PuTTy.

UFW es el programa de configuración de Ubuntu para su propio firewall. Configura una regla para la comunicación SSH con este programa, de modo que el puerto esté abierto para los datos entrantes y salientes:

```
sudo ufw allow ssh
```

### Configurar el servidor SSH de Ubuntu

La configuración básica de OpenSSH es la más idónea para establecer una conexión remota con tu sistema Ubuntu, pero también puedes **adaptar la configuración estándar** si, por ejemplo, prefieres comunicarte con otro puerto, elegir otra versión de protocolo de Internet o desactivar el TCP forwarding.

El archivo de configuración central del paquete de Ubuntu SSH se llama **_sshd_config_**. Para cualquier tipo de cambio, abre este archivo con el editor de texto (aquí: nano) que desees de la siguiente manera:

```
sudo nano /etc/ssh/sshd_config
```

Ajusta el **contenido del Config** a tus necesidades y guarda los cambios antes de cerrar el archivo. Luego, **vuelve a abrir OpenSSH** y, como podrás ver, se habrán aplicado los cambios efectuados.