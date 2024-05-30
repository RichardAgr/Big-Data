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
