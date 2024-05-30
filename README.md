# Big data: Estraccion de datos de la paginas Los tiempos, El deber, Opinion 
##Primer paso: Base de datos
![logoMysql](https://github.com/RichardAgr/Big-Data/assets/136004365/c70f40ac-3e19-4f3d-905f-139593d20f00)
<p>
La base de datos que se esta usando es Mysql, esta de forma local, en el cual se tiene que insertar el codigo para crear la tabla:
</p>
```mysql
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
