# APUNTES LARAVEL 

## RUTAS

Las rutas se situan en ***Mi-PROYECTO->ROUTES->WEB***

```.
└── routes
      └── web
```

Aquí podemos ver la primera ruta.

``` php
Route::get('mundo', function () {
    return 'Hello Clase 2 DAW';
});
```

Le pasamos a la ruta un identificador que tendrá un valor por defecto y esa ruta tendrá un nombre dentro de la misma vista, asignamos valor por defecto

```php
Route::get('/show/{id?}',function($id="122"){

  return $id;

})->name('show');
```

Otra forma de ruta y utilizando un pattern númerico (sólo pueden introducir números)

```php
Route::get('/par-o-impar-{numero}',function($numero){
     return $numero;

    //return redirect()->to('/show/3'); -> te redirije a la ruta que le indicas

    //return redirect()->route('show') -> te redirije a la ruta que tiene el nombre asignado en el name.

    //return redirect()->route('show',['id' => '222']) -> en caso de pasarle algun parámetro
} )->where(['numero' => '[0-9]+']);

// de esta forma añadimos una expresión regular para que solo puedan introducir números
```


*** Restricciones con Expresiones Regulares ***


```php
Route::get('user/{name}', function ($name) {
//
})->where('name', '[A-Za-z]+');   

//nomes poden passar lletres miníscules o Majúscules como mínim una volta
```

```php
Route::get('user/{id}', function ($id) {
//
})->where('id', '[0-9]+');

//com a mínim, a soles podem passar com a mínim un nombre.
```

```php
Route::get('user/{id}/{name}', function ($id, $name) {
//
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);

//le pasamos un array en caso de más de un elemento.
```

Ruta que nos lleva directamente a una vista e incluso pasarle un argumento.

```php
Route::view (directo a la vista)

Route::view('/registro', 'registro');

 //en el caso que sólo queramos enviar a una vista sin pasar por el controlador

Route::view('/welcome', 'clase', ['nombre' => 'Julio']);
```

Especificamos que podemos usar GET y POST para una ruta

```php
//verbos HTTP múltiples
Route::match(['get', 'post'], '/', function () {
//
});


//verbos HTTP cualquiera
Route::any('/', function () {
//
});
```
_______
##  CONTROLADOR

`php artisan make:controller PeliculasController`

Creamos un controlador vacio, **App->http->Controller->**

```
.
└── App
    └── http
         └── Controller
```

```php
//en route
//añadir la ruta en web.php arriba del todo.

use App\Http\Controllers\PeliculasController;

.............

Route::get('pelis', [PeliculasController::class, 'mostrar']);

//en controlador
public function index(){
   echo "hola Julio";
}
```

## VISTA

Podemos aprovechar herencia en las vistas, por ello, vamos a crear.

También podemos crear una carpeta que contiene vistas "menú" y dentro las páginas, para acceder a ellos, habría que poner "menu.pagina"

*** Vista Padre PADRE.blade.php ***
``` html
<!DOCTYPE html>
<head>
    <title>
        @yield('titulo')
    </title>
</head>
<body>

<ul>
  <li><a href="#home">Home</a></li>
  <li><a href="#news">News</a></li>
  <li><a href="#contact">Contact</a></li>
  <li><a href="#about">About</a></li>
</ul>

@yield('contenido')

</body>
</html>
```

*** Vista Layout Hija hija.blade.php ***

```python
@extends('padre')

@section('titulo','Página Heredada')

@section('contenido')
    <h1>Página Hija</h1>
@endsection
```

## SESIONES GLOBALES

```php
session(['nombre'=>'Julio']); //otra forma de crear variables sesión

$nombre= session('nombre');  

session()->forget('nombre');  //Borra Julio

session()->flush();           //borra todo tipo de variables sesión, flash...

dd(session()->all())         //muestra todas las variables de sesión

/*mostrar variables sesión en blade VISTA*/

{{session('julio')}}

```

https://laravel.com/docs/9.x/session#main-content

## MENSAJES FLASH

