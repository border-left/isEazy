# Efraín isEazy

## Info

The Laravel project together with its DB are deployed in docker, the necessary docker-compose.yml file is included in the project.

## Requirements

- Docker

## Init

1º Build docker and start php laravel proyect.

(By first locating ourselves in the directory where the file docker-compose.yml is located) Keep terminal open to keep the project up and running.
```bash
docker-compose up
```

2º Run to execute Migrations
```bash
docker exec -it efrain-iseazy-myapp-1 php artisan migrate
```

3º Run to execute Seeders
```bash
docker exec -it efrain-iseazy-myapp-1 php artisan db:seed
```

## Util

Reset database
```bash
docker exec -it efrain-iseazy-myapp-1 php artisan migrate:fresh --seed
```

## Start

Start php laravel proyect (If it was stopped).
```bash
docker exec -it efrain-iseazy-myapp-1 php artisan serve
```

## Docker containers

1º Access mariadb container.
```bash
docker start efrain-iseazy-mariadb-1
```

2º Access php laravel container.
```bash
docker start efrain-iseazy-myapp-1
```

## API testing with Postman

I have prepared a Postman Collection for API endpoint testing, you can access it through the following link.

The parameters I pass in Postman requests are in a Json in: Body > raw.

Postman API requests are prepared to be executed in order from first to last, otherwise they may change records needed for another request.

Independently of the link there is a json file in the project to import the Postman collection 

Efraín isEazy Collection.postman_collection.json.

[Link](https://www.postman.com/efrain91/workspace/efran-iseazy/documentation/7131583-d057ebac-1c73-43d8-aa23-b762fa6d531d?entity=folder-b0d1515c-b8ce-4752-bb4e-9efd92357616)
```
https://www.postman.com/efrain91/workspace/efran-iseazy/documentation/7131583-d057ebac-1c73-43d8-aa23-b762fa6d531d?entity=folder-b0d1515c-b8ce-4752-bb4e-9efd92357616
```

## Developer comments for isEazy

### General comments

- He decidido no usar DELETE CASCADE en las tablas para mantener la integridad de los datos en posibles errores en la lógica, de forma que cuando se necesite eliminar una **Shop** haya que eliminar explícitamente los registros de **ShopProduct** con detach.
- Dicho detach de los **ShopProduct** de un **Shop** se ejecutará en **ShopObserver** para hacer uso de los Observers.
- Teniendo en cuenta que no se indica en la descripción de la prueba el uso de autenticación y usuarios, he decidido hacerlas públicas con el middleware 'guest'. 
- He tenido dificultades en los endpoints PUT y DELETE para pasarle la **shop** como objeto, por lo que finalmente se la he pasado como **shopId**, y en los métodos de los controladores he hecho la consulta por ese **shopId** para obtener el objeto. Esto en cambio no me ha ocurrido con el endpoint GET. He prefereido entregarlo así para no alargar el tiempo de entrega.
- Finalmente también he usado **shopId** en el endpoint GET, ya que si le pasaba un id que no existía en la BD, directamente me daba un error 404, cosa que no ocurre y puedo controlar directamente pasándole **shopId**. 
- Inicialmente Eloquent no soporta primary key compuestas, aunque yo he optado por esta opción para la tabla **shops_products** ya que así evitamos duplicidad de registros con **quantity** diferentes, lo que podría ser un problema si llegara a suceder. Ya que tuve problemas para hacer un **save()** de los registros de esta tabla (Al tener primary key compuesta), he optado por crear el método **ShopProduct->updateDB()** para ejecutar ahí el SQL de la actualización de **quantity** al hacer una compra.
- Los campos **name** de **shops** y **products** son **unique**, de forma que así puedo comprobar si una tienda o producto existen.
- Aún podía haber seguido optimizando el proyecto, pero he preferido entregarlo ya para no alargarlo mucho. Me encanta optimizar el código y nunca encuentro el momento de parar :).
- No tengo mucha experiencia en Lararvel (Sobre un año o menos), seguramente encontréis detalles que no estén 100% optimizados, pero Laravel es un framework en el que me quiero especializar, aunque mis experiencias laborales aún no me han dado esa oportunidad.

### Comments for API endpoints

- Listado de las tiendas con número de productos de cada una.
    - .

- Descripción de una tienda, con el listado de productos de la misma y
  cantidad.
    - .
    
- Creación de una tienda. Posibilidad de pasarle una colección o array de
  productos para almacenarlos en base de datos.
    - Dado que no lo especificaba, he decidido crear los **Product** en caso de que no existan antes de asigarle las cantidades. Otra opción hubiera sido devolver un mensaje de que esos **Product** no existían en la BD.
    
- Edición de una tienda.
    - .
    
- Eliminación de una tienda.
    - .
    
- Añadir endpoint de venta de producto, con aviso en la respuesta si la tienda está a
  punto de quedarse sin stock, o si la operación es imposible por falta de stock.
    - .

## License
[MIT](https://choosealicense.com/licenses/mit/)
