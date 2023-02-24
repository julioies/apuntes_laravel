# PAQUETES LARAVEL

https://github.com/LaravelDaily/Best-Laravel-Packages

## Traducción

https://github.com/Laraveles/spanish


## DEBUGBAR

` composer require barryvdh/laravel-debugbar --dev `

Dentro de config->App añadimos en provider

`Barryvdh\Debugbar\ServiceProvider::class,`

En aliases

`'Debugbar' => Barryvdh\Debugbar\Facade::class,`


## CREAR MODELOS ELOQUENT desde la BASE DATOS


Introducimos el comando de instalación del paquete

`composer require reliese/laravel --dev`


Registrar el provider en app/Providers/AppServiceProvider.php

`
public function register()
{
    if ($this->app->environment() == 'local') {
        $this->app->register(\Reliese\Coders\CodersServiceProvider::class);
    }
}
`


Publicas el fichero de configuración:

`php artisan vendor:publish --tag=reliese-models`


Comandos: 

Crea todos los MODELOS de las tablas de la BD 
`php artisan code:models`

Crea un MODELO de una tabla
`php artisan code:models --table=users`


https://cristianllanos.medium.com/eloquent-models-from-my-database-5d74c632e03c#.qg7cd4vri
https://github.com/reliese/laravel

## GENERAR CRUD AUTOMÁTICAMENTE

https://github.com/awais-vteams/laravel-crud-generator

https://diarioprogramador.com/crear-crud-en-laravel-con-generador-de-cruds/

https://github.com/appzcoder/crud-generator

## GENERAR PDF

Instalamos libreria

`composer require barryvdh/laravel-dompdf`

añadimos la siguiente ruta en config/app.php en el array 'provider'

`Barryvdh\DomPDF\ServiceProvider::class,`


luego añadimos la siguiente ruta en config/app.php en el array 'aliases'

`'PDF' => Barryvdh\DomPDF\Facade::class,`


crearmos ruta en Web

`
Route::get('/imprimir', 'RelacionController@imprimir')->name('imprimir');
`



y una vista llamada 'imprimir.blade.php' con código en html

```php 
<body>
        <h1>Página de prueba</h1>

</body>
```


en el controlador 

```php
use PDF;
......

public function imprimir(){
     $pdf = PDF::loadView('imprimir');
     return $pdf->download('ejemplo.pdf');
}
```

https://www.nigmacode.com/laravel/Generar-pdf-Dompdf-Laravel



## DESCARGAR FICHEROS .ZIP

`composer require chumper/zipper`

dentro de config/app.php en providers

`Chumper\Zipper\ZipperServiceProvider::class,`

en aliases

`'Zipper' => Chumper\Zipper\Zipper::class,`


*** Controlador ***

```php
use Zipper;

class Controller extends BaseController
{
 

    public function comprimirDescargar()
    {
        /*Añadimos la ruta donde se encuentran los archivos que queramos comprimir,
          en este ejemplo comprimimos todos los que se encuentran en la carpeta 
          storage/app/public*/
        $files = glob(storage_path('app/public/*'));
        
        /* Le indicamos en que carpeta queremos que se genere el zip y los comprimimos*/
        Zipper::make(storage_path('app/public/ejemplo.zip'))->add($files)->close();
        
        /* Por último, si queremos descarlos, indicaremos la ruta del archiv, su nombre
        y lo descargaremos*/
        return response()->download(storage_path('app/public/ejemplo.zip'));
    }


}
```

*** RUTA ***
`
Route::get('/comprimir', 'Controller@comprimirDescargar')->name('comprimir');
`

*** VISTA ***

`<a href="{{route('comprimir')}}">Comprimir y descargar</a>`


*** FAKER SEEDER *** 

https://www.tutsplanet.com/how-to-use-faker-with-laravel/