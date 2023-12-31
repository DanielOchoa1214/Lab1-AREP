# Taller 1 AREP - Daniel Sebastian Ochoa Urrego

Este proyecto es un servidor web de consulta de películas. En él se uso un API externa www.omdbapi.com de donde sacamos toda la información y una implementación básica de Cache junto con un servidor que maneja a muy bajo nivel las conexiones hacia él y las peticiones del cliente.

## Iniciando

Estas instrucciones te ayudarán a tener una copia de este proyecto corriendo en tu máquina local, en donde podras hacer pruebas o desarrollar sobre él 

### Prerrequisitos

* Git 
* Java
* Maven

Si aún no tienes instaladas estas tecnologias, los siguientes tutoriales te pueden ayudar

* Git: https://www.youtube.com/watch?v=4xqVv2lTo40
* Java: https://www.youtube.com/watch?v=BG2OSaxWX4E
* Maven: https://www.youtube.com/watch?v=1QfiyR_PWxU

### Instalando el proyecto

Para hacer una copia local del proyecto, debes abrir tu terminal, dirigirte al directorio donde quieras que este el proyecto y usar el siguiente comando

```
git clone https://github.com/DanielOchoa1214/Lab1-AREP.git
```

Luego muevete al directorio creado y desde ahi ejecuta este comando

```
mvn exec:java
```

Ya que la aplicacián haya iniciado, puedes dirigirte a tu navegador de preferencia y entrar en http://localhost:35000 para ver la app corriendo, en ella encontraras una barra de busqueda y un botón. Si quieres buscar una película solo debes poner el título en la barra y darle click al botón (Nota: el hacer enter no funcionará, solo recargaras la pagina)

<img width="1680" alt="Ejemplo busqueda" src="https://github.com/DanielOchoa1214/Lab1-AREP/assets/77862016/9be37034-82fa-4650-a3c4-34dd8dda9b81">

## Corriendo los tests

Para correr los tests unitarios creados debes entrar al directorio del proyecto (Al clonarlo probablemente sea llamado "Lab1-AREP") y correr el siguiente comando 

```
mvn test
```

Y si tanto tu como yo hicimos todo bien, los test deberian correr y ser exitosos. 

Ahora, el proyecto tiene 2 tests, el primero es una prueba simple donde vemos que la API fachada si sea capaz de traer información correcta dado el titulo de una película, y el segundo prueba que nuestra app sea resistente a pedidos concurrentes chequeando que el cache se mantenga consistente a travez de estos pedidos.

## Documentacion

Para visualizar la documentación del proyecto solo debes correr el siguiente comando desde el directorio raiz del proyecto 

```
mvn javadoc:javadoc
```

Y en la siguiente ruta encontrarás el archivo index.html en donde si lo abres desde el navegador podras ver toda la documentación

```
./target/site/apidocs
```

## Construido con

* Amor
* [Maven](https://maven.apache.org/) - Administrador de dependencias
* [OMDAPI](https://www.omdbapi.com) - API externa de consulta

## Version

1.0-SNAPSHOT

## Autores

Daniel Sebastián Ochoa Urrego - [DanielOchoa1214](https://github.com/DanielOchoa1214)

## Licencia

GNU General Public License family

## Diseño

En la aplicación separamos las clases de manera lógica usando los paquetes para abstraer la arquitectura dada en el enunciado. Según este tenemos que crear 3 componentes, los cuales fueron creados de la siguiente manera: 

* Web Client - Para separar lógicamente el cliente web creamos un paquete "webclient" donde tenemos la clase HttpServer, la cual es la encargada de administrar las peticiones del cliente y enviarle las respuestas a travez de los Sockets.
* Web Server Facade - Ahora, para este componente se creo el paquete "apifacade" en donde tenemos las clases HttpConnection y Cache, las cuales en conjunto son las escargadas de consultar la información de la película buscada, ya sea haciendo una petición a la API esterna o con la información guardada en cache.
* Concurrent Java Test Client - Para este componente usamos la estructura por defecto que Maven nos provee para hacer tests, por lo que si navegamos hasta el final de la carpeta test del proyecto podremos encontrar la clase de prueba de la fachada en donde probramos la correctitud de la información buscada por la API externa y la resistencia a la concurrencia del cache.

### Extencibilidad

Para ilustrar la extencibilidad de la aplicación se me ocurren 2 ejemplos:

* Si se quisiera cambiar el proveedor externo de información sobre las películas, solo deberiamos cambiar el valor de la variable "GET_URL" de la clase HttpConnection y todo deberia seguir funcionando normalmente
* Si se quisieran añadir funcionalidades a la fachada solo se deberia crear un nuevo metodo dentro de la clase HttpConnection siguiendo los patrones de nombramiento con los verbos HTTP y tomar en cuenta el nuevo path dentro del cliente web para correr cada método según se necesite.

### Patrones

Se puede observar un patrón de diseño y un patrón que arquitectura

* El patrón de diseño corresponde al patrón Singleton, el cual se implementó, ya que dentro de la aplicación se debe tener (por ahora) solo un cache en donde guardemos las películas
* El patrón de arquitectura seria la implementación a bajo nivel de un MOM, ya que nuestra fachada sirve en resumidas cuentas como un intermediario que hace peticiones a la API externa y construye la respuesta de manera que sea mas fácilmente legible por el cliente Web 

### Modularización

En la aplicación cada clase tiene una funcion en especifico 

* HttpServer: Es el Cliente Web, el encargado de manejar los Sockets, las peticiones recividas y acorde a ellas dar la respuesta apropiada al navegador
* HttpConnection: Es la clase principal de la fachada, es la encargada de hacer las peticiones al API externa y transformar la respuesta en un objeto mas facilmente manejable por el cliente web.
* Cache: Es la clase encargada de simular un cache en la fachada, este se encarga de guardar y obtener una película en él, además de mirar si la película ya ha sido buscada antes.
  
## Agradecimientos

* A nuestro querido profesor de Arquitectura empresariales Daniel Benavides
* Figo

