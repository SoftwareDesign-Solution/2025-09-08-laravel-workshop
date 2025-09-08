# 2025-09-08-laravel-workshop

## ✨ Beschreibung

Dieses Repository enthält das Schulungsprojekt zur Schulung **„Laravel Workshop“**. Ziel ist es, die Grundlagen der Laravel-Entwicklung kennenzulernen, insbesondere den Aufbau von Controllern, Routing, Views sowie den Einsatz moderner Frontend-Technologien wie Tailwind CSS und Alpine.js.

Das Projekt nutzt Vite als modernen Frontend-Bundler in Kombination mit dem Laravel-Vite-Plugin. Die Laravel-Backend-Umgebung ist für PHP 8.2 und Laravel 12 konfiguriert.

---

## 🚀 Technologien

### **Backend (PHP / Laravel)**
- **Laravel (v12.x)**: Web-Framework für moderne PHP-Anwendungen
- **Laravel Breeze**: Einfaches Authentifizierungs-Starterkit
- **Laravel Sail**: Entwicklungsumgebung mit Docker
- **Laravel Pail**: Live-Tail von Logs im Terminal
- **PHPUnit**: Test-Framework
- **Faker**: Generierung von Testdaten

### **Frontend**
- **Vite (v7.x)**: Moderner Frontend-Bundler
- **Tailwind CSS (v3.x)**: Utility-first CSS-Framework
- **Alpine.js (v3.x)**: Minimalistisches JavaScript-Framework
- **Axios**: HTTP-Client für AJAX-Requests

---

## 📚 Schulungsaufgaben

Die Schulungsaufgaben befinden sich im Verzeichnis `labs`:

- **exercise-1-controller**: Einstieg in Controller, Routing und Views mit Laravel

### 🔁 Lösungen

Die Lösung zur Aufgabe ist im folgenden Branch zu finden:

- `solution-1-controller`

Zum Wechseln in den Branch:
```bash
   git checkout solution-1-controller
```

---

## ⚙️ Voraussetzungen

- PHP ≥ 8.2
- Composer
- Node.js & npm
- (optional) Docker (für Laravel Sail)

---

## 🧪 Entwicklungs- & Build-Skripte

### **Backend & Frontend gemeinsam starten**
```bash
composer run dev
```
Dies startet:
- Laravel Development Server
- Queue Listener
- Log Tail mit Laravel Pail
- Vite Dev Server

### **Nur Vite starten**
```bash
npm run dev
```

### **Build Frontend Assets**
```bash
npm run build
```

### **Backend Tests ausführen**
```bash
composer test
```

---

## 📁 Wichtige Pfade

- `app/Http/Controllers`: Controller-Logik
- `resources/views`: Blade-Templates
- `routes/web.php`: Web-Routing
- `resources/js/`: Frontend-Code (Alpine, Axios etc.)
- `labs/`: Schulungsaufgaben

---

## 🙏 Beitrag

Beiträge zur Schulung oder Verbesserung des Codes sind herzlich willkommen. Pull-Requests oder Issues sind gern gesehen!