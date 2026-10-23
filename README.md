[README - CRUD.md](https://github.com/user-attachments/files/33854805/README.-.CRUD.md)
# 🛒 Laboratorio Laravel: CRUD de Productos (documentación completa)

**Universidad Tecnológica de Panamá** · Facultad de Ingeniería de Sistemas Computacionales · Campus Víctor Levis Sasso
Desarrollo Web · II Semestre 2026 · Docente: Ing. Irina Fong

## 1. ¿De qué trata?

Se creó el proyecto `crud_rapido` con Laravel y se generó un CRUD completo para la tabla `products` (descripción, precio y stock), usando el paquete `ibex/crud-generator` con el *stack* Bootstrap.

## 2. Flujo de comandos

```bash
cd C:\wamp64\www
laravel new crud_rapido
cd crud_rapido

# (crear la BD crud_rapido en phpMyAdmin y configurar el .env)
php artisan config:clear

php artisan make:model Product -m      # modelo + migración
php artisan migrate

composer require ibex/crud-generator --dev
php artisan vendor:publish --tag=crud
php artisan make:crud products         # genera controlador, modelo, request y vistas
composer dump-autoload
php artisan route:list
```

## 3. Migración de `products`

```php
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('description');
    $table->double('price', 8, 2);   // 8 dígitos en total, 2 decimales
    $table->integer('stock');
    $table->timestamps();
});
```

## 4. Generación del CRUD

`php artisan make:crud products` pregunta el *stack* a instalar (Bootstrap, Tailwind, Livewire o API). Se eligió **bootstrap**, y el generador crea el controlador, el modelo, el `ProductRequest` y las vistas.

📎 Captura: `docs/img/08-make-crud.png`

Luego se registra la ruta en `routes/web.php`:

```php
use App\Http\Controllers\ProductController;

Route::resource('products', ProductController::class);
```

## 5. Rutas generadas

| Método | URI | Acción del controlador |
|---|---|---|
| GET | `/products` | `index` |
| GET | `/products/create` | `create` |
| POST | `/products` | `store` |
| GET | `/products/{product}` | `show` |
| GET | `/products/{product}/edit` | `edit` |
| PUT/PATCH | `/products/{product}` | `update` |
| DELETE | `/products/{product}` | `destroy` |

📎 Captura: `docs/img/07-route-list.png`

## 6. El CRUD en funcionamiento

| Operación | Captura |
|---|---|
| Listado vacío (Read) | `docs/img/01-products-vacio.png` |
| Crear producto (Create) | `docs/img/02-products-create.png` |
| Editar producto (Update) | `docs/img/03-products-edit.png` |
| Producto actualizado | `docs/img/04-products-updated.png` |
| Eliminar producto (Delete) | `docs/img/05-products-deleted.png` |
| Registros en la tabla `products` | `docs/img/06-phpmyadmin-products.png` |

## 7. Base de datos y respaldo

La base de datos `crud_rapido` tiene 10 tablas: las 9 que genera Laravel por defecto más `products`.

📎 Captura: `docs/img/09-phpmyadmin-crud-rapido.png`

Respaldo: `database/backup_crud_rapido.sql`. Para restaurarlo:

```bash
mysql -u root -p -e "CREATE DATABASE crud_rapido CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
mysql -u root -p crud_rapido < database/backup_crud_rapido.sql
```

## 8. Dificultades y soluciones

### Dificultad 1: `Value "" is invalid` al ejecutar `make:crud`

- **Problema:** al pedir el *stack* a instalar se presionó Enter sin escribir nada y el generador mostró `[ERROR] Value "" is invalid`.
- **Solución:** el comando volvió a preguntar y se escribió explícitamente `bootstrap`. Como el controlador y el modelo ya existían, se respondió `y` a las preguntas de sobrescritura.

### Dificultad 2: Longitud de cadena en las migraciones

- **Problema:** las columnas `string` pueden provocar errores de longitud de clave en ciertas configuraciones de MySQL.
- **Solución:** en `AppServiceProvider.php` se agregó `Schema::defaultStringLength(191);` dentro de `boot()`.

## 9. Referencias

1. Laravel. *Documentación oficial, versión 12.x.* https://laravel.com/docs/12.x
2. Awais. *Laravel CRUD Generator (`ibex/crud-generator`).* https://github.com/awais-vteams/laravel-crud-generator
3. Fong, I. *CRUD de Laravel: Guía del Laboratorio #2.* Universidad Tecnológica de Panamá, 2026.
4. Video de apoyo del laboratorio. https://www.youtube.com/watch?v=j5baJsM_Adc

## 10. Fecha de ejecución

**22 de octubre de 2026**

---

**Este laboratorio ha sido desarrollado por el estudiante de la Universidad Tecnológica de Panamá:**

- **Nombre:** Erick Elizondro

- **Instructor del Laboratorio:** Irina Fong