```php
//cuando en una página queremos enviar un mensaje flahs podemos poner

return back()->with('status','hemos recibido el mensaje'); //guardamos un mensaje flash

//luego en la misma pagina (back) ponemos una condicion en blade si hay mensaje de sesion muestra sino formulario.
 @if(session('status'))
   {{session('status')}}
@else
   formulario
@endif

//como vamos a repetir este codigo en muchos lados, podemos ponerlo en un fichero partial (resources-views-partial) y luego en el codigo lo sustituimos por @include en el editar.blade.php
```

## ALERT SWEET

https://sweetalert.js.org/guides/

*** VISTA ***
```javascript
<script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>

//ponerlo debajo del DOM en la vista
<script>
@if (session('success'))
swal("Buen Trabajo!", "{{ session('success') }}", "success");
@endif
</script>
```

*** CONTROLADOR ***
```php
//en el controlador 
return back()->with('success','Hemos recibido el mensaje');
```

## BASES DE DATOS - ORM

Si clicamos el comando **php artisan** nos dará un menú de todas las opciones disponibles. 

A nosotros nos interesa crear una base de datos empezaremos con una tabla, es recomendable primero crear la migracion que corresponde con la tabla en la base de datos **articulos** y a continuación crearemos el modelo que corresponde con la entidad **Articulo**. 

Es preferible crearlo en ese orden, también hay que tener cuidado a la hora de crear la migración en plural y la entidad en singular.


`php artisan make:migration create_articulos_table --create="articulos"`

```php
//Tabla articulos
 Schema::create('articulos', function (Blueprint $table) {
            $table->increments('id')->unsigned();
            $table->text('titulo');
            $table->mediumText('descripcion');
            $table->timestamps();
          });
```

!!!warning "IMPORTANTE -  TIPOS DE COLUMNAS!"
    https://laravel.com/docs/9.x/migrations



!!!danger "CUANDO DA ERROR EN LA MIGRACION NO ENCUENTRA DRIVERS"
    https://alphaparticle.com/blog/solved-laravel-sail-could-not-find-driver-error/

    https://stackoverflow.com/questions/70923053/laravel-sail-could-not-find-driver-sql-show-full-tables-where-table-type-b


`php artisan make:model Articulo`

```php
//Modelo Articulo
class Articulo extends Model
{
    protected $table = 'articulos';  //hacemos referencia a la tabla artículos
    protected $fillable = ['titulo', 'descripcion'];   
       //fillable para proteger los campos que desea que permitan la actualización a la hora de insertar en la base de datos por asignación masiva
}
```

## SEEDER (SEMILLAS)


*** SEEDER normal ***

1. Ejecutamos en terminal php artisan make:seeder NombreSeeder

2. podemos ver que dentro de database->seeds se genera la nueva semilla e introducimos los siguientes datos.

```php
//añadimos en la clase NombreSeeder
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Hash;
use Illuminate\Support\Str;

.....................

for ($i = 0; $i < 10; $i++) {
    DB::table('articulos')->insert([
   'titulo' => Str::random(10),
   'descripcion' => Str::random(10).'@gmail.com',
   'created_at' => date('Y-m-d'),
   'updated_at' => date('Y-m-d')]);
}
```

3. luego en la clase DataBaseSeeder (otro fichero que ya está creado) activamos dentro de la clase 

`$this->call([NombreSeeder::class]);`

```php

public function run()
{

$this->call([
        PeliculasSeeder::class,
                   ]);
}
```

4. Para generar los datos insertarmos en terminal

 `./vendor/bin/sail php artisan db:seed`

https://laravel.com/docs/9.x/seeding#writing-seeders

## CONSULTAR DATOS

### CONSTRUCTOR DE CONSULTAS (Query Builder)

Cuando estamos usando conexiones múltiple

