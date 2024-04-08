---
sidebar_position: 1
---

# Migraciones

Tablas de la base de datos y sus relaciones


## Estado 


los campos que tiene son los siguientes:

* Id
* Descripcion
* Timestamps (Crea dos campos [created_at - updated_at])


```php
    public function up(): void
    {
        Schema::create('estado', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('descripcion', 50)->nullable();
        });
    }

```

## Empresa

los campos que tiene son los siguientes:

* Id
* Nombre
* Nit
* Web
* EstadoId
* Timestamps (Crea dos campos [created_at - updated_at])


```php
      public function up(): void
    {
        Schema::create('empresa', function (Blueprint $table) {
            $table->id();
            $table->string('nombre', 50);
            $table->string('Nit', 20);
            $table->string('web', 50);
            $table->unsignedBigInteger('estadoId');
            $table->timestamps(); 
            $table->foreign('estadoId')->references('id')->on('estado');
        });
    }

```

### Relaciones 

* Tiene una relacion de 1:1 con estado

## Productos

los campos que tiene son los siguientes:

* Id
* Nombre
* ProductoId
* ModeloId
* Timestamps (Crea dos campos [created_at - updated_at])


```php
    public function up(): void
    {
        Schema::create('etiquetas', function (Blueprint $table) {
            $table->id();
            $table->string('nombre',50)->nullable();
            $table->unsignedBigInteger('modeloId');
            $table->unsignedBigInteger('productoId')->nullable();
            $table->foreign('modeloId')->references('id')->on('modelos')->onDelete('cascade')->onUpdate('cascade');
            $table->foreign('productoId')->references('id')->on('productos')->onDelete('cascade')->onUpdate('cascade');
            $table->timestamps();
        });
    }

```
### Relaciones 

* Tiene una relacion de n:1 con modelos
* Tiene una relacion de 1:1 con productos

## Users

los campos que tiene son los siguientes:

* Id
* Username
* Password
* Regional
* estado
* empresaId
* Regional
* RememberToken (Este es importante para la implementacion de la api de usuarios)
* Timestamps (Crea dos campos [created_at - updated_at])


```php

    public function up(): void
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('username', 50)->nullable();
            $table->string('password', 255)->nullable();
            $table->string('regional', 100)->nullable();
            $table->string('telefono',15)->nullable();
            $table->string('email', 100)->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->rememberToken();
            $table->unsignedBigInteger('estado')->nullable();
            $table->timestamps();
            $table->unsignedBigInteger('empresaId')->nullable();
            $table->foreign('empresaId')->references('id')->on('empresa')->onDelete('cascade')->onUpdate('cascade');
            $table->foreign('estado')->references('id')->on('estado')->onDelete('cascade')->onUpdate('cascade');
        });
    }


```

### Relaciones 

* Tiene una relacion de n:1 con empresa
* Tiene una relacion de 1:1 con estado

## Modelos

los campos que tiene son los siguientes:

* Id
* nombre
* descripcion
* userId
* Timestamps (Crea dos campos [created_at - updated_at])

### Relaciones 

* Tiene una relacion de 1:1 con users

```php

    public function up(): void
    {
        Schema::create('modelos', function (Blueprint $table) {
            $table->id();
            $table->string('nombre',50);
            $table->string('descripcion',250);
            $table->unsignedBigInteger('userId');
            $table->foreign('userId')->references('id')->on('users')->onDelete('cascade')->onUpdate('cascade');
            $table->timestamps();
        });
    }

```

## Mision

los campos que tiene son los siguientes:

* Id
* nombre
* descripcion
* dias
* estado
* image
* fechaInicial
* fechaFinal
* semanas
* Obligatorio
* userId
* modeloId
* Timestamps (Crea dos campos [created_at - updated_at])
* Step, este step se agrego a partir de una migracion llamada add_step_to_misiones
    este campo solo se actualiza por medio del frontend, ya que mediante este campo se sabra en que paso quedo y de esta forma se podra finalizar 

```php

       public function up(): void
    {
        Schema::create('mision', function (Blueprint $table) {
            $table->id();
            $table->string('nombre',50)->nullable();
            $table->string('descripcion',250)->nullable();
            $table->integer('dias')->nullable();
            $table->unsignedBigInteger('estado')->nullable();
            $table->boolean('image')->nullable();
            $table->string('fechaInicial',50)->nullable();
            $table->string('fechaFinal',50)->nullable();
            $table->integer('semanas')->nullable();
            $table->integer('Obligatorio')->nullable();
            $table->unsignedBigInteger('userId')->nullable();
            $table->unsignedBigInteger('modeloId');
            $table->foreign('modeloId')->references('id')->on('modelos');
            $table->foreign('userId')->references('id')->on('users');
            $table->foreign('estado')->references('id')->on('estado');
            $table->timestamps();
        });
    }

```
### Relaciones 

* Tiene una relacion de 1:1 con users
* Tiene una relacion de 1:1 con estado
* Tiene una relacion de 1:1 con modelos


## Logica

los campos que tiene son los siguientes:

* Id
* logica
* expresion
* estado
* Timestamps (Crea dos campos [created_at - updated_at])

```php

    public function up(): void
    {
        Schema::create('logica', function (Blueprint $table) {
            $table->id();
            $table->string('logica');
            $table->string('expresion');
            $table->unsignedBigInteger('estado')->nullable();
            $table->timestamps();
            $table->foreign('estado')->references('id')->on('estado');
        });
    }

```

### Relaciones 

* Tiene una relacion de 1:1 con estado

## Detalles Misiones

los campos que tiene son los siguientes:

* Id
* misionId
* modeloId
* EtiquetasId
* Timestamps (Crea dos campos [created_at - updated_at])

