# Big data: Estraccion de datos de la paginas Los tiempos, El deber, Opinion 
##Primer paso: Base de datos
![logoMysql](https://github.com/RichardAgr/Big-Data/assets/136004365/c70f40ac-3e19-4f3d-905f-139593d20f00)
<p>
La base de datos que se esta usando es Mysql, esta de forma local, en el cual se tiene que insertar el codigo para crear la tabla:
</p>

```
CREATE DATABASE BigData;
USE BigData;
CREATE TABLE noticias (
    id VARCHAR(500) PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    date DATE NOT NULL,
    content TEXT NOT NULL,
    link VARCHAR(500) NOT NULL,
    page VARCHAR(50) NOT NULL,
    linkImage VARCHAR(500) NOT NULL,
    contentFilter TEXT NOT NULL
);
```

<p>
Posterior a eso se tiene que ir al archivo a la ruta src/lib/bd.ts y se tiene que cambiar el password por la contrasenia que tiene la base de datos(contrasenia que se puso al momento de crear la base de datos, eso es diferente por cada caso):

![password](https://github.com/RichardAgr/Big-Data/assets/136004365/1f2595bc-2ec8-49e1-b08f-d1c960e4e396).

una vez ejecutado el codigo de mysql como resultado tenemos lo siguiente:

![baseDatos](https://github.com/RichardAgr/Big-Data/assets/136004365/5cf11857-4329-42d2-a6f0-3b0e12142771)![baseDatosCreado](https://github.com/RichardAgr/Big-Data/assets/136004365/03447e1e-b3d8-4807-814a-9d40d568d464)
</p>

##Segundo paso: realizar el scraping
<p>
Para la parte del scraping se tiene que tomar en cuenta el tiempo, se recomiendo poner el valor de 30 pagianas y esto tardaria alrededor de 4 horas en recolectar los datos de las 3 paginas en simultaneo si se presiona el boton "Todas las  paginas", si es pagina uno por uno tardara un maximo de 1 hora.

![Scraping](https://github.com/RichardAgr/Big-Data/assets/136004365/74e1275e-709b-44db-a104-97f265f55854)

Nota: El proyecto esta en react asi que una vez descargado el proyecto, abrimos la carpeta en un editor de codigo y ejecutamos **npm install** para descargar las dependencias y luego para correr el proyecto el **npm run dev**.

**SCRAPING FINALIZADO**
Una vez finalizado el scraping ya deberiamos tener la base de datos con datos:

![datos](https://github.com/RichardAgr/Big-Data/assets/136004365/1394158e-3332-489b-889a-f6ef29a6d5bd)
</p>


##Generar archivo txt


<p>
Una vez teniendo los datos en la base de datos vamos a poner una fecha inicial y una fecha final para poder descargar las noticias en un txt.

![fecha](https://github.com/RichardAgr/Big-Data/assets/136004365/da443c88-e586-40ad-a9e5-fec2cd9027e5)

Vamos a generar la solicitud presionando el boton: "INICIAR SOLICITUD".
Y en la interfaz nos tendria que mostrar:

![generamosTxt](https://github.com/RichardAgr/Big-Data/assets/136004365/8e2909ce-c0e4-4d72-a78d-83ea64d16c72)

Lo que nos muestra es las noticias que hemos extraido del scraping y para poder descargarlos en un formato txt vamos a presionar en el boton "DESCARGAR NOTICIAS" y posterior a eso guardamos el txt con el nombre **noticias** ya que en hadoop lo va a reconocer con ese nombre para su analisis.

![txt4](https://github.com/RichardAgr/Big-Data/assets/136004365/25e8f7c6-d6fe-4a78-8b91-35e218a39dde)
</p>


##Hadoop configuraciones y comandos


<p>
    Antes tenemos que descargar el archivo hadoop-bigData que esta en este link ![]https://drive.google.com/drive/folders/1VC97MHkIZ1d6AmC9DC8WQtyRwH5oJsCl
Para poder usar Hadoop vamos a tener que instalar:
</p>
- Virtual box.
-El ova (Carpeta ova).

![virtualBox](https://github.com/RichardAgr/Big-Data/assets/136004365/50413ff5-a5bb-4c1f-94ef-7168f57bb300)

<p>
iniciamos el ova en el virtual box y cuando ya este listo la maquina virtual nos pedira un usuario y contrasenia:
usuario: hadoop
contrasenia: hadoop
luego ejecutamos estos comandos para su correcto funcionamiento:
</p>

```
[hadoop@nodo1 ~]$ start-dfs.sh
[hadoop@nodo1 ~]$ start-yarn.sh
[hadoop@nodo1 ~]$ ./automatizacion.sh 


[hadoop@nodo1 ~]$ rm -rf /home/hadoop/resultado/*
[hadoop@nodo1 ~]$ hdfs dfs -put noticias.txt /noticias
[hadoop@nodo1 ~]$ cd /opt/hadoop/hadoop-2.7.7
[hadoop@nodo1 hadoop-2.7.7]$ cd share/
[hadoop@nodo1 share]$ cd hadoop/
[hadoop@nodo1 hadoop]$ cd mapreduce/
[hadoop@nodo1 mapreduce]$ hadoop jar hadoop-mapreduce-examples-2.7.7.jar wordcount /noticias /resultado
[hadoop@nodo1 mapreduce]$ hdfs dfs -get /resultado/part-r-00000 /home/hadoop/resultado
[hadoop@nodo1 mapreduce]$ hdfs dfs -rm -r /resultado
[hadoop@nodo1 mapreduce]$ hdfs dfs -rm /resultado/noticias.txt


hdfs dfs -rm -r /resultado
hdfs dfs -rm /noticias/noticias.txt
```
<p>
cabe recalcar que solo se ejecuta una vez todos estos comandos pero si luego se apaga la maquina virtual, y se la vuelve a prender solo ejecutamos:

[hadoop@nodo1 ~]$ start-dfs.sh

[hadoop@nodo1 ~]$ start-yarn.sh

para que funcione correctamente.
</p>

##Analizar noticias

<p>
Para analizar el archivo txt en el hadoop tenemos que conectarnos con nuestra maquina virtual desde nuestra maquina, en este caso tenemos windows 10, se esta usando filezilla para poder hacer la transferencia del archivo noticias.txt, para esto a que darle el servidor y un puerto.

servidor: sftp://localhost
usuario: hadoop
contrasenia: hadoop
puerto: 22022

![servidorfile](https://github.com/RichardAgr/Big-Data/assets/136004365/b4953613-cdc5-434b-9703-30fae8bce20e)

y si todo esta correcto nos mostrar esto:

![conexionCorrecta](https://github.com/RichardAgr/Big-Data/assets/136004365/6e8c2124-c399-4d6c-87a0-31b15ff25d15)

una vez hecho estos pasos, vamos a arrastras el archivo txt al apartado de hadoop:

![arrastre](https://github.com/RichardAgr/Big-Data/assets/136004365/94ec9632-bf65-4fe9-a5e0-046711ad2376)

esperamos ya que suele tardar y notamos que se transferio correctamente el archivo y lo podemos en en hadoop:

![traspaso](https://github.com/RichardAgr/Big-Data/assets/136004365/de3abd2a-f6a9-4e6f-be4d-e844890bf3be)

como se puede apreciar tenemos el archivo txt.

luego ejecutamos el comando:

./automatization.sh

y esperamos a que termine
para poder ver que esta haciendo hadoop podemos poner el dominio:
http://localhost:8089/	

![progresoHadop](https://github.com/RichardAgr/Big-Data/assets/136004365/66a32160-2167-4a66-9694-4b693eb148b9)

se ve que esta haciendo el proceso y cuando termine nos mostrara SUCCEEDED.
Finalizado el proceso nos vamos a la carpeta hadoop y este no ha generado un archivo este tenemos que traerlo a nuestro escritorio, este es el resultado de todo el proceso hecho:

![resultado](https://github.com/RichardAgr/Big-Data/assets/136004365/850c2d2c-e427-4bbc-8ece-f0907d19595a)

este archivo lo cambiamos por el nombre **noticia.txt** y lo ponemos a la interfaz para ver los resultados:

![filtrps](https://github.com/RichardAgr/Big-Data/assets/136004365/95fbd946-7695-4eac-8a64-2462f0e4c05b)

presionamos el boton "CARGAR MAP REDUCE" y colocamos el archivo noticias.txt (El cual se cambio de nombre por ser el resultdo despues del proceso de hadoop), escogemos una fecha inicial y final y presionamos el boton "INICIAR SOLICITUD " esto para traer las noticias de la base datos, y con todo eso ya tenemos las palabras claves para ir escogiendo cuales fueron las noticas mas publicadas en ese rango de fechas. 

![resultadoFinal](https://github.com/RichardAgr/Big-Data/assets/136004365/eb8478c7-7f08-4649-95a7-f2a4ec158647)

se puede escoger los periodicos como las palabras claves.
</p>