```php
//añadimos 
use Illuminate\Support\Facades\DB;
....

//obtenemos todos los usuarios
$datos = DB::table('articulos')->get();

foreach ($datos as $dato)
{
    echo $dato->descripcion;
}

//Más ejemplos de consultas
$dato = DB::table('articulos')->first();

$dato = DB::table('articulos')->where('name', 'julio')->get();

$dato = DB::table('articulos')->where('edad', '>', 100)->get();

$dato = DB::table('articulos')->where('estado', '<>', 'active')->get();

$dato = DB::table('articulos')->where('name', 'like', 'J%')->get();


//insertar
DB::table('articulos')->insert(
        ['titulo'=>"pepe",'descripcion'=>"nuevadescripcion"]
    );

//update
DB::table('datos')
              ->where('id', 1)
              ->update(['nombre' => 'Estefania']);

//delete
DB::table('datos')->delete();
DB::table('datos')->where('votes', '>', 100)->delete();
```
https://laravel.com/docs/9.x/queries#running-database-queries


### CONSULTAS SQL NATIVAS

```php
//seleccionar
$results = DB::select('select * from articulos where id = :id', ['id' => 1]);

//insertar en la Base users
DB::insert('insert into articulos (titulo, descripcion) values (?, ?)', [1, 'Dayle']);

//actualizar
$affected = DB::update('update users set votes = 100 where name = ?', ['John']);

//borrar
$deleted = DB::delete('delete from users');

//Ejecutando una instrucción general que no devuelve ningún valor
DB::statement('drop table users');
```
### ELOQUENT

```php
$articulos=Articulo::all();  //$escritores=Escritor::get();

$articulosOrdenados=Articulo::orderBy('descripcion','DESC')->get();

$articulo= Articulo::findOrFail(1);

$articulo= Articulo::where('edad', '>', 100)->firstOrFail();

$articulo= Articulo::whereEdad(100)->get();  //es más eficiente especificar las columnas Escritor::select('id','name')->whereEdad(100)->get()

$ArticuloUlti=Articulo::latest('created_at')->get();   //muestra los últimos que se han añadido


//insertar Objeto en BD
        $articulo = new Articulo();
        $articulo->titulo = 'Mi libro Laravel';
        $articulo->descripcion = 'Libro donde se habla de Laravel';
        $articulo->save();

//actualizar
        $articulo = Articulo::find(1);  //$escritor = App/Escritor::find(1);
        $articulo->titulo = 'Nuevo Laravel';     //$request->name;
        $articulo->save();

//borrar
        $articulo= Articulo::find(1);
        $articulo->delete();
```

* `all()` devuelve todos los elementos de una tabla.

* `find($id)` coge una id y devuelve un sólo modelo. Si no existe un modelo coincidente, devuelve null.

* `findOrFail($id)` toma una idy devuelve un sólo modelo. Si no existe un modelo coincidente, arroja un error1 404.

* `first()` devuelve el primer registro encontrado en la base de datos. Si no existe un modelo coincidente, devuelve null.

* `firstOrFail()` devuelve el primer registro encontrado en la base de datos. Si no existe un modelo coincidente, arroja un error1 404.

* `get()` devuelve una colección de modelos que coinciden con la consulta.

* `findOrCreate()` en el caso que no existe en la BD, lo crea.

```php
$dato=Articulo::firstOrCreate(['titulo'=>'Principito','descripcion'=>'Mi primer articulo']);
```

https://laravel-news.com/firstornew-firstorcreate-firstor-updateorcreate

* `toArray()` convierte el modelo / colección en una matriz PHP simple.

* `Pluck()` en caso de obtener una colección y filtrar la parte que nos interesa.

```php
$users = User::all(); 
// Devuelve una colección de usuarios, por ejemplo, ['name' => 'John', 'age' => 55],['name' => 'Melissa', 'age' => 18],['name' => 'Bob', 'age' => 43]

$usernames = $users->pluck('name'); 
// tenemos una colección de ['John', 'Melissa', 'Bob', 'Sara']
```

*** ¿Cúal forma es mejor o más óptima? ¿SQL NATIVE o QUERY BUILDER o ELOQUENT?  ***

https://www.laraveltip.com/que-es-mejor-eloquent-query-builder-o-sql/

## PAGINAR

*** en el CONTROLADOR de index ***
```php
  $datos=Dato::latest()->paginate(3);
```

*** en la VISTA debajo de forelse ***
```php
//en el momento que insertamos datos dentro de la tabla y queremos mostrar los datos de forma más humana.
<ul>
  @forelse ($datos as $item)
  
    <li>  {{$item->title}}  {{$item->created_at->diffForHumans() }} 
     </li>  
  
  @empty
     <li>NO HAY NADA </li>
  @endforelse
</ul>

{{$datos->links()}}  //DEBAJO DEL FORELSE
```

