---
sidebar_position: 1
---

# Controladores Monolíticos

Los contronladores Monolíticos tienen fines orientados a los dueños de la aplicación y desarrolladores, esto, para acceder a todos los modulos, sin limitaciones ni restricciones.

### Empresa controller

este controlador es el responsable de realizar todas las operaciones relacionadas con la empresa

#### Metodos

##### Constructor

este método constructor tiene como misión hacer de middleware para que solo los usuarios con dicho rol y/o permiso puedan acceder a las distintas acciones del controlador. cada metodo tiene sus propios permisos

```php

      function __construct()
    {
         $this->middleware('permission:empresa-list|empresa-create|empresa-edit|empresa-delete', ['only' => ['index','store']]);
         $this->middleware('permission:empresa-create', ['only' => ['create','store']]);
         $this->middleware('permission:empresa-edit', ['only' => ['edit','update']]);
         $this->middleware('permission:empresa-delete', ['only' => ['destroy']]);
    }
```

##### Index

el fin de este metodo es retornar todos los registros que se encuentran dentro de la tabla empresa
si la respuesta es positiva el resultado es la vista empresa.index , la cual contiene una tabla donde estan las acciones disponibles para cada uno de los registros (eliminar, modificar y detalles). Por fuera de la tabla se pueden crear las empresas

```php

    public function index()
    {
        $user = Empresa::all();
        $data = Empresa::paginate(5);
        return view('empresas.index', compact('data'));
    }
```

##### Store

Este método se da a partir de la necesidad de poder crear empresas desde el inicio del sistema, este devuelve la vista empresa Create

```php

    public function store(Request $request)
    {
        $empresa =  request()->except('_token');
             Empresa::insert($empresa);
             return redirect('empresas')->with('msg', 'empresa creada correctamente');
    }

```

El request que se envía debe ser así, con estos datos se crea la empresa omitiendo el "_token" 

```JSON

    {
        "_token": "tuREi3AdI4BjZqsxYVSz1foJKRku44felYCNZhQM",
        "nombre": "bitsoluciones.sa",
        "nit": "900556990-8",
        "web": "bitsoluciones.net",
        "estado": "1"
    }

```
##### Show 

La funcionalidad de este método es mostrar una empresa especifica en caso de que se necesite, 
Parametro: recibe el id, posteriormente se busca una empresa que coincida con el id enviado por el  parametro y por ultimo devuelve la vista empresas/show con la información de la empresa 
```PHP
   public function show(string $id)
    {
        $empresa = Empresa::find($id);
        return view('empresas.show', compact('empresa'));
    }
```

###### Edit 

Este método al es igual a show por que  busca una empresa especifica la diferencia esta en que este es utilizado para modificarse luego, mientras que el primero solo dejo ver los detalles 

```PHP
    public function edit(string $id)
    {
        $empresa = Empresa::find($id);
        $estados = Estado::pluck('descripcion', 'id');

        return view('empresas.edit', compact('empresa','estados'));
    }

```

##### update 

El método update es la continuación del metodo edit, solo que este solo se ejecuta cuando se da click en el botón eliminar 

```PHP
    public function update(Request $request, string $id)
    {
        $empresa = $request->except(['_token', '_method']);
         Empresa::where('id', '=', $id)->update($empresa);
        
         return redirect('empresas')->with('msg', 'empresa actualizada correctamente');

    }

```

###### Destroy 

Este mótodo se encarga de elminiar de forma lógica la empresa, cambiando su estado da activo a inactivo 

```PHP 
    public function destroy(string $id)
    {
        $empresa = Empresa::where('id', $id);
        $empresa->estado = 2;
        $empresa->save();
        return redirect('empresas')->with('msg', 'empresa eliminada correctamente');
    }

```

### Etiqueta Controller 

Esta al igual que el controlador empresa es un controlador de tipo resource que en pocas palabras quiere decir, que es un contralodor que hace un CRUD

#### Metodos 

* Index 
* Store
* Show 
* Edit 
* Update 
* Delete 

estan agrupados de esta forma ya que los metodos son exactamente los mismos, lo unico que cambia es el modelo 

### Mision Controller 

este controlador tambien es un controlador de tipo resource. Y tiene unos metodos adicionales por lo que hablare de ellos para evitar documentar lo mismo 

#### Metodos 

* Index 
* Store
* Show 
* Edit 
* Update 
* Delete 

##### AddEtiquetas


Este método es la continuación del método *Store*, ya que al guardarse la misión inmediatamente se ejecuta este, el propósito de este método es retornar la vista addEtiquetas y filtrar las etiquetas que se disponibles del modelo anteriormente seleccionado 

```PHP
    public function addEtiquetas($mision)
    {

        $modelo = $mision->modeloId;
        $mision = $mision->id;

        $etiquetas = DB::table('etiquetas')
            ->where('modeloId', $modelo)
            ->get();

        return view('mision.addEtiquetas', compact('etiquetas', 'mision', 'modelo'));
    }

```

#####