```php

    public function up(): void
    {
        Schema::create('detallesMisiones', function (Blueprint $table) {
            $table->id();
            $table->unsignedBigInteger('misionId');
            $table->unsignedBigInteger('modeloId');
            $table->unsignedBigInteger('etiquetasId');
            $table->foreign('misionId')->references('id')->on('mision');
            $table->foreign('modeloId')->references('id')->on('modelos');
            $table->foreign('etiquetasId')->references('id')->on('etiquetas');
            $table->timestamps();
        });
    }

```
### Relaciones 

* Tiene una relacion de 1:1 con misionId
* Tiene una relacion de 1:1 con modelos

## LogDetalleMisiones

los campos que tiene son los siguientes:

* Id
* detalleMisionId
* logId
* operacion
* Timestamps (Crea dos campos [created_at - updated_at])

```php

    public function up(): void
    {
        Schema::create('logDetallesMisiones', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->unsignedBigInteger('detalleMisionId');
            $table->unsignedBigInteger('logId');
            $table->string('operacion');
            $table->foreign('logId')->references('id')->on('logica');
            $table->foreign('detalleMisionId')->references('id')->on('detallesMisiones');
        });
    }


```
### Relaciones 

* Tiene una relacion de 1:1 con detalleMision
* Tiene una relacion de 1:1 con logica


## image

los campos que tiene son los siguientes:

* Id
* nombre
* procesado
* userId
* clienteId
* url 
* fechaCaptura
* fechaCarga
* misionId
* codBodegas
* nombreCliente
* nombreVendedor
* nombreBodega
* empresaId
* width
* height
* week
* numdoc
* aplicaIA
* estado
* configMovilId
* fechaProcesado
* usuarioActualiza
* fechaUsuarioActualiza
* Timestamps (Crea dos campos [created_at - updated_at])

```php

    public function up(): void
    {
        Schema::create('image', function (Blueprint $table) {
            $table->id();
            $table->string('nombre', 100);
            $table->integer('procesado')->nullable();
            $table->unsignedBigInteger('userId')->nullable();
            $table->unsignedBigInteger  ('clienteId')->nullable();
            $table->string('url', 200)->nullable();
            $table->dateTime('fechaCaptura')->nullable();
            $table->dateTime('fechaCarga')->nullable();
            $table->unsignedBigInteger('misionId')->nullable();
            $table->string('codBodega', 50)->nullable();
            $table->string('nombreCliente', 100)->nullable();
            $table->string('nombreVendedor', 50)->nullable();
            $table->string('nombreBodega', 100)->nullable();
            $table->unsignedBigInteger('empresaId')->nullable();
            $table->integer('width')->nullable();
            $table->integer('height')->nullable();
            $table->integer('week')->nullable();
            $table->string('numdoc', 80)->nullable();
            $table->integer('aplicaIA')->nullable();
            $table->unsignedBigInteger('estado')->nullable();
            $table->unsignedBigInteger('configMovilId')->nullable();
            $table->dateTime('fechaProcesado')->nullable();
            $table->integer('usuarioActualiza')->nullable();
            $table->dateTime('fechaUsuarioActualiza')->nullable();
            $table->timestamps();
            $table->foreign('userId')->references('id')->on('users');
            $table->foreign('misionId')->references('id')->on('mision');
            $table->foreign('estado')->references('id')->on('estado');
        });
    }

```
### Relaciones 

* Tiene una relacion de 1:1 con estado
* Tiene una relacion de 1:1 con users
* Tiene una relacion de N:1 con mision

## Coordenadas

los campos que tiene son los siguientes:

* Id
* imageId
* yMin
* yMin
* yMax
* xMin
* xMax
* classname
* own
* shelf 

```php

public function up(): void
  {
      Schema::create('coordenadas', function (Blueprint $table) {
          $table->id();
          $table->unsignedBigInteger('imageId')->nullable();
          $table->float('yMin')->nullable();
          $table->float('yMax')->nullable();
          $table->float('xMin')->nullable();
          $table->float('xMax')->nullable();
          $table->float('classname')->nullable();
          $table->integer('own')->nullable();
          $table->integer('shelf')->nullable();
          $table->timestamps();
          $table->foreign('imageId')->references('id')->on('image');
      });
  }

```
### Relaciones 

* Tiene una relacion de n:1 con Image

## Veredicto

los campos que tiene son los siguientes:

* Id
* veredictoIa
* VeredictoUser
* ExplicacionIa
* imageId
* explicaiconIdUser

```php

  public function up(): void
  {
    Schema::create('veredictos', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->boolean('veredictoIa')->nullable();
        $table->boolean('veredictoUser')->nullable();
        $table->unsignedBigInteger('explicacionIdIa')->nullable();  
        $table->unsignedBigInteger('imageId')->nullable();  
        $table->unsignedBigInteger('explicacionIdUser')->nullable();  
        $table->foreign('imageId')->references('id')->on('image');
    });
  }


```
### Relaciones 

* Tiene una relacion de 1:1 con Image

## Favorite

los campos que tiene son los siguientes:

* Id
* userId
* imageId

```php
    public function up(): void
    {
        Schema::create('favorite', function (Blueprint $table) {
            $table->id();
            $table->unsignedBigInteger('userId');
            $table->unsignedBigInteger('imageId');
            $table->foreign('userId')->references('id')->on('users');
            $table->foreign('imageId')->references('id')->on('image');
            $table->timestamps();
        });
    }


```
### Relaciones 

* Esta tabla funciona como tabla pivote de tal manera que mostrara los usuarios que le den me gusta a una tabla y viceversa 