!!!warning "¿Qué ocurre si vemos las flechas enormes?"
    https://stackoverflow.com/questions/64002774/laravel-pagination-is-showing-weird-arrows


## CRUD

Creas la tabla

`php artisan make:migration create_blogs_table --create=blogs`


Luego en la migración creas los campos para la base de datos

```php
Schema::create('blogs', function (Blueprint $table) {
$table->bigIncrements('id');
$table->string('blog_title');
$table->text('blog_content');
$table->timestamps();
});
```

Para generar las rutas podemos hacer dos cosas:


1. Generarlas automáticamente con el nombre de las rutas en inglés.


Generas las rutas insertando el fichero Web

```php
use App\Http\Controllers\PhotoController;
..............
Route::resource('photos', PhotoController::class);
```

Para listar las rutas, en terminal 
`php artisan route:list -v`


*** Creamos Controlador  ***

`php artisan make:controller photoController --resource`


1.index()

2.create()

3.store()

4.show()

5.edit()

6.update()

7.destroy()

_____

2. Diseñar nuestras rutas y controlador en castellano uno a uno

```php
//Diseñamos nuestras rutas en Web
use App\Http\Controllers\DatosController;

..............

Route::get('/index', [DatosController::class, 'index'])->name('index');
Route::post('/almacenar', [DatosController::class, 'almacenar'])->name('almacenar');
Route::get('/editar/{id}', [DatosController::class,'editar'])->name('editar');
Route::patch('/actualizar/{id}', [DatosController::class,'actualizar'])->name('actualizar');
Route::delete('/borrar/{id}', [DatosController::class,'borrar'])->name('borrar');
Route::view('/insertar','insertar')->name('insertar');
```

Diseñamos la migración y el modelo, en caso de necesitarlo

`php artisan make:migration create_datos_table --create=datos`


Creamos la migracion que es la tabla en la base de datos "datos"

```php
Schema::create('datos', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('nombre');
            $table->text('descripcion');
            $table->timestamps();
        });
```

Creamos el modelo "Dato" que es la Entidad que vamos a trabajar y dentro ponemos los datos que queremos proteger.

`php artisan make:model Dato`

```php
protected $table= 'datos';
protected $fillable = ['nombre', 'descripcion']; //¿Qué atributos deberías incluir en dicho array?

//Aquellos que consideres sensibles y que no deberían modificarse o asignarse en cierta forma, el más típico es el id, el cual normalmente no se modifica ni se asigna de forma manual.
}
```

### INDEX-MOSTRAR DATOS

*** en el CONTROLADOR ***

```php
 public function index(){

    $datos=Dato::all();

    return view('index',['datos'=>$datos]);  //view('index',compact('datos');
   } 
```

*** en la VISTA ***
```php

@forelse ($datos as $item)
<li> 
{{$item->nombre}} {{$item->descripcion}} 
     <a href="{{ route('editar', $item->id)}}" >Edit</a> <!--añadimos también EDITAR-->
     <form action="{{ route('borrar', $item->id)}}" method="post"> <!--añadimos también BORRAR-->
        @csrf
        @method('DELETE')
        <button type="submit">borrar</button>
    </form>
</li>
@empty
   <li>NO HAY NADA </li>
@endforelse
```

### INSERTAR -STORE


*** En el CONTROLADOR ***

```php
 public function almacenar(Request $request){

//LA MEJOR FORMA DE INSERTAR DATOS ya que se hace la comprobación de los campos obligatorios para que no hagan inyeccion y luego inserta.
    $datos=request()->validate([
        'nombre'=>'required|max:25', 
        'descripcion'=>'required']
    );

    Dato::create($datos);

    return redirect()->route('index');
    //return back(); //te redirecciona a la misma página
   } 
```

 *** En la VISTA ***
