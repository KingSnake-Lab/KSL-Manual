El token de GitHub sirve para autenticar tu cuenta en algunas aplicaciones, pero también te sirve como contraseña para hacer un push en Ubuntu.

Primero debes tener una cuenta de GitHub.

Después te va a ir a Settings
![[Pasted image 20230227124413.png]]

Elegimos la opción de Developer settings
![[Pasted image 20230227124524.png]]

Seleccionamos en Personal access tokens -> tokens
![[Pasted image 20230227124557.png]]

Generamos el token
![[Pasted image 20230227124653.png]]

Lo vamos a copiar y a guardar en un documento txt ya que será nuestro token

Podemos guardar el token en el servidor, solo necesitamos guardar una variable de entorno:

La sintaxis básica de este comando se vería así:

export VAR="value"

Vamos a desglosarlo:

-   **export**: el comando utilizado para crear la variable.
-   **VAR**: el nombre de la variable.
-   **=** indica que la siguiente sección es el valor.
-   **«value»**: el valor real, aqui iría el token que copiaste


Para leerla podemos accedes desde una terminal en ubuntu con:

echo $VARr