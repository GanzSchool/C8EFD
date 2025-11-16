# Bevezetés a NodeJS + MSSql konfigárcióba | C8EFD

> [!NOTE] 
> **SQL Server 2022 Express - BASIC Telepítés (HA MÉG NINCS TELEPÍTVE)**

> [!CAUTION]
> **Ha jelszóra van szükséged, kérlek mindenképp kérd a `TANÁR ÚR SEGÍTSÉGÉT`!!!**

> 1. Letöltés

   [Telepítő letöltése](https://go.microsoft.com/fwlink/p/?linkid=2216019&clcid=0x40e&culture=hu-hu&country=hu)

> 2. Telepítő futtatása

    Futtasd a letöltött SQLServer2022-SSEI-Expr.exe fájlt.

> 3. Válaszd a "Basic" opciót

> A telepítő három lehetőséget kínál

 - Basic ← Ezt válaszd!
     
 - Custom
     
 - Download Media
    

> Kattints a **Basic** gombra​

> 4. Licenc elfogadása

    Olvasd el és fogadd el a licencfeltételeket az Accept gombbal​

> 5. Telepítési hely

    Hagyd az alapértelmezett értéket, és kattints az Install gombra Nem kell semmilyen függőség!!!! Csak simán az INSTALL GOMB!!!!!​
---
##  RÉSZ 1: SQL SERVER ALAPBEÁLLÍTÁSOK

> [!WARNING]
> **Fontos, hogy ezt a telepítéstől függetlenül el kell végezni!**

### Lépés 1: TCP/IP Protokoll Engedélyezése

    1. Nyomd meg: **Win + R**
    2. Írd be: `SQLServerManager16.msc`
    3. Kattints **OK**
    4. Bal oldalon: **SQL Server Network Configuration** → **Protocols for SQLEXPRESS**
    5. Jobb oldalon: **Jobb klikk a TCP/IP-n** → **Enable**
    6. Egy figyelmeztetés ablak jön → kattints **OK**

### Lépés 2: Statikus Port Beállítása (1433)

    1. Ugyanott: **Jobb klikk a TCP/IP-n** → **Properties**
    2. Menj az **IP Addresses** fülre
    3. Görgess le egészen legalulra az **IPAll** szekcióhoz
    4. **TCP Dynamic Ports**: Töröld ki a számot (maradjon üres)
    5. **TCP Port**: Írd be: `1433`
    6. Kattints **OK**

###  Lépés 3: SQL Server Újraindítása

    1. Bal oldalon: **SQL Server Services**
    2. Jobb oldalon: **SQL Server (SQLEXPRESS)** → **Jobb klikk** → **Restart**
    3. Várj, amíg a Status **Running** lesz

***

## RÉSZ 2: MSSQL ADATBÁZIS LÉTREHOZÁSA

###  Lépés 4: SQL Server Management Studio (SSMS) Megnyitása

    1. Start menü → Keresd: **Microsoft SQL Server Management Studio**
    2. **Connect to Server** ablakban:
        - **Server name**: `localhost\SQLEXPRESS`
        - **Authentication**: `Windows Authentication`
        - Kattints **Connect**

###  Lépés 5: SQL Felhasználó Létrehozása

1. Kattints a **New Query** gombra (felül, bal oldalt)
2. Másold be ezt a kódot:
```sql
USE master;
GO

-- SQL felhasználó létrehozása
CREATE LOGIN admin WITH PASSWORD = '1234';
GO

-- Adatbázis létrehozása
CREATE DATABASE KonyvtarDB;
GO

-- Felhasználó hozzáadása az adatbázishoz
USE KonyvtarDB;
GO

CREATE USER admin FOR LOGIN admin;
GO

ALTER ROLE db_owner ADD MEMBER admin;
GO
```

3. Kattints az **Execute** gombra (F5 vagy zöld nyíl ikon)
4. Alul látod: **"Query executed successfully"**

###  Lépés 6: Tábla Létrehozása és Adatok Beszúrása

1. Ugyanabban a Query ablakban töröld ki a kódot
2. Másold be ezt:
```sql
USE KonyvtarDB;
GO

-- Tábla létrehozása
CREATE TABLE Konyvek (
    KonyvID INT PRIMARY KEY IDENTITY(1,1),
    Cim NVARCHAR(100) NOT NULL,
    Szerzo NVARCHAR(50) NOT NULL,
    Kiadasi_Ev INT,
    Ar INT,
    Raktaron BIT
);
GO

-- 10 könyv beszúrása
INSERT INTO Konyvek (Cim, Szerzo, Kiadasi_Ev, Ar, Raktaron) VALUES
('A Gyűrűk Ura', 'J.R.R. Tolkien', 1954, 4500, 1),
('Harry Potter és a Bölcsek Köve', 'J.K. Rowling', 1997, 3200, 1),
('1984', 'George Orwell', 1949, 2800, 1),
('A Kis Herceg', 'Antoine de Saint-Exupéry', 1943, 2100, 1),
('Egri Csillagok', 'Gárdonyi Géza', 1901, 3500, 0),
('Az Arany Ember', 'Jókai Mór', 1872, 2900, 1),
('Pál utcai fiúk', 'Molnár Ferenc', 1906, 2400, 1),
('A Dzsungel Könyve', 'Rudyard Kipling', 1894, 2600, 0),
('Rómeó és Júlia', 'William Shakespeare', 1597, 3100, 1),
('Don Quijote', 'Miguel de Cervantes', 1605, 3800, 1);
GO

-- Ellenőrzés
SELECT * FROM Konyvek;
GO
```

3. Kattints **Execute** (F5)
4. Alul a **Results** fülön látod a 10 könyvet

***




