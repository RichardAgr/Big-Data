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


