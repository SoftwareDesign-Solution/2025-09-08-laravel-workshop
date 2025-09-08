- [1. Controller & Routing](#1-controller--routing)
  - [1.1 DashboardController](#11-dashboardcontroller)
  - [1.2 TaskController](#12-taskcontroller)
  - [1.3 Routing](#13-routing)

Bearbeitungszeit: 30 Minuten

The solution branch for the whole lab is `solution-1-aufgaben`

# 1. Controller & Routing

## 1.1 DashboardController

Erstellen Sie den DashboardController und registrieren Sie diesen im Routing

**Anforderungen:**

- Verwenden Sie den Artisan-Befehl, um den Controller zu erstellen
- Implementieren Sie die öffentliche Methode `index`
- Stelle sicher, dass index() eine View rendert oder einen einfachen Text zurückgibt (temporär genügt ein Platzhalter).

**Artisan-Befehl:**

```bash
php artisan make:controller DashboardController
```

<details>
<summary>Show solution</summary>
<p>

**/app/Http/Controllers/DashboardController.php**

```php
<?php
namespace App\Http\Controllers;

use App\Models\Task;
use App\Models\Category;
use Illuminate\Support\Facades\Auth;

class DashboardController extends Controller
{
    public function index()
    {
        return view('dashboard');
    }
}
```

</p>
</details>

## 1.2 TaskController

Erstellen Sie den TaskController als Resource Controller und registrieren Sie diesen im Routing

- Verwenden Sie den Artisan-Befehl, um den Controller zu erstellen
- Stelle sicher, dass die Standard-Resource-Methoden vorhanden sind: index, create, store, show, edit, update, destroy.

**Artisan-Befehl:**

```bash
php artisan make:controller TaskController --resource
```

<details>
<summary>Show solution</summary>
<p>

**/app/Http/Controllers/TaskController.php**

```php
<?php
namespace App\Http\Controllers;

use App\Models\Task;
use App\Models\Category;
use App\Models\Tag;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

class TaskController extends Controller
{
    public function index(Request $request)
    {
        //
    }

    public function create()
    {
        //
    }

    public function store(Request $request)
    {
        //
    }

    public function show(Task $task)
    {
        //
    }

    public function edit(Task $task)
    {
        /
    }

    public function update(Request $request, Task $task)
    {
        //
    }

}
```

</p>
</details>

## 1.3 Routing in `routes/web.php`

1. Weiterleitung der Startseite:
   - Route / soll standardmäßig auf /dashboard umleiten.
2. Dashboard-Route:
   - Lege eine Route /dashboard an, die auf DashboardController@index zeigt.
   - Vergib den Routennamen dashboard.
3. Task-Routen:
   - Registriere eine Resource Route für tasks, die den TaskController verwendet.

<details>
<summary>Show solution</summary>
<p>

**/routes/web.php**

```php
<?php

use App\Http\Controllers\ProfileController;
use App\Http\Controllers\DashboardController;
use App\Http\Controllers\TaskController;
use App\Http\Controllers\AttachmentController;
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return redirect()->route('dashboard');
});

Route::middleware('auth')->group(function () {

    // Dashboard
    Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');
    
    // Tasks Resource Routes
    Route::resource('tasks', TaskController::class);
    
    // Profile Routes
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');
    
});

require __DIR__.'/auth.php';
```

</p>
</details>
