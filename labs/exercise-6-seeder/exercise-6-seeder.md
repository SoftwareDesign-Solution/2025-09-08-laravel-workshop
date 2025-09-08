- [6. Seeder](#1-aufgabe)
  - [6.1 Seeder für Kategorien](#61-seeder-für-kategorien)
  - [6.2 Seeder für Schlagwörter (Tags)](#62-seeder-für-schlagwörter-tags)
  - [6.3 Seeder für Benutzer](#63-seeder-für-benutzer)

Bearbeitungszeit: n Minuten

The solution branch for the whole lab is `solution-1-aufgaben`

# 1. Aufgaben

In den vorherigen Aufgaben haben Sie die Tabellen über Migrationen erstellt. In dieser Aufgabe geht es nun darum, die Tabellen mit Beispieldaten zu befüllen. Dazu werden Seeder-Klassen erstellt, die mithilfe von Models Datensätze automatisch in die Datenbank einfügen.

## 6.1 Seeder für Kategorien

Erstelle einen Seeder, der die Tabelle `categories` mit Beispiel-Daten füllt.

Die Anforderungen:

- Der Seeder soll mehrere Kategorien in die Datenbank einfügen.
- Jede Kategorie hat folgende Felder:
  - `name` (z. B. Arbeit, Privat, Projekt, …)
  - `color` (Hex-Farbcode, z. B. #3B82F6)
  - `description` (kurze Beschreibung, optional)
- Der Seeder soll eine Schleife nutzen, um die Datensätze in die Datenbank zu schreiben.
- Verwende das `Category`-Model für die Erstellung der Einträge.

Erstelle den Seeder mit Artisan:

```bash
php artisan make:seeder CategorySeeder
```

Führe ihn danach mit folgendem Befehl aus:

```bash
php artisan db:seed --class=CategorySeeder
```

<details>
<summary>Show solution</summary>
<p>

**/database/seeders/CategorySeeder.php**

```php
<?php
namespace Database\Seeders;

use App\Models\Category;
use Illuminate\Database\Seeder;

class CategorySeeder extends Seeder
{
    public function run(): void
    {
        $categories = [
            ['name' => 'Arbeit', 'color' => '#3B82F6', 'description' => 'Berufliche Aufgaben und Projekte'],
            ['name' => 'Privat', 'color' => '#10B981', 'description' => 'Private Erledigungen und Termine'],
            ['name' => 'Projekt', 'color' => '#F59E0B', 'description' => 'Spezifische Projektarbeit'],
            ['name' => 'Lernen', 'color' => '#8B5CF6', 'description' => 'Weiterbildung und Lernziele'],
            ['name' => 'Gesundheit', 'color' => '#EF4444', 'description' => 'Gesundheit und Fitness'],
            ['name' => 'Shopping', 'color' => '#EC4899', 'description' => 'Einkäufe und Besorgungen'],
        ];

        foreach ($categories as $category) {
            Category::create($category);
        }
    }
}
```

</p>
</details>

## 6.2 Seeder für Schlagwörter (Tags)

Erstelle einen Seeder, der die Tabelle `tags` mit Beispiel-Daten füllt.

Die Anforderungen:

- Es sollen mehrere Tags in die Datenbank eingefügt werden.
- Jedes Tag soll folgende Felder enthalten:
  - `name` (z. B. Dringend, Meeting, Review, …)
  - `slug` → automatisch aus dem Namen generiert (verwende `Str::slug()`).
  - `color` (Hex-Farbcode, z. B. #EF4444).
- Der Seeder soll eine Schleife nutzen, um die Einträge mit dem `Tag`-Model zu erstellen.

Erstelle den Seeder mit Artisan:

```bash
php artisan make:seeder TagSeeder
```

Führe ihn mit folgendem Befehl aus:

```bash
php artisan db:seed --class=TagSeeder
```

<details>
<summary>Show solution</summary>
<p>

**/database/seeders/TagSeeder.php**

```php
<?php
namespace Database\Seeders;

use App\Models\Tag;
use Illuminate\Database\Seeder;
use Illuminate\Support\Str;

class TagSeeder extends Seeder
{
    public function run(): void
    {
        $tags = [
            ['name' => 'Dringend', 'color' => '#EF4444'],
            ['name' => 'Meeting', 'color' => '#3B82F6'],
            ['name' => 'Review', 'color' => '#10B981'],
            ['name' => 'Bug', 'color' => '#F97316'],
            ['name' => 'Feature', 'color' => '#8B5CF6'],
            ['name' => 'Dokumentation', 'color' => '#6B7280'],
            ['name' => 'Testing', 'color' => '#06B6D4'],
            ['name' => 'Deployment', 'color' => '#EC4899'],
            ['name' => 'Wartung', 'color' => '#84CC16'],
            ['name' => 'Planung', 'color' => '#F59E0B'],
        ];

        foreach ($tags as $tag) {
            Tag::create([
                'name' => $tag['name'],
                'slug' => Str::slug($tag['name']),
                'color' => $tag['color']
            ]);
        }
    }
}
```

</p>
</details>

## 6.3 Seeder für Benutzer

Erstelle einen Seeder, der die Tabelle `users` mit Beispiel-Daten füllt.

Die Anforderungen:

- Es soll mindestens ein Admin-User mit folgenden Eigenschaften erstellt werden:
  - `name`: Administrator
  - `email`: admin@taskflow.dev
  - `password`: verschlüsselt mit Hash::make()
  - `email_verified_at`: aktuelles Datum/Zeit (now())
- Es soll ein Demo-User erstellt werden:
  - `name`: Max Mustermann
  - `email`: user@taskflow.dev
  - `password`: ebenfalls verschlüsselt mit Hash::make()
  - `email_verified_at`: aktuelles Datum/Zeit
- Zusätzlich sollen mit einer Factory mehrere Demo-Benutzer erstellt werden (z. B. 8 Stück).

Erstelle den Seeder mit Artisan:

```bash
php artisan make:seeder UserSeeder
```

Führe ihn mit folgendem Befehl aus:

```bash
php artisan db:seed --class=UserSeeder
```

<details>
<summary>Show solution</summary>
<p>

**/database/seeders/UserSeeder.php**

```php
<?php

namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Hash;

class UserSeeder extends Seeder
{
    public function run(): void
    {
        // Admin User
        User::create([
            'name' => 'Administrator',
            'email' => 'admin@taskflow.dev',
            'password' => Hash::make('password'),
            'email_verified_at' => now(),
        ]);

        // Demo User
        User::create([
            'name' => 'Max Mustermann',
            'email' => 'user@taskflow.dev',
            'password' => Hash::make('password'),
            'email_verified_at' => now(),
        ]);

        // Additional demo users
        User::factory(8)->create();
    }
}
```

</p>
</details>
