# MySQL Server

## Índice<a name="indice"></a>
 1. [Creación Bases de Datos](#bd)

##  Creación Bases de Datos<a name="bd"></a>
Para una mayor facilidad y ahorro de tiempo vamos a la hora de crear las dos bases de datos vamos a importar un archivo ".sql".

Primero vamos a crear el archivo mediante el editor "nano", uno de los más recomendados. Para ello precisamos instalar, en el caso de que no lo tengas instalado, el editor mediante el siguiente comando en la consola:

    sudo apt-get install nano

Con el siguiente comando creamos un nuevo archivo e inmediatamente se nos abrirá el editor para empezar a escribir el código:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/comandoNano.PNG)

Copiamos y pegamos el código que ya tenemos desarrollado y guardamos el archivo:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/scriptNano.PNG)

Introducimos el comando necesario para importar el archivo:

    sudo mysql -u root -p < fichero.sql

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/comandoBD.PNG)

Ahora iniciamos nuestro Servidor Local de MySQL:

    sudo mysql
  
Queremos usar nuestra nueva base de datos:

    use nombre_BD

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/comandoUse.PNG)

Ahora ya podemos modificar o hacer lo que queramos en esa base de datos.

Vamos a probar algunas cosas, por ejemplo vamos a comprobar que todas las tablas sean las correctas:

    SHOW TABLES;

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/comandoTablaIn.PNG)

Ahora que comprobamos que están todas las tablas correctas, vamos a observar que atributos tienen:

    desc nombre_Tabla;

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/tablaIn1.PNG)

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/comandoIn2.PNG)

Para crear la base de datos del segundo ejercicio ejecutamos los mismos pasos:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/comandosNaves.PNG)

Nos situamos en la nueva BD:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/UseNaves.PNG)

Mostramos las tablas:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/tablasNaves.PNG)

Y su contenido:

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/NavesTabla1.PNG)

![imagen](https://raw.githubusercontent.com/Fonsi13/Sublenguajes-SQL/master/MySQL%20Server/NavesTabla2.PNG)

[Volver al índice](#indice)
