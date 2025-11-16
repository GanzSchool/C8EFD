# CRUD Műveletek

**CRUD** = **C**reate, **R**ead, **U**pdate, **D**elete

***
PROJEKT STRUKTÚRA

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
	        ├── konyvtar-api-simple-02/
	        │   ├── node_modules/
	        │   ├── package.json
	        │   ├── package-lock.json
	        │   └── server.js
	        │
	        └── konyvtar-api-crud-03/      ← Ezt hoztad létre
	            ├── node_modules/       ← Ezt hoztad létre
	            ├── package.json        ← Ezt hoztad létre
	            ├── package-lock.json   ← Ezt hoztad létre
	            └── server.js           ← Ezt hoztad létre

```
 

***
### Lépés 1: Projekt mappa

1. Nyisd meg a  **VS Code terminált**, majd navigálj be a `webprogramozas/251117/C8EFD` mappába
2. Írd be:
```powershell
mkdir konyvtar-api-crud-03
cd konyvtar-api-crud-03
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

##  TELJES CRUD - Mind a 4 művelet

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

// ============================================
// 4. CONNECTION POOL TÁROLÁSA
// ============================================
let pool;

// ============================================
// 5. MIDDLEWARE JSON adatok fogadásához
// ============================================
app.use(express.json());

// ============================================
// 6. ADATBÁZIS CSATLAKOZÁS
// ============================================
sql.connect(config)
    .then(p => {
        pool = p;
        console.log('✓ Adatbázis kapcsolat létrejött');
        startServer();
    })
    .catch(err => {
        console.log('✗ Kapcsolódási hiba:', err.message);
    });

// ============================================
// 7. CRUD MŰVELETEK
// ============================================

// CREATE (C) - Új könyv létrehozása
app.post('/api/konyvek', async (req, res) => {
    const { Cim, Szerzo, Kiadasi_Ev, Ar, Raktaron } = req.body;
    
    const result = await pool.request()
        .input('Cim', sql.NVarChar, Cim)
        .input('Szerzo', sql.NVarChar, Szerzo)
        .input('Kiadasi_Ev', sql.Int, Kiadasi_Ev)
        .input('Ar', sql.Int, Ar)
        .input('Raktaron', sql.Bit, Raktaron)
        .query('INSERT INTO Konyvek (Cim, Szerzo, Kiadasi_Ev, Ar, Raktaron) VALUES (@Cim, @Szerzo, @Kiadasi_Ev, @Ar, @Raktaron)');
    
    res.json({ success: true, message: 'Könyv létrehozva' });
});

// READ (R) - Összes könyv lekérdezése
app.get('/api/konyvek', async (req, res) => {
    const result = await pool.request().query('SELECT * FROM Konyvek');
    res.json(result.recordset);
});

// READ (R) - Egy könyv lekérdezése ID alapján
app.get('/api/konyvek/:id', async (req, res) => {
    const id = req.params.id;
    
    const result = await pool.request()
        .input('id', sql.Int, id)
        .query('SELECT * FROM Konyvek WHERE KonyvID = @id');
    
    if (result.recordset.length === 0) {
        return res.status(404).json({ message: 'Könyv nem található' });
    }
    
    res.json(result.recordset[0]);
});

// UPDATE (U) - Könyv módosítása
app.put('/api/konyvek/:id', async (req, res) => {
    const id = req.params.id;
    const { Cim, Szerzo, Kiadasi_Ev, Ar, Raktaron } = req.body;
    
    const result = await pool.request()
        .input('id', sql.Int, id)
        .input('Cim', sql.NVarChar, Cim)
        .input('Szerzo', sql.NVarChar, Szerzo)
        .input('Kiadasi_Ev', sql.Int, Kiadasi_Ev)
        .input('Ar', sql.Int, Ar)
        .input('Raktaron', sql.Bit, Raktaron)
        .query('UPDATE Konyvek SET Cim = @Cim, Szerzo = @Szerzo, Kiadasi_Ev = @Kiadasi_Ev, Ar = @Ar, Raktaron = @Raktaron WHERE KonyvID = @id');
    
    res.json({ success: true, message: 'Könyv frissítve' });
});

// DELETE (D) - Könyv törlése
app.delete('/api/konyvek/:id', async (req, res) => {
    const id = req.params.id;
    
    const result = await pool.request()
        .input('id', sql.Int, id)
        .query('DELETE FROM Konyvek WHERE KonyvID = @id');
    
    res.json({ success: true, message: 'Könyv törölve' });
});

// ============================================
// 8. SZERVER INDÍTÁSA
// ============================================
function startServer() {
    app.listen(port, () => {
        console.log('✓ Szerver fut: http://localhost:' + port);
        console.log('✓ CRUD API elérhető: http://localhost:' + port + '/api/konyvek');
    });
}
```


***

##  CRUD összefoglaló táblázat:

| Művelet | HTTP metódus | Endpoint | SQL parancs | Mit csinál |
| :-- | :-- | :-- | :-- | :-- |
| **C**reate | POST | `/api/konyvek` | INSERT | Új könyv hozzáadása |
| **R**ead (összes) | GET | `/api/konyvek` | SELECT | Összes könyv lekérdezése |
| **R**ead (egy) | GET | `/api/konyvek/:id` | SELECT WHERE | Egy könyv lekérdezése |
| **U**pdate | PUT | `/api/konyvek/:id` | UPDATE | Könyv módosítása |
| **D**elete | DELETE | `/api/konyvek/:id` | DELETE | Könyv törlése |


***

# Postman tesztek:

### CREATE (Új könyv)

- **POST** `http://localhost:3000/api/konyvek`
- **Body** → **raw** → **JSON**:

```json
{
    "Cim": "Tesztkönyv",
    "Szerzo": "Test Szerző",
    "Kiadasi_Ev": 2024,
    "Ar": 5000,
    "Raktaron": true
}
```


### READ (Összes)

- **GET** `http://localhost:3000/api/konyvek`
### READ (Egy könyv)

- **GET** `http://localhost:3000/api/konyvek/5`
### UPDATE (Módosítás)

- **PUT** `http://localhost:3000/api/konyvek/5`
- **Body** → **raw** → **JSON**:

```json
{
    "Cim": "Módosított cím",
    "Szerzo": "Gárdonyi Géza",
    "Kiadasi_Ev": 1901,
    "Ar": 4000,
    "Raktaron": true
}
```


### DELETE (Törlés)

- **DELETE** `http://localhost:3000/api/konyvek/5`



