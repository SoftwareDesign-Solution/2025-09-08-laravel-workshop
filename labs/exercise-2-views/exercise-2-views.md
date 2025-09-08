- [2. Views & Blade](#2-views--blade)
  - [2.1 Views anlegen](#21-views-anlegen)
  - [2.2 Controller-Anbindung](#22-controller-anbindung)

Bearbeitungszeit: 30 Minuten

The solution branch for the whole lab is `solution-1-aufgaben`

# 1. Views & Blade

## 2.1 Views anlegen

1. Erstelle im Verzeichnis `resources/views` die Datei `dashboard.blade.php`.
   - Übertrage den Inhalt von `dashboard.html` aus dem Ausgabenverzeichnis.
2. Erstelle im Verzeichnis `resources/views/tasks` die Datei `index.blade.php`.
   - Übertrage den Inhalt von `tasks/index.html` aus dem Ausgabenverzeichnis.
3. Erstelle im Verzeichnis `resources/views/tasks` die Datei `show.blade.php`.
   - Übertrage den Inhalt von `tasks/show.html` aus dem Ausgabenverzeichnis.

<details>
<summary>Show solution</summary>
<p>

**/**

```
```

</p>
</details>

## 2.2 Controller-Anbindung

1. Passe den DashboardController so an, dass die Methode index() die dashboard.blade.php-View rendert.
2. Passe den TaskController so an:
   - `index()` rendert die View `tasks/index.blade.php`
   - `show()` rendert die View `tasks/show.blade.php` (ggf. mit Platzhalterdaten)

<details>
<summary>Show solution</summary>
<p>

**/**

```
```

</p>
</details>
