# UD7: Política de Backup y sistemas RAID
## 1. Estructura de ficheros y política de **Backup** 
### Diseño política
Preparamos la siguiente estructura de ficheros que usaremos para hacer pruebas con el programa **Paragon Backup Community Edition**  
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt   
    - [f] ficheroLog2.txt
- [f] fichero1.txt
- [f] fichero2.txt
- [f] fichero1.pdf
- [f] otroFichero1.jpg
- [f] otroFichero2.jpg
````
Una vez establecido el sistema de ficheros que usaremos pasamos a diseñar la política de copias de seguridad **diarias**. Hemos decido usar un ciclo de backups de **1 full + 6 increments**. Se harán todos los días a las 5:30 
hora española empezando el día 26/05/2024. Hemos decidido usar este sitema de copias de seguridad ya que así gestionamos el espacio de la mejor manera, todos los domingos se hará una copia de seguridad completa mientras que el resto de días de la semana
crearán una copia de seguridad incremental, lo que quiere decir que solo harán copia de lo que haya sido modificado desde la última copia de seguridad incremental o, en su defecto, completa.
  
Foto de como queda en el programa:  
  
![image](https://github.com/regueiroangel/UD7/assets/111371837/b6857662-12c3-447f-a4f6-cb0178e8cc62)

### Pruebas

Para poner a prueba nuestra política de copias de seguridad haremos lo siguiente:  
1. Creamos una copia de seguridad completa simulando que es Domingo
2. El lunes modificamos erroneamente fichero1.txt fichero2.txt y creamos fichero3.txt y fichero4.txt
3. Martes creamos fichero5.txt
4. Miercoles borramos erróneamente fichero1.pdf y creamos otroFichero3.jpg y otroFicheroLog4.jpg
5. Jueves borramos erróneamente otroFichero1.jpg y creamos ficheroLog3.txt y ficheroLog4.txt
  
Cuando llegamos el viernes para solucionar los problemas nos encontramos con las siguientes copias de seguridad hechas:
![image](https://github.com/regueiroangel/UD7/assets/111371837/2f0e9254-183a-4673-a191-addb4b8c3114)

Para restaurar esto haremos lo siguiente:  


