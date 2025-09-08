- [5. Migrationen](#5-migrationen)
  - [5.1 Migration für Kategorien](#51-migration-für-kategorien)
  - [5.2 Komplexe Migration für Aufgabenverwaltung](#52-komplexe-migration-für-aufgabenverwaltung)
  - [5.3 Migration für Schlagwörter (Tags)](#53-migration-für-schlagwörter-tags)
  - [5.4 Migration für Pivot-Tabelle (Many-to-Many)](#54-migration-für-pivot-tabelle-many-to-many)
  - [5.5 Migration für Kommentare](#55-migration-für-kommentare)
  - [5.6 Migration für Dateianhänge](#56-migration-für-dateianhänge)

Bearbeitungszeit: 60 Minuten

The solution branch for the whole lab is `solution-5-migrations`

# 5. Migrationen

In den vorherigen Aufgaben haben Sie Models kennengelernt. In dieser Aufgabe geht es nun darum, die zugehörigen Tabellenschemas in den Migrationsdateien zu hinterlegen und anschließend die Datenbank auf Basis der Migrationsdateien zu erstellen.

## 5.1 Migration für Kategorien

Erstelle eine Migration für eine Tabelle `categories` mit den folgenden Anforderungen:

- Die Tabelle soll ein Primärschlüssel-Feld `id` enthalten.
- Ein Pflichtfeld `name` vom Typ `string`.
- Ein Feld `color` vom Typ `string`, das standardmäßig den Wert `#3B82F6` hat.
- Ein Feld `description` vom Typ `text`, das optional ist.
- Standardmäßig sollen die Spalten `created_at` und `updated_at` vorhanden sein.

Wenn die Migrationsdatei in der vorherigen Aufgabe nicht erstellt wurde, nutzen Sie den folgenden Artisan-Befehl um die Migration zu erstellen

```bash
php artisan make:migration create_categories_table
```

<details>
<summary>Show solution</summary>
<p>

**/database/migrations/2025_08_29_214632_create_categories_table.php**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('color')->default('#3B82F6');
            $table->text('description')->nullable();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('categories');
    }
};
```

</p>
</details>

## 5.2 Komplexe Migration für Aufgabenverwaltung

Erstelle eine Migration für eine Tabelle `tasks` mit folgenden Anforderungen:

- Ein Primärschlüssel `id`.
- Ein Pflichtfeld `title` vom Typ `string`.
- Ein optionales Feld `description` vom Typ `text`.
- Ein Feld `status` als `enum` mit den Werten:
  - `pending` (Standardwert)
  - `in_progress`
  - `completed`
  - `cancelled`
- Ein Feld `priority` als `enum` mit den Werten:
  - `low`
  - `medium` (Standardwert)
  - `high`
  - `urgent`
- Ein optionales Feld `due_date` vom Typ `date`.
- Eine Beziehung zu `users` (`user_id`), die bei Löschung des Users alle zugehörigen Aufgaben ebenfalls löscht (`cascade`).
- Eine optionale Beziehung zu `categories` (`category_id`), die bei Löschung der Kategorie den Wert auf `null` setzt.
- Ein optionales Feld `completed_at` vom Typ `timestamp`.
- Automatische Zeitstempel (`created_at`, `updated_at`).
- Ein Index auf `status` und `priority` (Kombination).
- Ein Index auf `due_date`.

Wenn die Migrationsdatei in der vorherigen Aufgabe nicht erstellt wurde, nutzen Sie den folgenden Artisan-Befehl um die Migration zu erstellen

```bash
php artisan make:migration create_tasks_table
```

<details>
<summary>Show solution</summary>
<p>

**/database/migrations/2025_08_29_214755_create_tasks_table.php**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description')->nullable();
            $table->enum('status', ['pending', 'in_progress', 'completed', 'cancelled'])->default('pending');
            $table->enum('priority', ['low', 'medium', 'high', 'urgent'])->default('medium');
            $table->date('due_date')->nullable();
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->foreignId('category_id')->nullable()->constrained()->onDelete('set null');
            $table->timestamp('completed_at')->nullable();
            $table->timestamps();
            
            $table->index(['status', 'priority']);
            $table->index('due_date');
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('tasks');
    }
};
```

</p>
</details>

## 5.3 Migration für Schlagwörter (Tags)

Erstelle eine Migration für eine Tabelle `tags` mit den folgenden Anforderungen:

- Primärschlüssel `id`.
- Ein Pflichtfeld `name` (`string`), das **eindeutig** sein muss.
- Ein Pflichtfeld `slug` (`string`), das **eindeutig** sein muss.
- Ein Feld `color` (`string`), das standardmäßig den Wert `#6B7280` hat.
- Automatische Zeitstempel (`created_at`, `updated_at`).

Wenn die Migrationsdatei in der vorherigen Aufgabe nicht erstellt wurde, nutzen Sie den folgenden Artisan-Befehl um die Migration zu erstellen

```bash
php artisan make:migration create_tags_table
```

<details>
<summary>Show solution</summary>
<p>

**/database/migrations/2025_08_29_214852_create_tags_table.php**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('tags', function (Blueprint $table) {
            $table->id();
            $table->string('name')->unique();
            $table->string('slug')->unique();
            $table->string('color')->default('#6B7280');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('tags');
    }
};
```

</p>
</details>

## 5.4 Migration für Pivot-Tabelle (Many-to-Many)

Erstelle eine Migration für eine Pivot-Tabelle `tag_task`, die eine Many-to-Many-Beziehung zwischen `tasks` und `tags` abbildet.

Die Tabelle soll folgende Anforderungen erfüllen:

- Primärschlüssel `id`.
- Fremdschlüssel `task_id`, der auf die Tabelle `tasks` verweist.
  - Beim Löschen eines Tasks sollen die zugehörigen Einträge ebenfalls gelöscht werden (`cascade`).
- Fremdschlüssel `tag_id`, der auf die Tabelle `tags` verweist.
  - Beim Löschen eines Tags sollen die zugehörigen Einträge ebenfalls gelöscht werden (`cascade`).
- Automatische Zeitstempel (`created_at`, `updated_at`).
- Eine Kombination aus `task_id` und `tag_id` muss eindeutig sein (damit ein Task nicht mehrfach mit demselben Tag verknüpft werden kann).

Erstelle die Migration mit dem Artisan-Befehl:

```bash
php artisan make:migration create_tag_task_table
```

<details>
<summary>Show solution</summary>
<p>

**/database/migrations/2025_08_29_215440_create_tag_task_table.php**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('tag_task', function (Blueprint $table) {
            $table->id();
            $table->foreignId('task_id')->constrained()->onDelete('cascade');
            $table->foreignId('tag_id')->constrained()->onDelete('cascade');
            $table->timestamps();
            
            $table->unique(['task_id', 'tag_id']);
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('tag_task');
    }
};
```

</p>
</details>

## 5.5 Migration für Kommentare

Erstelle eine Migration für eine Tabelle `comments`, die es ermöglicht, Kommentare zu Aufgaben zu speichern.

Die Tabelle soll folgende Anforderungen erfüllen:

- Primärschlüssel `id`.
- Fremdschlüssel `task_id`, der auf die Tabelle `tasks` verweist.
  - Beim Löschen einer Aufgabe sollen auch die zugehörigen Kommentare gelöscht werden (`cascade`).
- Fremdschlüssel `user_id`, der auf die Tabelle `users` verweist.
  - Beim Löschen eines Users sollen ebenfalls die zugehörigen Kommentare gelöscht werden (`cascade`).
- Ein Pflichtfeld `content` vom Typ `text`.
- Automatische Zeitstempel (`created_at`, `updated_at`).

Wenn die Migrationsdatei in der vorherigen Aufgabe nicht erstellt wurde, nutzen Sie den folgenden Artisan-Befehl um die Migration zu erstellen

```bash
php artisan make:migration create_comments_table
```

<details>
<summary>Show solution</summary>
<p>

**/database/migrations/2025_08_29_215604_create_comments_table.php**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('comments', function (Blueprint $table) {
            $table->id();
            $table->foreignId('task_id')->constrained()->onDelete('cascade');
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->text('content');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('comments');
    }
};
```

</p>
</details>

## 5.6 Migration für Dateianhänge

Erstelle eine Migration für eine Tabelle `attachments` mit den folgenden Anforderungen:

- Primärschlüssel `id`.
- Eine Beziehung zu `tasks` (`task_id`), die bei Löschung der zugehörigen Aufgabe ebenfalls den Anhang löscht (`cascade`).
- Ein Pflichtfeld `filename` (interner Dateiname, `string`).
- Ein Pflichtfeld `original_name` (ursprünglicher Dateiname, `string`).
- Ein Pflichtfeld `path` (Speicherpfad, `string`).
- Ein Pflichtfeld `mime_type` (z. B. `image/png`, `application/pdf`, `string`).
- Ein Pflichtfeld `size` als `unsignedBigInteger` (Dateigröße in Bytes).
- Automatische Zeitstempel (`created_at`, `updated_at`).

<details>
<summary>Show solution</summary>
<p>

**/database/migrations/2025_08_29_215612_create_attachments_table.php**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('attachments', function (Blueprint $table) {
            $table->id();
            $table->foreignId('task_id')->constrained()->onDelete('cascade');
            $table->string('filename');
            $table->string('original_name');
            $table->string('path');
            $table->string('mime_type');
            $table->unsignedBigInteger('size');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('attachments');
    }
};
```

</p>
</details>
