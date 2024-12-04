# Introduktion till SQL

Structured Query Language, förkortat SQL, är ett språk som databaser använder för kommunikation. Med SQL så kan man hämta data, ladda upp data, uppdatera data, radera data och mer. Med andra ord så är SQL språket som databasen förstår och utvecklare måste använda för att kommunicera med databasen. 

En av de enklare sakerna man kan göra är att hämta data och det kan se ut såhär:
```sql
SELECT * FROM users;
```
(Det skulle hämta all information om användare. I detta fallet så har en 'users' tabell skapats tidigare.)

I SQL så finns det inbyggda nyckelord som gör olika saker och dessa kombineras till mening-liknande sekvenser. Ta ett annat exempel som visar på det:
```sql
SELECT * FROM products WHERE price > 100 AND category = 'food';  
```
(Det skulle hämta alla produkter som kostar över 100 och ingår i en mat kategori. Igen, här måste man anta att det redan finns en 'products' tabell med data i.)

En sådan här "mening" eller "sökning" kallas en query.

## CRUD

CRUD står för Create, Read, Update Delete. Dessa är termer som används för att beskriva vanliga operationer för databaser (de används mycket inom backend utveckling). För att ladda upp data (CREATE) med SQL så använder man `INSERT`:
```sql
INSERT INTO people VALUES ('Ironman', 45, 'tony@stark.com'):
```

För att hämta data (READ) så använder man `SELECT`:
```sql
SELECT name, email, age FROM people:
```

För att uppdatera existerande data (UPDATE) så använder man `UPDATE`:
```sql
UPDATE people SET age = 46 WHERE name = 'Ironman':
```

För att radera data (DELETE) så använder man `DELETE FROM`:
```sql
DELETE FROM people WHERE name = 'Ironman':
```

Man kan säga att dessa är basen för alla queries. Det finns andra nyckelord som man kan använda för att bygga på och skapa mer avancerade queries, exempelvis villkor och funktioner. Det finns även annan funktionalitet, utöver dessa bas-queries. Läs vidare i andra dokument för mer information. 

Fler exempel:

```sql
-- CREATE (INSERT)
-- Specificera kolumner explicit
INSERT INTO people (name, age, email) 
VALUES ('Ironman', 45, 'tony@stark.com');

-- Flera rader samtidigt
INSERT INTO people (name, age, email) VALUES 
    ('Ironman', 45, 'tony@stark.com'),
    ('Spiderman', 19, 'peter@parker.com');

-- READ (SELECT)
-- Grundläggande select
SELECT name, email FROM people;

-- Med villkor
SELECT * FROM people 
WHERE age >= 18 
ORDER BY name;

-- UPDATE
-- Säkrare uppdatering med flera villkor
UPDATE people 
SET 
    age = 46,
    email = 'tony.stark@avengers.com'
WHERE id = 1 AND name = 'Ironman';

-- DELETE
-- Säker radering med primärnyckel
DELETE FROM people 
WHERE id = 1;
```

## Funktioner och villkor

Villkor:
```sql
-- Grundläggande jämförelser
SELECT * FROM products WHERE price > 100;
SELECT * FROM users WHERE age >= 18;
SELECT * FROM orders WHERE status = 'pending';

-- AND och OR
SELECT * FROM products 
WHERE price > 100 AND category = 'electronics';

SELECT * FROM users 
WHERE age < 18 OR age > 65;

-- IN och BETWEEN
SELECT * FROM products 
WHERE category IN ('food', 'drinks');

SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

-- LIKE för textsökning
SELECT * FROM users 
WHERE email LIKE '%@gmail.com';
```

Funktioner:
```sql
-- Räkna rader
SELECT COUNT(*) FROM users;

-- Summera värden
SELECT SUM(price) FROM orders;

-- Genomsnitt
SELECT AVG(age) FROM users;

-- Gruppera data
SELECT category, COUNT(*) as product_count 
FROM products 
GROUP BY category;
```

## Syntax och regler

```sql
-- SQL är inte skiftlägeskänsligt
SELECT * FROM users;
select * from users;  -- Fungerar också men mindre läsbart

-- Semikolon avslutar statements
SELECT name FROM users;
SELECT age FROM users;

-- Strängar använder enkla citattecken
SELECT * FROM users WHERE name = 'John';

-- Siffror skrivs utan citattecken
SELECT * FROM products WHERE price > 100;
```
