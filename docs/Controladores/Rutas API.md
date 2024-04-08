---
sidebar_position: 4
---

# Rutas de la API

las rutas de la API se dividen por controladores:

La mayoria de las rutas son de tipo resource, lo que quiere decir que se utilizan como crud

los metodos que se usan en este tipo de api son index, store, show, delete y update

## tipos de funciones y los métodos http que manejan

Los métodos que tienen el nombre **index** son de tipo **GET.** Estos traen todos los registros del modelo seleccionado.

Los métodos que tienen el nombre **store** son de tipo **POST.** Estos sirven para crear un nuevo recurso del modelo.

Los métodos que tienen el nombre **show** son de tipo **GET.** Este tipo de endpoint contiene un **parámetro** que es el **id** del recurso, el endpoint quedaría así **$urlBase:$puerto/$recurso/$idRecurso**

Los métodos que tienen el nombre **delete** son de tipo **DELETE.** Este tipo de endpoint tiene la misma estructura que la función **show** la única diferencia es el método http.

Los métodos que tienen el nombre **update** son de tipo **PATCH**. Al igual que la función **show** maneja la misma estructura con diferencia de metodo http.

## Rutas de producto

```php
    Route::apiResource('/productos', ProductoController::class)->names([
        'index' => 'api.productos.index',
        'destroy' => 'api.productos.destroy',
        'store' => 'api.productos.store',
        'update' => 'api.productos.update',
        'show' => 'api.productos.show',
    ]);;
```

## Rutas de empresas

```php
    Route::apiResource('empresas', EmpresaController::class)->names([
        'index' => 'api.empresas.index',
        'destroy' => 'api.empresas.destroy',
        'store' => 'api.empresas.store',
        'update' => 'api.empresas.update',
        'show' => 'api.empresas.show',
    ]);
```

## Rutas de etiquetas

```php
Route::apiResource('etiquetas', EtiquetaController::class)->names([
        'index' => 'api.etiquetas.index',
        'destroy' => 'api.etiquetas.destroy',
        'store' => 'api.etiquetas.store',
        'update' => 'api.etiquetas.update',
        'index' => 'api.etiquetas.index',
        'show' => 'api.etiquetas.show',
    ]);
Route::get('etiquetas/modelo/$modeloId', [EtiquetaController::class, 'getEtiquetaByModelo'])->name('api.etiquetas.getEtiquetaByModelo');
```

La Ruta 'etiquetas/modelo/$modeloId' recibe como parámetro el id del modelo y trae las etiquetas pertenecientes al modelo. 

## Rutas de estado 

```php
    Route::apiResource('estados', EstadoController::class)->names([
        'index' => 'api.estados.index',
        'destroy' => 'api.estados.destroy',
        'store' => 'api.estados.store',
        'update' => 'api.estados.update',
        'index' => 'api.estados.index',
        'show' => 'api.estados.show',
    ]);

```
## Rutas de usuario 

```php 
    Route::apiResource('users', UserController::class)->names([
        'index' => 'api.users.index',
        'destroy' => 'api.users.destroy',
        'store' => 'api.users.store',
        'update' => 'api.users.update',
        'index' => 'api.users.index',
        'show' => 'api.users.show',
    ]);

```
## Rutas de modelos 

```php
    Route::apiResource('modelos', ModeloController::class)->names([
        'index' => 'api.modelos.index',
        'destroy' => 'api.modelos.destroy',
        'store' => 'api.modelos.store',
        'update' => 'api.modelos.update',
        'show' => 'api.modelos.show',
    ]);
    Route::get('modelos-por-usuario/$userId', [ModeloController::class, 'modelsByUsers']);

```

La ruta modelos-por-usuario recibe como parámetro el id del usuario, este trae los modelos pertenecientes a la empresa donde trabaja el usuario. 

## Rutas de coordenadas

```php
    Route::apiResource('coordenadas', CoordenadasController::class)->except(['destroy', 'store', 'update']);

```

## Rutas de misiones 

```php
   Route::controller(MisionController::class)->group(function () {
        Route::get('/misiones', 'index')->name('api.misiones.index');
        Route::post('/misiones', 'store')->name('api.misiones.store');
        Route::get('/misiones/$id', 'show')->name('api.misiones.show');
        Route::patch('/misiones/$id', 'update')->name('api.misiones.update');
        
        
        Route::get('/misiones-usuario-estado',[MisionController::class, 'missionByUserAndState'])->name('api.misiones.missionByUserAndState');
        Route::get('/mision-created/$id', 'misionCreated')->name('api.misiones.misionCreated');
        Route::delete('/misiones/$id/$operación', 'destroy')->name('api.misiones.delete');
        Route::post('misiones/$stepBack/$id', [MisionController::class, 'destroyByStepBack'])->name('api.misiones.destroyByStepBack');
    });

```
### Misiones-Usuario-estado

Este endpoint se esta utilizando para la aplicación movil. y para su utilización se manejan dos querys de url que son las siguientes 
* userId = el id del usuario que se quiere consultar 
* estado = el estado en el que se encuentra la misión  



Create a Markdown file at `docs/hello.md`:

```md title="docs/hello.md"
# Hello

This is my **first Docusaurus document**!
```

A new document is now available at [http://localhost:3000/docs/hello](http://localhost:3000/docs/hello).

## Configure the Sidebar

Docusaurus automatically **creates a sidebar** from the `docs` folder.

Add metadata to customize the sidebar label and position:

```md title="docs/hello.md" {1-4}
---
sidebar_label: "Hi!"
sidebar_position: 3
---

# Hello

This is my **first Docusaurus document**!
```

It is also possible to create your sidebar explicitly in `sidebars.js`:

```js title="sidebars.js"
export default {
  tutorialSidebar: [
    "intro",
    // highlight-next-line
    "hello",
    {
      type: "category",
      label: "Tutorial",
      items: ["tutorial-basics/create-a-document"],
    },
  ],
};
```
