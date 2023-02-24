# COMANDOS

## CREAR PROYECTOS

`
curl -s "https://laravel.build/mi-proyecto" | bash
`

## ARRANCAR SAIL

`
./vendor/bin/sail up -d
`

## CONTROLADOR

- Añadir CONTROLADOR vacio

` php artisan make:controller PeliculasController `

- Crea el modelo Trainer y además hace la migracion

`php artisan make:model Trainer -m  `

- modelo migracion controlador

`php artisan make:model Tarea -mcr ` 

## MODELO

` php artisan make:model Categoria`

## MIGRACIONES

- Creamos la migración (la tabla en la base de datos es en plural y el modelo en singular)

`php artisan make:migration create_categorias_table --create="categorias" `

- Editar tabla (podemos crear otra migración con otro atributo)

`php artisan make:migration anadir_telefono_tabla_cate --table="categorias" `


 - Deshacer la última migración ejecutada y registrada en la base de datos.

`php artisan migrate:rollback `


- Borra la base de datos y la carga de nuevo.
`php artisan migrate:fresh`  

## RUTAS

- Genera todas las rutas get,put,post de un crud... Route::resource() 
   
` php artisan routes`  

- Para listar las rutas, en terminal
  
`php artisan route:list -v`   