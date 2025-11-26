# Bevezetés a NodeJS + MSSql konfigárcióba | C8EFD

## Tábla Létrehozása és Adatok Beszúrása

```sql
-- Adatbázis létrehozása
CREATE DATABASE KonyvtarDB;
GO

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