```php
@if ($errors->any())  //esto muestra todos los errores seguidos
    <ul>
        @foreach ($errors->all() as $error)
        <li>{{ $error }}</li>
        @endforeach
    </ul>
<br/>
@endif

<form method="post" action="{{ route('almacenar') }}"> 
 
        @csrf
        <label for="name">Nombre:</label>
        <input type="text" name="nombre" value="{{old('nombre')}}"/>  
        <!--ponemos old porque en el caso que carguemos el formulario y diera error habría que volver a introducir todos los campos, así recuerda o guarda los campos que están bien-->
     {!! $errors->first('name', '<small>:message</small><br>' )!!}  <!-- así especificamos los errores debajo-->
  
        <label for="price">Descripcion:</label>
        <input type="text" name="descripcion" value="{{old('descripcion')}}"/>
     {!! $errors->first('descripcion', '<small>:message</small><br>' )!!}  
 

    <button type="submit">Crear</button>
</form>
```

### EDITAR


*** EN EL CONTROLADOR ***

```php
public function editar($id){

    $dato = Dato::findOrFail($id);  //como no está el dato si nos equivocamos de id nos muestra la página de error 404, podemos crear uno personalizado en la view->errors->404.blade.php , creamos carpeta "errors"
    return view('editar', compact('dato'));
   } 
```

*** EN LA VISTA para EDITAR ***

```php
@if ($errors->any())
        <div class="alert alert-danger">
            <strong>Whoops!</strong> There were some problems with your input.<br><br>
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

<form method="post" action="{{ route('actualizar', $dato) }}">  
//No es necesario especificar el Id ya que la nueva versión laravel ya busca dentro del Objeto
          <div> 
              @method('PATCH')
              @csrf
              
              <label for="name">nombre:</label>
              <input type="text"  name="nombre" value="{{ $dato->nombre }}"/>
          </div>
          <div >
              <label for="descripcion">descripcion</label>
              <input type="text"  name="descripcion" value="{{ $dato->descripcion }}"/>
          </div>
         
          <button type="submit" >Actualizar</button>
      </form>
  </div>
</div>
```


***  En el CONTROLADOR para ALMACENAR DATOS ya MODIFICADOS ***

```php
public function actualizar(Request $request)
    {
        $validacion = $request->validate([
            'nombre' => 'required|min:1|max:10',
            'descripcion' => 'required',
        ]);

        Dato::whereId($request->id)->update($validacion); //otra opción

        /*  //otra forma de almacenar
         $datos = Dato::find($id);   //podremos utilizar findOrFail($id) para que en caso de no encontrar no falle
         $datos->nombre = $validacion['nombre'];
         $datos->descripcion = $validacion['descripcion'];
         $datos->update();*/

        return redirect()->route('index');
}

// formar de recuperar datos de un formulario. $request->get('nombre'); $request->nombre;  $request->input('nombre');
```

### BORRAR

*** En el controlador Borrar ***

```php
    public function borrar($id){

        $dato = Dato::findOrFail($id);
        $dato->delete();
        return redirect()->route('index');
       } 
```

## RELACIÓN UNO A UNO (1X1)

Crearemos la tabla Usuario y Teléfono. El modelo de User puede estar asociado con un modelo de teléfono. Para definir esta relación, colocaremos un método de phone en el modelo User. El método de phone debe llamar al método hasOne y devolver su resultado.

```php
//tabla users 
Schema::create('users', function (Blueprint $table) {

 $table->increments('id');

 $table->string('name');

 $table->string('email')->unique();

 $table->timestamps();

 });

```

```php

//Crear Tabla de Migración de Teléfonos con Clave Foránea
Schema::create('phones', function (Blueprint $table) {

 $table->increments('id');

 $table->integer('user_id')->unsigned();

 $table->string('phone_no');

 $table->timestamps();

 $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade'); 

});

```

***  Modelo de usuario: ***

```php

 namespace App\Models;

 use Illuminate\Database\Eloquent\Model;

 class User extends Model 
{
 /**
 * Get the phone associated with the user.
 */
 public function phone()
 {
 return $this->hasOne(Phone::class);
 } 
}

```
*** Modelo de teléfono ***

Podemos acceder al modelo de Phone desde nuestro modelo de User. A continuación, definamos una relación en el modelo Phone que nos permitirá acceder al usuario propietario del teléfono. Podemos definir el inverso de una relación hasOne usando el método belongsTo.

