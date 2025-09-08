- [3. EloquentORM](#3-eloquentorm)
  - [3.1 Model für Kategorien](#31-model-für-kategorien)
  - [3.2 Model für Aufgaben](#32-model-für-aufgaben)
  - [3.3 Model für Schlagwörter (Tags)](#33-model-für-schlagwörter-tags)
  - [3.4 Model für Kommentare](#34-model-für-kommentare)
  - [3.5 Model für Dateianhänge](#35-model-für-dateianhänge)

Bearbeitungszeit: n Minuten

The solution branch for the whole lab is `solution-1-aufgaben`

# 3. EloquentORM

In dieser Aufgabe lernen Sie das Eloquent ORM kennen, mit dem Sie in Laravel auf Datenbanktabellen zugreifen können. Mithilfe von Models lassen sich Datensätze einfach erstellen, auslesen, aktualisieren und löschen, ohne dabei direkt SQL schreiben zu müssen.

## 3.1 Model für Kategorien

Erstellen Sie ein Model `Category`, das Kategorien für Aufgaben repräsentiert.

**Anforderungen:**

- Verwenden Sie den Artisan-Befehl, um das Model zu erstellen.
- Geben Sie bei der Erstellung auch direkt an, dass gleichzeitig eine Migration erzeugt wird.
- Das Model soll im Verzeichnis `app/Models` liegen.
- Definieren Sie im Model eine Relationship vom Typ **One-to-Many**:
- Eine Kategorie kann mehrere Tasks haben (`hasMany`).
- Machen Sie die Felder `name`, `color` und `description` massen-zuweisbar (`$fillable`).
- Erstellen Sie zusätzlich ein Accessor-Attribut `task_count`, das die Anzahl der Tasks einer Kategorie zurückgibt.

**Artisan-Befehl:**

```bash
php artisan make:model Category -m
```

**Hinweis:**

- Die Relationship wird im Model mit `hasMany(Task::class)` definiert.

<details>
<summary>Show solution</summary>
<p>

**/app/Models/Category.php**

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Category extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'color', 'description'];

    public function tasks(): HasMany
    {
        return $this->hasMany(Task::class);
    }

    public function getTaskCountAttribute(): int
    {
        return $this->tasks()->count();
    }
}
```

</p>
</details>

## 3.2 Model für Aufgaben

Erstellen Sie ein Model `Task`, das Aufgaben in der Anwendung repräsentiert.

**Anforderungen:**

- Verwenden Sie den Artisan-Befehl, um das Model zu erstellen.
- Geben Sie bei der Erstellung direkt an, dass gleichzeitig eine Migration erzeugt wird.
- Das Model soll im Verzeichnis `app/Models` liegen.
- Definieren Sie im Model folgende Eigenschaften:
  - `$fillable` für die Felder: `title`, `description`, `status`, `priority`, `due_date`, `user_id`, `category_id`, `completed_at`.
  - `$casts` für die Felder:
    - `due_date` → `date`
    - `completed_at` → `datetime`

**Relationships:**

- `belongsTo(User::class)` → jede Aufgabe gehört einem Benutzer.
- `belongsTo(Category::class)` → jede Aufgabe gehört zu einer Kategorie.
- `belongsToMany(Tag::class)` → Aufgaben können mehrere Tags haben (Many-to-Many mit Timestamps).
- `hasMany(Comment::class)` → eine Aufgabe kann mehrere Kommentare haben (Sortierung: neueste zuerst).
- `hasMany(Attachment::class)` → eine Aufgabe kann mehrere Anhänge haben.

**Scopes:**

- `scopeCompleted` → filtert abgeschlossene Aufgaben (`status = completed`).
- `scopePending` → filtert offene Aufgaben (`status = pending`).
- `scopeOverdue` → filtert überfällige Aufgaben (Fälligkeitsdatum liegt in der Vergangenheit und `status` ist nicht `completed`).

**Accessor:**

- `is_overdue` → gibt zurück, ob eine Aufgabe überfällig ist (`true` / `false`).

**Artisan-Befehl:**

```bash
php artisan make:model Task -m
```

**Hinweis:**

- Mit den Scopes und Accessors können Sie Aufgaben direkt im Code filtern oder Zusatzinformationen abrufen.

<details>
<summary>Show solution</summary>
<p>

**/app/Models/Task.php**

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;
use Illuminate\Database\Eloquent\Relations\HasMany;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Task extends Model
{
    use HasFactory;

    protected $fillable = [
        'title', 'description', 'status', 'priority', 'due_date',
        'user_id', 'category_id', 'completed_at'
    ];

    protected $casts = [
        'due_date' => 'date',
        'completed_at' => 'datetime',
    ];

    public function user(): BelongsTo
    {
        return $this->belongsTo(User::class);
    }

    public function category(): BelongsTo
    {
        return $this->belongsTo(Category::class);
    }

    public function tags(): BelongsToMany
    {
        return $this->belongsToMany(Tag::class)->withTimestamps();
    }

    public function comments(): HasMany
    {
        return $this->hasMany(Comment::class)->latest();
    }

    public function attachments(): HasMany
    {
        return $this->hasMany(Attachment::class);
    }

    public function scopeCompleted($query)
    {
        return $query->where('status', 'completed');
    }

    public function scopePending($query)
    {
        return $query->where('status', 'pending');
    }

    public function scopeOverdue($query)
    {
        return $query->where('due_date', '<', today())
                    ->where('status', '!=', 'completed');
    }

    public function getIsOverdueAttribute(): bool
    {
        return $this->due_date && 
               $this->due_date->isPast() && 
               $this->status !== 'completed';
    }
}
```

</p>
</details>

## 3.3 Model für Schlagwörter (Tags)

Erstellen Sie ein Model `Tag`, das die Schlagwörter (Tags) in der Anwendung verwaltet.

**Anforderungen:**

- Verwenden Sie den Artisan-Befehl, um das Model zu erstellen.
- Geben Sie bei der Erstellung direkt an, dass gleichzeitig eine Migration erzeugt wird.
- Das Model soll im Verzeichnis `app/Models` liegen.
- Definieren Sie im Model folgende Eigenschaften:
  - `$fillable` für die Felder: `name`, `slug`, `color`.

**Relationships:**

- `belongsToMany(Task::class)` → Tags können in mehreren Aufgaben verwendet werden (Many-to-Many mit Timestamps).

**Slug-Generierung:**

- Verwenden Sie den creating-Hook für die automatische Generierung von `slug` aus `name` unter Verwendung des Helpers `Str::slug()`

**Artisan-Befehl:**

```bash
php artisan make:model Tag -m
```

<details>
<summary>Show solution</summary>
<p>

**/app/Models/.php**

```php
```

</p>
</details>

## 3.4 Model für Kommentare

Erstellen Sie ein Model `Comment`, das Kommentare zu Aufgaben speichert.

**Anforderungen:**

- Verwenden Sie den Artisan-Befehl, um das Model zu erstellen.
- Geben Sie bei der Erstellung auch direkt an, dass gleichzeitig eine Migration erzeugt wird.
- Das Model soll im Verzeichnis `app/Models` liegen.

Artisan-Befehl:

```bash
php artisan make:model Comment -m
```

<details>
<summary>Show solution</summary>
<p>

**/app/Models/Comment.php**

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    //
}
```

</p>
</details>

## 3.5 Model für Dateianhänge

Erstellen Sie ein Model `Attachment`, das die Dateianhänge zu Aufgaben repräsentiert.

**Anforderungen:**

- Verwenden Sie den Artisan-Befehl, um das Model zu erstellen.
- Geben Sie bei der Erstellung auch direkt an, dass gleichzeitig eine Migration für die zugehörige Tabelle erzeugt werden soll.
- Das Model soll sich im Verzeichnis app/Models befinden.
- In der Migration soll die Tabelle attachments erstellt werden.

**Artisan-Befehl:**

```bash
php artisan make:model Attachment -m
```

<details>
<summary>Show solution</summary>
<p>

**/app/Models/Attachment.php**

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Attachment extends Model
{
    //
}
```

</p>
</details>
