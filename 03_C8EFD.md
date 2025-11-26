
# SZERVER

## NODE.JS Szerver létrehozás

###  Lépés 1: Projekt Mappa Létrehozása

A szervert az alábbi környezetben hozd létre:

**PROJEKT STRUKTÚRA**

```
webprogramozas/
└── backend/
	 └── 251117/
	    └── C8EFD/
	        ├── konyvtar-test-01/
	        │   ├── node_modules/  
	        │   ├── package.json
	        │   ├── package-lock.json
	        │   └── test.js
	        │
	        ├── konyvtar-api-02/        ← Ezt hoztad létre
	        │   ├── node_modules/       ← Ezt hoztad létre
	        │   ├── package.json        ← Ezt hoztad létre
	        │   ├── package-lock.json   ← Ezt hoztad létre
	        │   └── server.js           ← Ezt hoztad létre
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
```powershell
mkdir konyvtar-api-02
cd konyvtar-api-02
```


###  Lépés 2: Node.js Projekt Inicializálása

```powershell
npm init -y
```

###  Lépés 3: Csomagok Telepítése

```powershell
npm install express mssql
```

###  Lépés 4: server.js Fájl Létrehozása

#### PowerShellben:

```powershell
ni server.js
```

ni - új file létrehozása
#### Vagy egyszerűen VS Code-ban:

1. **File** → **New File**
2. Mentsd el: **server.js**

###  Lépés 2: server.js Kód Bemásolása

Másold be ezt a teljes kódot a `server.js` fájlba:

```javascript
// ============================================
// 1. CSOMAGOK IMPORTÁLÁSA
// ============================================
const express = require('express');
const sql = require('mssql');


// ============================================
// 2. ALKALMAZÁS LÉTREHOZÁSA
// ============================================
const app = express();
const port = 3000;


// ============================================
// 3. ADATBÁZIS KONFIGURÁCIÓ
// ============================================
const config = {
  user: 'felhasználónév',
  password: 'jelszó',
  server: 'localhost',   
  database: 'KonyvtarDB',
  options: {
    encrypt: false,
    trustServerCertificate: true,
  }
};


// ============================================
// 4. CONNECTION POOL TÁROLÁSA
// ============================================

// A pool célja a teljesítmény optimalizálása: nem kell minden kérésnél új kapcsolatot létrehozni és lezárni, hanem újrahasználjuk a meglévőket.
let pool;


// ============================================
// 5. ADATBÁZIS CSATLAKOZÁS
// ============================================
sql.connect(config)
    .then(p => {
        pool = p;
        console.log('✓ Adatbázis kapcsolat létrejött');
        
        // Szerver indítása csak kapcsolat után
        startServer();
    })
    .catch(err => {
        console.log('✗ Kapcsolódási hiba:', err.message);
    });


// ============================================
// 6. API ENDPOINT - Összes könyv lekérdezése
// ============================================
app.get('/api/konyvek', async (req, res) => {
    const result = await pool.request().query('SELECT * FROM Konyvek');
    res.json(result.recordset);
});


// ============================================
// 7. SZERVER INDÍTÁSA
// ============================================
function startServer() {
    app.listen(port, () => {
        console.log('✓ Szerver fut: http://localhost:' + port);
        console.log('✓ Endpoint: http://localhost:' + port + '/api/konyvek');
    });
}
```


***


### 1. Csomagok importálása

- `express`: HTTP szerver létrehozásához
- `mssql`: SQL Server kapcsolathoz
### 2. Alkalmazás létrehozása

- `app`: Express alkalmazás objektum
- `port`: Szerver portja (3000)
### 3. Adatbázis konfiguráció

- Felhasználó, jelszó, szerver cím, port, adatbázis név
### 4. Pool tárolása

- Globális változó a connection pool számára
### 5. Csatlakozás

- Kapcsolódás az adatbázishoz
- Pool elmentése
- Hiba kezelés
### 6. API Endpoint

- GET kérés `/api/konyvek` címen
- SELECT lekérdezés végrehajtása
- JSON válasz küldése
### 7. Szerver indítása

- Express szerver elindítása a 3000-es porton
- Csak akkor indul, ha az adatbázis kapcsolat sikerült

***

## Futtatás:

```powershell
node server.js
```

**Várt kimenet:**

```
✓ Adatbázis kapcsolat létrejött
✓ Szerver fut: http://localhost:3000
✓ Endpoint: http://localhost:3000/api/konyvek
```


***

# Postman teszt:

- **GET** `http://localhost:3000/api/konyvek`
- **Send**
- Látod a 10 könyvet