```php

namespace App\Models; 

use Illuminate\Database\Eloquent\Model; 

class Phone extends Model 
{
 /**
 * Get the user that owns the phone.
 */
 public function user()
 {
 return $this->belongsTo(User::class);
 //return $this->belongsTo(User::class, 'foreign_key');   
//Si la clave externa en el modelo de Phone no es user_id, puede pasar un nombre de clave personalizado
 } 
}

```

*** Recuperar registros usando el modelo: ***

Una vez que se define la relación, podemos recuperar el registro relacionado usando las propiedades dinámicas de Eloquent. Entonces, aquí podemos usar el modelo de usuario con función de teléfono.

```php

$phone = User::find(1)->phone;

$user = Phone::find(1)->user;

```

***  Crear registros usando el modelo ***

```php
$user = User::find(1);
$phone = new Phone; 
$phone->phone_no = '9876543210';
$user->phone()->save($phone);
```

```php
$phone = Phone::find(1);
$user = User::find(1);
$phone->user()->associate($user)->save();
```



## RELACIÓN UNO A MUCHOS (1XM)


Una categoria tiene muchos artículos. Creamos primero las migraciones (bases datos) y luego el modelo (Entidades), también tenemos la opción de crear el modelo y luego automaticamente las migraciones pero hay que tener cuidado ya que genera el nombre de las tablas de la migración en inglés.

`php artisan make:migration create_categorias_table --create="categorias"`


`php artisan make:migration create_articulos_table --create="articulos"`


*** Tablas migraciones ***

```php
//tabla Categories
  Schema::create('categorias', function (Blueprint $table) {
            $table->increments('id')->unsigned();
            $table->text('nombre');
            $table->mediumText('masInfo');
            $table->timestamps();
        });

//tabla articulos

    Schema::create('articulos', function (Blueprint $table) {
            $table->increments('id')->unsigned();
            $table->integer('categoria_id')->unsigned();  //si hubieramos puesto categorias_id tambien hubiera cogido la relación
            $table->text('titulo');
            $table->mediumText('descripcion');
            $table->timestamps();
            //relaciones
            $table->foreign('categoria_id')->references('id')->on('categorias')->onDelete('cascade')->onUpdate('cascade');
            //category_id hace referencia a la tabla categories y el borrado y la actualización es en cascada.
        });
```

*** Creamos los modelos ***

`php artisan make:model Categoria`

```php
//Modelo Categoria
class Categoria extends Model
{
    protected $table = 'categorias';

    //protected $primaryKey = 'categoria_id';  
    //Por defecto id es la clave primaria, podemos definir un campo diferente con la propiedad primaryKey

    //public $timestamps = true;  
    //especificamos si queremos crear created_at y updated_at en la base de datos
    protected $fillable = ['nombre', 'masInfo'];

    public function articulos()
    {
        return $this->hasMany('App\Models\Articulo');
    }
}


//Modelo Artículos

class Articulo extends Model
{
    protected $table = 'articulos';
    protected $fillable = ['titulo', 'descripcion'];

    public function categoria()
    {
        return $this->belongsTo('App\Models\Categoria');
    }
}
```

*** En el controlador ***

```php
public function insertar()
    {
        $categoria = Categoria::find(3);   //buscamos que ya tenemos almacenada o la creamos.

        $articulo = new Articulo(['titulo' => 'Pedazo Artículo', 'descripcion' => 'toda la descripción']);  //creamos artículo.
        $categoria->articulos()->save($articulo);  //guardamos dentro de categoria un artículo.
       
       //en caso que tengamos varios artículos
       /* $categoria->articulos()->saveMany([
            new Articulo(['titulo' => 'A new comment.', 'descripcion' => 'hola']),
            new Articulo(['titulo' => 'Another comment.',  'descripcion' => 'hola']),
        ]);*/
        
    }
```

http://documentacion-laravel.com/eloquent-relationships.html#insertando-y-actualizando-modelos-relacionados


*** PROBLEMA N+1 - LAZY LOADING ***

