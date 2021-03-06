# Proyecto ficohsa-practica

## Contenido

- [Tecnologias](#Tecnologias)
- [Arquitectura](#Arquitectura)
- [Instalacion](#Instalacion)
- [Estrategia](#Estrategia)
- [Api](#Api)
- [Datos de prueba](#Datos)
- [Peticiones Agresivas](#Peticiones)
- [Licencia](#Licencia)

# Tecnologias

* [Java 11] - Lenguaje de programacion
* [SpringBoot 2.7.0] - Plataforma de desarrollo
* [apache-maven-3.8.2] - Manejo dependencias 
* [BlueJ] - Desarrollo de la logica del programa
* [IntelliJ IDEA 2021.2.1 (Community Edition)] - Ide de Desarrollo
* [Docker] - Virtualizacion BD (Opcional)
* [PostgreSQL] - Base de datos
* [Git] - Versionado
* [GitHub] - Repositorio
* [Swagger] - Documentacion


# Arquitectura

- Desarrollo SpringBoot, modelo MVC, Tomcat Embebido

# Instalacion

- Se necesita tener lo siguiente

| Requiere |  |
| ------ | ------ |
| Java 11 | https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html |
| Git | https://git-scm.com/downloads |
| Maven | https://maven.apache.org/download.cgi |
| Postgresql | https://www.postgresql.org/download/ |
| Docker | https://www.docker.com/products/docker-desktop/ |

Con los paquetes listos, proceder a clonar el repositorio
```sh
git clone https://github.com/e972136/ficohsa-practica.git
```

Desde la terminal de IntelliJ (o directorio raiz)
```sh
mvn clean install
```

- Para ver la version en linea

```sh
https://gaspar-xmen.herokuapp.com/swagger-ui/index.html#/
```

# Estrategia

Para el desarrollo de la api se tomaron las siguientes consideraciones

* Validacion codigo DNA: se valida que no este null(@notnull), que no este vacia(@notempty) 
y que al menos tenga 4 filas y siempre 4 columnas como minimo (@dnavalidator)

- Primero: se considera una secuencia exitosa si se tiene 4 codigos iguales consecutivos (Vertical, Horizontal, Diagonal)
- Segunda: Debe haber por lo menos dos secuencias exitosas
- Tercero: Segun lo anterior, determinamos si es mutante, o si es humano, y retornamos la respuesta correspondiente
- Cuarto: De ser humano, retornara 403, de ser mutante 200.
- Quinto: Si el ADN no esta registrado, se procede a ser almacenado para determinar estadisticas futuras.

Una vez desarrollado la API, se procede a su publicacion en Heroku

# Api

- El puerto asignado es 8081 (por si el nativo 8080 esta ocupado)
- En postgresql debe haber una base de datos llamada "hatleycastle"

| DESCRIPCION | URL | Peticion | Respuesta
| ------ | ------ | ------ | ------ |
| Servicio Validar Mutante | localhost:8081/mutant |  Post  | 200 Valida / 403 Humano
| Servicio Estadisticas | localhost:8081/stats |  Get  | JSON
| Servicio listar | localhost:8081/listar |  Get  | JSON
| Servicio limpiar | localhost:8081/limpiar |  Delete  | JSON

- Hay una instancia en Heroku, para ver la version en linea directo en swagger.
- se se desea usar los mismos endpoint, reemplazar "localhost:8081" por "gaspar-xmen.herokuapp.com" 
en postman o el cliente rest que esten usando

```sh
https://gaspar-xmen.herokuapp.com/swagger-ui/index.html#/
```


# Datos

- /mutant: Evalua si el candidato es un mutante

  - Para Mutante

Request:
{
  "dna": ["ATGCGA","CAGTGC","TTATGT","AGAAGG","CCCCTA","TCACTG"]
}

Response: 200 - ok

- Para Humano

Request:
{
  "dna": ["ATGCGA","CXGTXC","TTATGT","AGAAGG","XCXCAA","TCACTA"]
}

Response: 403 - Forbiden

- /stats: Retorna una estadistica del ratio de mutantes con respecto a humanos.

Response:

{
  "count_mutant_dna": 0,
  "count_human_dna": 0,
  "ratio": 0
}

- Estos Endpoints estan para hacer pruebas en la bd, una vez pasado a producction, se recomiendo comentarlos

  - /limpiar: Hace un limpiado de la base de datos
  - /listar: Lista los elementos almacenados en la base de datos


# Peticiones

La persistencia dependera de donde este alojado el servicio, y cantidad de replicas existentes.
Esto se puede manejar usando Eureka/Kubernetes, que distribuyen el manejo de cargas. 

# Licencia

Libre distribucion
