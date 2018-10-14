Los pasos que se deben seguir para realizar la instalación del entorno de prácticas y proyectos son iguales para los sistemas operativos Linux o Solaris, por lo que se describirá a continuación la instalación bajo Linux:

* Entre en el computador Linux como administrador (root).
* Copie el fichero 88k Linux Static.tar.gz que ha obtenido en un directorio del disco duro del sistema Linux (p.e. /tmp).

Sitúese en el directorio donde ha copiado el fichero 88k Linux Static.tar.gz y descomprímalo:

		cd /tmp
		tar zxvf 88k_Linux_Static.tar.gz
Este comando habrá generado el directorio EM88110, que contendrá todos los ficheros de la distribución.
Cambie el directorio de trabajo, situándose en el directorio que acaba de crear:
			
			cd EM88110

Si teclea el comando ls -l obtendrá un listado con todos los ficheros que componen la distribución. Entre ellos se encuentra el fichero INSTALL. Para instalar el paquete
ejecute:
			
			sh INSTALL

El comando de instalación le preguntará por dos directorios donde ubicar la instalación:

Introduzca el directorio donde se va a instalar el emulador
(por defecto /usr/local/88k)
En este directorio se copiarán los ficheros que componen la herramienta. Debe introducir
un camino completo dentro del árbol de directorios, o simplemente pulsar la tecla enter para aceptar el directorio por defecto. A continuación aparecerá el
siguiente mensaje:

El directorio donde se van a instalar los ejecutables debe formar parte de la variable PATH. Introduzca el directorio donde se van a instalar los ejecutables (por defecto /usr/bin) En este caso debe especificar el directorio donde se van a buscar los ejecutables de la
instalación. 

Este directorio debe estar referenciado en la variable de entorno PATH.
Al igual que en el caso anterior si pulsa enter se acepta la opción por defecto. A continuación aparecerá el siguiente mensaje:

* El directorio de instalacion es /usr/local/88k
* El directorio de los ejecutables es /usr/bin

Si los parametros son correctos pulse enter
Si pulsa enter se continúa con la instalación y si los dos directorios que ha especificado son correctos el programa se habrá instalado correctamente. Si, por el contrario, desea cambiar alguno de los parámetros, pulse CTRL C y se cancelará la instalación.

Si la instalación se ha efectuado correctamente, salga de la cuenta root, vuelva a entrar como un usuario no privilegiado y ejecute los comandos de la práctica o el proyecto. Deben funcionar correctamente.