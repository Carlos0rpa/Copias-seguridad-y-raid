# UD7: Política de Backup y sistemas RAID
## 1. Estructura de ficheros y política de **Backup** 
### Diseño política
Preparamos la siguiente estructura de ficheros que usaremos para hacer pruebas con el programa **Paragon Backup Community Edition**  
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt   
    - [f] ficheroLog2.txt
- [f] fichero1.pdf
- [f] fichero1.txt
- [f] fichero2.txt
- [f] otroFichero1.jpg
- [f] otroFichero2.jpg
`````
Una vez establecido el sistema de ficheros que usaremos pasamos a diseñar la política de copias de seguridad **diarias**. Hemos decido usar un ciclo de backups de **1 full + 6 increments**. Se harán todos los días a las 5:30 
hora española empezando el día 26/05/2024. Hemos decidido usar este sitema de copias de seguridad ya que así gestionamos el espacio de la mejor manera, todos los domingos se hará una copia de seguridad completa mientras que el resto de días de la semana
crearán una copia de seguridad incremental, lo que quiere decir que solo harán copia de lo que haya sido modificado desde la última copia de seguridad incremental o, en su defecto, completa.
  
Foto de como queda en el programa:  
  
![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/backup.png)

### Pruebas

Para poner a prueba nuestra política de copias de seguridad haremos lo siguiente:  
1. Creamos una copia de seguridad completa simulando que es Domingo
2. El lunes modificamos erroneamente fichero1.txt fichero2.txt y creamos fichero3.txt y fichero4.txt
3. Martes creamos fichero5.txt
4. Miercoles borramos erróneamente fichero1.pdf y creamos otroFichero3.jpg y otroFicheroLog4.jpg
5. Jueves borramos erróneamente otroFichero1.jpg y creamos ficheroLog3.txt y ficheroLog4.txt
  
Cuando llegamos el viernes para solucionar los problemas nos encontramos con las siguientes copias de seguridad hechas:
![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/8.png)

En este punto contamos con la siguiente estructura de ficheros:
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt   
    - [f] ficheroLog2.txt
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.txt
- [f] fichero2.txt
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero2.jpg
- [f] otroFichero3.jpg
- [f] otroFichero4.jpg
````

Para restaurar esto haremos lo siguiente:  

1. Al no haber hecho cambios aún el viernes no tendremos que restaurar la copia de seguridad de jueves ya que es la misma que la que tenemos. Restauramos la copia de seguridad del miercoles. En este proceso
recuperamos el fichero otroFichero1.jpg que eliminamos por error el jueves.
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt
    - [f] ficheroLog2.txt    
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.txt [ERROR]
- [f] fichero2.txt [ERROR]
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero1.jpg
- [f] otroFichero2.jpg
- [f] otroFichero3.jpg
- [f] otroFichero4.jpg
````
2. La restauración del martes es redundante ya que, el fichero5.txt lo tenemos ya y el fichero1.pdf lo podemos recuperar con la copia de seguridad del lunes. Restauramos la copia del lunes y recuperaremos el fichero1.pdf
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt
    - [f] ficheroLog2.txt    
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.pdf
- [f] fichero1.txt [ERROR]
- [f] fichero2.txt [ERROR]
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero1.jpg
- [f] otroFichero2.jpg
- [f] otroFichero3.jpg
- [f] otroFichero4.jpg
````
3. Por último restauramos la copia de seguridad completa para volver a tener los archivos fichero1.txt y fichero2.txt sin errores.
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt
    - [f] ficheroLog2.txt  
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.pdf
- [f] fichero1.txt
- [f] fichero2.txt
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero1.jpg
- [f] otroFichero2.jpg
- [f] otroFichero3.jpg
- [f] otroFichero4.jpg
````   
## Sistema RAID Windows
