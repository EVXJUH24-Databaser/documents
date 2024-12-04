# SQL tabeller

En tabell är en del av en databas som sparar data av en viss struktur. Den kan exempelvis spara användare, blogginlägg, kommentarer och bilar, och egentligen vad som helst.

Som exempel, säg att du vill skriva ett program som behöver spara kunder och produkter. Då hade du skapat en tabell för att hålla kunderna och en tabell för att hålla produkterna.

## Struktur

En tabell har två delar:
- Kolumner (columns) - En kolumn är en egenskap/property som en rad innehåller
- Rader (rows) - En rad innehåller värden för de egenskaper som kolumnerna definierar

Exempel:
```
PRODUCT
| id | name     | description | price |
| 1  | Cola     | Dryck 33cl  | 15    |
| 2  | Pepsi    | Dryck 33cl  | 15    |
| 3  | Snickers | Chokladbar  | 12    |
```

Ovan visualiserar en tabell som heter "PRODUCT". Den har fyra kolumner (id, name, description och price) och tre rader med produkter. Varje rad har sina egna värden för varje kolumn:
- Rad 1: id=1, name=Cola, description=Dryck 33cl, price=15
- Rad 2: id=2, name=Pepsi, description=Dryck 33cl, price=15
- Rad 3: id=3, name=Snickers, description=Chokladbar, price=12

## Datatyper

Alla kolumner har en specifik datatyp. En kolumn som ska innehålla text har datatypen "text" eller "varchar". En kolumn som ska innehålla siffror har datatypen "integer" eller "decimal". Några vanliga datatyper är:

- text/varchar - för text
- integer/int - för heltal
- decimal/numeric - för decimaltal
- boolean - för sant/falskt
- date - för datum
- timestamp - för datum och tid

När man skapar en tabell så måste man ange datatyp för varje kolumn. Det kan se ut såhär:

```sql
CREATE TABLE product (
    id INTEGER,
    name VARCHAR(255),
    description TEXT,
    price DECIMAL,
    inStock BOOLEAN,
    created TIMESTAMP
);
```

## Primary Key

En primary key är en kolumn som unikt identifierar en rad i tabellen. Som exempel, i tabellen ovan så är "id" en primary key. Det betyder att ingen annan rad i tabellen kan ha samma id. Primary keys används ofta när man vill referera till en specifik rad i tabellen.

När man skapar en tabell med primary key så ser det ut såhär:

```sql
CREATE TABLE product (
    id INTEGER PRIMARY KEY,
    name VARCHAR(255),
    description TEXT,
    price DECIMAL
);
```

## Foreign Key

En foreign key är en kolumn som refererar till en primary key i en annan tabell. Som exempel:

```
PRODUCT
| id | name     | price |
| 1  | Cola     | 15    |
| 2  | Pepsi    | 15    |

ORDER
| id | productId | amount |
| 1  | 1         | 2      |
| 2  | 2         | 1      |
```

I tabellen "ORDER" så är "productId" en foreign key som refererar till "id" i "PRODUCT" tabellen. Det betyder att varje order är kopplad till en specifik produkt.

När man skapar en tabell med foreign key så ser det ut såhär:

```sql
CREATE TABLE order (
    id INTEGER PRIMARY KEY,
    productId INTEGER REFERENCES product(id),
    amount INTEGER
);
```

## Index

Ett index är en datastruktur som gör det snabbare att söka efter data i en tabell. Det fungerar ungefär som ett index i en bok - istället för att behöva läsa hela boken för att hitta något specifikt så kan man kolla i indexet först.

När man skapar ett index så ser det ut såhär:

```sql
CREATE INDEX product_name_idx ON product(name);
```

Detta skapar ett index på kolumnen "name" i tabellen "product". Nu kommer sökningar på produktnamn att gå snabbare.

Primary keys får automatiskt ett index, så man behöver inte skapa det manuellt. Att skapa index kostar mycket så man skall endast skapa index för absolut nödvändiga kolumner.