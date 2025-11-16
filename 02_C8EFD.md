# KAPCSOLAT TESZT

PROJEKT STRUKTÚRA

```
webprogramozas/
└── backend/
	 └── 251117/
	    └── C8EFD/
	        ├── konyvtar-test-01/           ← Ezt hoztad létre
	        │   ├── node_modules/        ← Ezt hoztad létre
	        │   ├── package.json         ← Ezt hoztad létre
	        │   ├── package-lock.json    ← Ezt hoztad létre
	        │   └── test.js              ← Ezt hoztad létre
	        │
	        ├── konyvtar-api-simple-02/
	        │   ├── node_modules/
	        │   ├── package.json
	        │   ├── package-lock.json
	        │   └── server.js
	        │
	        └── konyvtar-api-crud-03/ 
	            ├── node_modules/
	            ├── package.json
	            ├── package-lock.json
	            └── server.js

```


### Lépés 1: Projekt mappa

1. Nyisd meg a  **VS Code terminált**, majd navigálj be a `webprogramozas/251117/C8EFD` mappába
2. Írd be:

---
```powershell
mkdir konyvtar-test-01
cd konyvtar-test-01
```


###  Lépés 2: Node.js inicializálás

```powershell
npm init -y
```


### Lépés 3: mssql csomag telepítése

```powershell
npm install mssql
```


### Lépés 4: test.js fájl létrehozása

```powershell
ni test.js
```

### Lépés 5: test.js kód

Másold be ezt a `test.js` fájlba:

```javascript
const sql = require('mssql');

const config = {
    user: 'admin',
    password: '1234',
    server: 'localhost',
    port: 1433,
    database: 'KonyvtarDB',
    options: {
        encrypt: false,
        trustServerCertificate: true
    }
};

console.log('Próbálok csatlakozni...');

sql.connect(config)
    .then(() => {
        console.log('✓ SIKERES KAPCSOLAT!');
        console.log('Az adatbázis elérhető.');
        console.log('Lépj át a következő szintre HARCOS!');
        process.exit(0);
    })
    .catch(err => {
        console.log('✗ HIBA!');
        console.log('Hibaüzenet:', err.message);
        process.exit(1);
    });
```

Mentsd el: **Ctrl + S**

###  Lépés 6: Futtatás

```powershell
node test.js
```


***

## VÁRT KIMENET:

```
Próbálok csatlakozni...
✓ SIKERES KAPCSOLAT!
Az adatbázis elérhető.
```


***
