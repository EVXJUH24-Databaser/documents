# Introduktion till databaser

En databas är ett program som är till för att spara data. Man kan jämföra databaser med filer. I filer kan du ju spara vad som helst: bilder, filmer, text, dokument och mer. Anledningen till att databaser finns är för att hantera mer avancerade saker. Tänk dig att du behöver spara lite information om kunder, som exempelvis deras namn, ålder och email. Säg också att du vill skriva ett program som kan söka efter dessa kunder baserat på olika 'queries' eller sökkriterier, som exempelvis: "Alla kunder som är över 30 år och har 'Adam' som förnamn". Om du skulle försöka göra detta med vanliga filer så kan det bli jobbigt. Om varje kund har en egen fil så måste du söka igenom varje fil, hämta ut egenskaperna och sedan lägga ihop dem så att ointressanta kunder kan filtreras ut. Det är inte omöjligt men poängen är att det blir mycket jobb.

Databaser är till för just detta: att kunna hantera data på ett effektivt och strukturerat sätt. De kan:

- Söka bland stora mängder data snabbt
- Garantera att data är konsistent
- Hantera flera användare samtidigt
- Återställa data vid problem
- Skydda data från obehörig åtkomst

Som ett enkelt exempel så skulle man med SQL (en typ av databas) kunna säga:

```sql
SELECT * FROM customers WHERE age > 30 AND name LIKE 'Adam%';
```

och då skulle databasen hämta alla kunder som stämmer med sökkriterierna. Med andra ord så är alla dessa sökfunktioner inbyggda i databasen så att du slipper implementera dem själv.

## Typer av databaser

Det finns olika typer av databaser som är designade för olika användningsområden. Här är de vanligaste typerna:

### SQL (Relational Databases)

SQL-databaser använder tabeller för att lagra data och relationer mellan dessa tabeller. De är utmärkta när du har strukturerad data med tydliga relationer. Till exempel:

- Kundinformation och deras ordrar
- Produktkataloger med kategorier
- Användare och deras inställningar

Fördelar:

- Stark dataintegritet
- Komplex sökning och filtrering
- Väletablerad standard
- Bra för strukturerad data

Exempel på SQL-databaser:

- PostgreSQL: Öppen källkod, bra funktionalitet, snabb
- MySQL: Populär, enkel att börja med
- SQLite: Lokal fil-baserad databas

### NoSQL

NoSQL-databaser är designade för att hantera ostrukturerad data och situationer där flexibilitet är viktigare än strikt struktur. De delas ofta upp i flera kategorier:

#### Document Databases

Sparar data i dokumentformat (ofta JSON-liknande struktur). Bra för:

- Innehållshantering
- Katalogdata
- Användarprofilering

Exempel:

- MongoDB: Populär dokumentdatabas
- CouchDB: Bra för distribuerade system

#### Key-Value Stores

Mycket enkla och snabba databaser som lagrar data som nyckel-värde par. Perfekta för:

- Sessionshantering
- Användarinställningar
- Shopping carts

Exempel:

- Redis (också in-memory)
- DynamoDB (Amazon)

### Realtime Databases

Designade för applikationer som behöver uppdateras i realtid, som:

- Chat-applikationer
- Spel
- Kollaborativa verktyg

Exempel:

- Firebase Realtime Database
- PubNub

### In-memory Databases

En in-memory databas sparar all information i RAM-minnet, inte på disk. Detta gör dem extremt snabba men datan är temporär - när databasen startas om försvinner all data.

Användningsområden:

- Caching (temporär lagring av ofta använd data)
- Session-hantering
- Realtidsanalyser
- Tillfälliga beräkningar

Exempel:

- Redis: Populär in-memory databas som också kan spara till disk
- Memcached: Enkel men effektiv cache-lösning

## Queries (sökningar)

När du vill hämta eller manipulera data i en databas använder du queries. Olika databastyper har olika sätt att skriva queries:

### SQL Queries

SQL använder ett standardiserat språk. Här är några exempel:

```sql
-- Hämta alla kunder
SELECT * FROM customers;

-- Hämta specifika kunder
SELECT * FROM customers WHERE age > 30;

-- Hämta specifika kolumner
SELECT name, email FROM customers;

-- Joina (koppla ihop) tabeller
SELECT customers.name, orders.total 
FROM customers 
JOIN orders ON customers.id = orders.customer_id;
```

### MongoDB Queries

MongoDB använder ett JSON-liknande format:

```javascript
// Hämta alla kunder
db.customers.find({});

// Hämta specifika kunder
db.customers.find({ age: { $gt: 30 } });

// Hämta specifika fält
db.customers.find({}, { name: 1, email: 1 });

// Mer komplex sökning
db.customers.find({
  age: { $gt: 30 },
  city: "Stockholm",
});
```

## Koppling till kod

För att använda en databas i kod behövs ett bibliotek (driver) som kan kommunicera med databasen. Olika språk har olika bibliotek. Läs "java och postgres" dokumentet för mer information.

## Att välja rätt databas

När du ska välja databas bör du tänka på:

1. **Datastruktur**
   - Har du många relationer? → SQL
   - Flexibel struktur? → Document database
   - Enkel key-value data? → Key-value store

2. **Prestanda**
   - Behöver du extremt snabb åtkomst? → In-memory
   - Stora datamängder? → SQL eller specialized NoSQL
   - Realtidsuppdateringar? → Realtime database

3. **Skalbarhet**
   - Många läsningar? → NoSQL ofta bättre
   - Många skrivningar? → Beror på användningsfall
   - Behov av transaktioner? → SQL

4. **Utvecklingsteam**
   - Erfarenhet av SQL? → Kanske börja där
   - Behov av flexibilitet? → NoSQL kan vara enklare
   - Tid att lära sig? → Välj något välkänt

För nybörjare rekommenderas ofta att börja med en SQL-databas som PostgreSQL eftersom den är modern och snabb med bra funktionalitet. Den har också bra dokumentation och är populär.