Muestra el nombre de todas las categorias de todos los artículos. Una consulta para obtener todos los artículos en la tabla, después otra consulta para cada articulos para obtener quien es el autor

```php
 $articulos = Articulo::all();

 foreach ($articulos as $articulo) {
     dump($articulo->categoria->nombre);
   }
```

***  SOLUCIÓN - EAGER LOADING  ***

Al usar la carga previa o carga impaciente (Eager Loading) para reducir esta operación a solo 2 consultas

```php
$articulos = Articulo::with('categoria')->get();

 foreach ($articulos as $articulo) {
           echo $articulo->categoria->nombre ."</br>";
        }
```

https://styde.net/lazy-loading-vs-eager-loading/


*** BORRADO ***

Borra la categoria 9 y todos los articulos relacionados (cascada)

```php
$Categoria = Categoria::find(9);
$Categoria->delete();

//borra artículo
$articulo = Articulo::find(2);        
$articulo->delete();
```

## RELACION DE MUCHOS A MUCHOS (NXM)


Un post puede tener muchas Etiquetas y una etiqueta puede tener muchos Posts, por tanto habrá que crear una tabla auxiliar.


`php artisan make:migration create_etiquetas_table --create="etiquetas"`

`php artisan make:migration create_posts_table --create="posts"`

`php artisan make:migration create_etiqueta_post_table --create="etiqueta_post"`


Dentro de la tabla de migraciones

*** ETIQUETAS ***

```php
Schema::create('etiquetas', function (Blueprint $table) {
            $table->increments('id');

            $table->string('nombre',128);
            $table->string('slug',128);

            $table->timestamps();
        });
```
*** POSTS ***

```php
   Schema::create('posts', function (Blueprint $table) {
            $table->increments('id');

             $table->string('nombre',128)->unique();
             $table->string('slug',128);
             $table->text('cuerpo');
             $table->enum('status',['PUBLISHED','DRAFT'])->default('DRAFT');
             $table->string('file', 128)->nullable();
 
             $table->timestamps();
             
        });
```

*** ETIQUETA_POST ***
```php
 Schema::create('etiqueta_post', function (Blueprint $table) {
            $table->increments('id');

            $table->integer('post_id')->unsigned();
            $table->integer('etiqueta_id')->unsigned();

            $table->timestamps();

             //relaciones
             $table->foreign('post_id')->references('id')->on('posts')->onDelete('cascade')->onUpdate('cascade');
             $table->foreign('etiqueta_id')->references('id')->on('etiquetas')->onDelete('cascade')->onUpdate('cascade');
        });
```

Dentro de los modelos

```php
//Modelo POST
class Post extends Model
{
    protected $table = 'posts';
    //protected $primary_key = 'categoria_id';
    protected $fillable = ['nombre', 'slug', 'cuerpo', 'status' ,'file'];

    public function etiquetas()
    {
        return $this->belongsToMany('App\Models\Etiqueta');
    }
}

//Modelo ETIQUETA

class Etiqueta extends Model
{
    protected $table = 'etiquetas';
    //protected $primary_key = 'categoria_id';
    protected $fillable = ['nombre', 'slug'];

    public function posts()
    {
        return $this->belongsToMany('App\Models\Post');
    }
}

//INSERTAR Tabla intermedia

public function insertarTablaIntermedia(){
        $etiqueta=Etiqueta::find(1);
        $post = Post::find(2);
        $post->etiquetas()->attach($etiqueta);
       }
```


*** OBTENER DATOS DE LA otra TABLA RELACIONADA. ***

```php
return Post::find(1)->etiquetas()->get();  
//del Post(1) obtiene todas las etiquetas relacionadas

return Post::find(1)->etiquetas()->where('nombre','futbol')->get();  
//del Post(1) obtiene la etiqueta relacionada que tiene nombre = "futbol"

```

*** CONSULTAS EN MULTIPLES TABLAS ***

Ejemplo:

Tablas: a, b, c, d, e, f Quiero traer los datos de todas las tablas relacionadas, los datos de a, b, c, d,e,f. A está relacionado con B (1,n) B con C (1,n) C con D (1,n) D con E (1,n) E con F (1,n) Solución:

 https://es.stackoverflow.com/questions/173727/consultas-en-m%C3%BAltiples-tablas-con-laravel-5-5

