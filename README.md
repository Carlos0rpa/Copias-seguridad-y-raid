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
- [f] otroFichero1.jepg
- [f] otroFichero2.jepg
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
- [f] fichero1.txt [ERROR]
- [f] fichero2.txt [ERROR]
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero2.jepg
- [f] otroFichero3.jepg
- [f] otroFichero4.jepg
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
- [f] otroFichero1.jepg
- [f] otroFichero2.jepg
- [f] otroFichero3.jepg
- [f] otroFichero4.jepg
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
- [f] otroFichero1.jepg
- [f] otroFichero2.jepg
- [f] otroFichero3.jepg
- [f] otroFichero4.jepg
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
- [f] otroFichero1.jepg
- [f] otroFichero2.jepg
- [f] otroFichero3.jepg
- [f] otroFichero4.jepg
````   
## 2. Sistema RAID Windows

1. El primer paso sería añadirle un nuevo disco virtual a nuestra máquina con Windows 11
![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/tipo_disco.png).

2. El siguiente sería escoger el tamaño del disco, en este caso añadiremos uno de 10 GB

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/seleccion_del_disco_de_10GB_vm.png).

3. A continuación. Ya en nuestra máquina virtual, ponemos en línea nuestro nuevo disco virtual, dándole clic derecho sobre el mismo y clicando donde pone "en línea".

4. Para crear un volumen simple, le damos clic derecho sobre nuestro disco sin formato, le damos un formato (NTFS, FAT32, FAT, ReFS) y un nombre para el volumen.


### Creación de un volumen distribuido.

1. Para la creación de este volumen, reduciremos el tamaño de nuestro disco principal en 1024 MB y de nuestro disco virtual 2048 MB (ambos sin formato).

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/vol_distr.png)

2. Al volumen al que le hayamos dado clic para iniciar el proceso de creación del volumen distribuido, le añadiremos el volumen restante, en nuestro caso, al de 1 GB le añadiremos el de 2 GB.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/2_gb_mas.png)

3. Al igual que hicimos con el volumen simple, le daremos un formato (en este caso y en los siguientes NTFS) y un nombre para el volumen.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/formato_nombre.png)

4. Así es como quedaría una vez finalizado, nuestro particionado con volúmenes distribuidos.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/distr.png)


### Creación de un volumen seccionado

Para la creación de este volumen sería exactamente lo mismo que en el anterior caso, simplemente los cambios son, que los discos tienen ambos 4 GB.

1. Clic derecho en el disco que le queramos crear el tipo de volumen seccionado.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/vol_secc_4gb.png)

2. Le damos formato y nombre al volumen y finalmente nuestro volumen seccionado aparecería de la siguiente manera.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/vol_secc.png)


### Creación de un volumen reflejado

Para este volumen necesitaremos otras dos secciones de disco (una por cada uno) de 4 GB de almacenamiento, sin formato.

1. El primer paso sería darle clic derecho sobre uno de los discos y seleccionar la opción de "Nuevo volumen refeljado".

2. El siguiente paso sería darle formato y nombre al volumen, como hicimos en los anteriores dos apartados.

La información del volumen sería la siguiente.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/Info_refl.png)

3. Y finalmente así es como quedaría el gestor de discos con estes volúmenes reflejados.

![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/vol_refl.png)

## 3. Sistema RAID Ubuntu
Para crear un sistema raid en ubuntu utilizaremos la erramienta mdadm, para instalarla utilizaremos el siguiente comando:
````
sudo apt-get install mdadm
````
Una vez instalado comenzaremos a crear las RAID
### RAID 0 
Usaremos 2 discos para el RAID 0, para conseguirlo en Ubuntu usaremos los comandos de mdadm instaldo previamente. Para ello usaremos el comando 
````
sudo mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc
````
> [!WARNING]
> Tanto en este como en los siguiente apartados /dev/sdX, "X" elige el disco en cuestión.
  
Y nos da el siguiente resultado  
![image](https://github.com/Carlos0rpa/Trabajo-M.2/blob/main/raid0.png)
### RAID 1
Volvemos a usar dos discos, el comando necesario para crearlo es el siguiente:
````
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdc /dev/sdd
````
### RAID 5
Usando de nuevo dos discos usamos el siguiente comando:
````
sudo mdad-create /dev/md0 --level=5 --raid-devices=2 /dev/sdb /dev/sdd
````
### TABLA COMPORTAMIENTO
| Tipo RAID    | Discos Necesarios | Espacio Disponible                     | Tolerancia al Fallo          | Rendimiento                                                             |
|--------------|-------------------|----------------------------------------|------------------------------|-------------------------------------------------------------------------|
| **RAID 0**   | 2                 | Total a la sunma de los discos (20GB) | Ninguna, no admite fallo                      |  Aumentado tanto en lectura como en escritura        |
| **RAID 1**   | 2                 | Como son ambos discos de 10GB solo tendremos disponible 10GB | Admite fallo de 1 disco | Lectura mejorada y escritura ni mejora ni pérdida. |
| **RAID 5**   | 3                 | (N-1) * GB Discos ((3-1) * 10 = 20GB) | Admite fallo de 1 disco | Mejora de lectura y pérdida de escritura por la paridad. |
