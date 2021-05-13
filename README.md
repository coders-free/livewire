## Apuntes

### 02 - Como se renderizan los componentes de Livewire

### Crear un componente 

Para crear un componente de livewire utiliza el siguiente comando 

- php artisan make:livewire ShowPosts

Esto creara dos archivos, la clase ShowPosts dentro de app/Http/Livewire y la vista show-post dentro de resources/views/livewire/show-posts.blade.php

Para crear componentes dentro de una carpeta deberás utilizar el comando 

- php artisan make:livewire Nav/ShowPosts


### Utilizar un componente 

Para utilizar un componente utiliza 

- livewire('show-posts',['title'=>'Este es un título de prueba']) 

Lo anterior renderizara  el componente y en caso de esperar un parámetro, en este caso title, deberás pasárselo dentro de un arreglo llave-valor, en caso de que tu componente se encuentre de una carpeta deberás especificar la ruta por ejemplo

 - @livewire('nav.show-posts')

 ### Uso de un componente como controlador

 En caso de que sea necesario que utilices un componente que se mostrara como todo el contenido, deberás especificarlo 
 como el parámetro de una ruta 
 
- Route::middleware(['auth:sanctum', 'verified'])->get('/dashboard',ShowPosts::class)->name('dashboard');


 Este renderizara su contenido utilizando a views/layouts/app.blade.php, en caso de que
 necesites que renderize usando otro layout, deberás especificarlo haciendo uso de la función layout dentro de la clase
 del componente por ejemplo 

 - return view('livewire.show-posts')
    ->layout('layouts.base');

### La función mount

La función mount, permite recibir parámetros para después asignarlos a propiedades publicas del componente, por ejemplo 

-  public function mount($name)
    {
        $this->name = $name;
    }

Que estaría asignando el valor recibido, ya sea por url o dentro de la directiva de blade @livewire()

El siguiente es un ejemplo de una ruta que utiliza la función de un componente como Controlador, misma que recibe el parametro name
el cual es usado por la función mount

- Route::get('prueba/{name}',ShowPosts::class);

Para que esto funcione, la clase ShowPosts deberá tener definida como publica la propiedad name como se muestra a continuación

-  public $name;

De esta forma la ruta, recibe la variable name, y la función mount, al recibirla la asigna a su propiedad publica $name,
misma que sera renderizada dentro de la vista del componente usando los {{ }} de la siguiente forma 

- {{ $name }}


### 03 - Vinculación de datos 

Para vincular el valor de un input con el valor de la clase del componente se utiliza 

- wire:model="search"

En donde search es el nombre de la variable de la clase del componente, esto permite que cada ves que dicha variable es  modificada
el componente recargue el contenido de su vista utilizando la función render, es importante entender esto ya que este comportamiento 
podría jugar en nuestro favor o no dependiendo el caso de uso 