## AUTENTIFICACIÓN (LOGIN - REGISTRO)


https://github.com/laravel/ui

1. Primero Instalamos (copiamos las rutas de web ya que se borran )

`./vendor/bin/sail composer require laravel/ui`

2. `php artisan ui bootstrap --auth`

3. `npm install`
4. `npm run dev`

5. Después si accedemos dentro routes->Web podemos ver que se ha creado `Auth::routes()`;

6. Para cambiar la redirección después de Logearnos, tendremos que ir dentro ***app->http->Controller->Auth*** cambiamos las rutas de register y login cambiando ruta a /
   
7. Quitamos la ruta **/home** dentro de las rutas en web


*** MUESTRA USUARIO BLADE ***

```python
@auth    <!--sirve para mostrar en caso de estar autentificado, estar logeado-->
{{auth()->user()->name}}
@endauth

<!--tambien cambiar la ruta de middleware en caso de estar autentificado y 
volver hacer login peta y te manda a home por tanto cambiar a raiz.-->

@if(auth()) <!--si el usuario está autentificado-->

@endif
```

*** SABER SI ESTÁ LOGEADO EN EL CONTROLADOR ***

```php
use Illuminate\Support\Facades\Auth;

if(Auth::check()){ 
 return "estas logeado";
}else { 
 return "no estas logeado";
} 

// Obtiene el objeto del Usuario Autenticado
$user = Auth::user();
$user->name;
```

*** SECURIZAR ***


También podemos indicarlo directamente en el constructor de un controlador 

```php
public function __construct()
{
$this->middleware('auth');
}
```

Otra forma es indicándolo en la ruta para protegerla 

```php
Route::get('/resultado', 'PruebasController@ejemplo')->middleware('auth);
```

*** LOGOUT ***

```php
@guest 
<a href="{{route('login')}}">Login</a>
@else   
<a href="{{ route('logout') }}" onclick="event.preventDefault(); document.getElementById('logout-form').submit();"> Logout </a>

  <form id="logout-form" action="{{ route('logout') }}" method="POST" style="display: none;">
     @csrf
  </form>
@endguest
```

*** DESHABILITAR REGISTRO ***

vamos a Web y modificamos ruta

```php
Auth::routes(['register'=>false]);
```

***  BLOQUEAR USUARIO, INTENTOS FALLIDOS ***

En el caso que querramos bloquear al usuario por intentos de ingresos fallidos y penalizar el tiempo a la hora de volver autenficarte,
bastará con añadir el controlador por defecto  ***app->http->Controller->auth-> LoginController***

```php
class LoginController extends Controller
{
   
    use AuthenticatesUsers;

    public $maxAttempts = 2;   //número máximo de intentos
    public $decayMinutes = 2;  //tiempo que durará el bloqueo
```


## TRY ... CATCH 

*** Las Rutas ***

```php
Route::get('/formularioBusqueda', [PeliculasController::class, formulario'])->name('formularioBusqueda');   
Route::post('/product/resultado', [ProductController:class, 'resultado'])->name('resultadoBusqueda');
```

*** En la vista 'formularioBusqueda' ***
```php

@if (session('error'))
<div>{{ session('error') }}</div>   <!--En caso de no encontrar el producto en la BBDD muestra error-->
@endif
 
<form action="{{ route('resultadoBusqueda') }}" method="POST">
@csrf

<input id="title name="title" type="text" value="{{ old('product') }}" placeholder="Product Title">

<input type="submit" value="Search">
</form>
```

***  En el controlador ***
```php

public function resultado(Request $request)
{
    try {
        $product = Product::where('title',$request->get('title'));  //busca el producto en la BBDD
    } catch (ModelNotFoundException $exception) {
        return back()->withError($exception->getMessage())->withInput();   //en caso de no encontrar en la BBDD captura la excepción y envia la info a la misma vista 
        //return $exception->getMessage();

    }
    return view('resultadoBusqueda', compact('product'));   //en caso de encontrar el producto manda otra vista el resultado
}
```
