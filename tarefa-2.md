# MySQL Server

## Índice<a name="indice"></a>
 1. [Instalación del Servidor Local](#install)
 
## Instalación del Servidor Local <a name="install"></a>
Vamos a realizar la instalación local de un SGBD MySQL en Ubuntu 18.04.

Únicamente la última versión de MySQL se incluye en el repositorio de paquete APT de forma predeterminada en Ubuntu 18.04, por lo que hay que actualizar la lista de paquetes disponible mediante comandos en la  Terminal:

    $ sudo apt update

Una vez terminado el proceso, mediante otro comando instalamos el paquete:

    $ sudo apt install mysql-server

Una vez finalizada la instalación, si no ha habido ningún fallo, el servidor MySQL ya estaría instalado y funcionando.

Para comprobar esto último podemos ejecutar el siguiente comando:

    systemctl status mysql.service
    
Y darnos el siguiente resultado:

![alttext](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/Prueba_funcionamiento.PNG)

Una vez que sabemos que funciona a la perfección y ya está listo para usar podemos probar algunos comandos.

Lo primero es entrar en la línea de comandos del servidor:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/Linea_de_comandos.PNG)

Aquí dejo un ejemplo de una respuesta a un comando:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/Ejemplo_comando.PNG)

Si ya hemos acabado de usar el servidor, para salir de él es tan sencillo como poner solo exit:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/Exit_comando.PNG)

[Volver al índice](#indice)
