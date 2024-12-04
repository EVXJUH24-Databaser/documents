# SQL joins

För att hämta data ifrån en tabell så använder man `SELECT`, men vad händer om datan som man vill hämta finns i två olika tabeller? Hela poängen med SQL är att man kan använda relationer mellan tabeller för att hämta all data man behöver i ett enda anrop. För att komma åt flera tabeller i samma query så använder man 'joins'.

Som exempel, säg att du har två tabeller:

```
PETS
| id | name      | favoriteFood | type  |
| 1  | Ben       | banana       | horse | 
| 2  | Supergirl | apple        | dog   |
| 3  | Hulk      | candy        | dog   |
| 4  | Voldemort | cucumber     | cat   |

CUSTOMERS
| id | name    | email           | petId |
| 1  | Bob     | bob@mail.com    | 2     |
| 2  | Amanda  | amanda@mail.com | 1     |
| 3  | Alex    | alex@mail.com   | 4     |
| 4  | Rachel  | rachel@mail.com | 3     |
```

Säg också att du får tillgång till ett namn på en 'customer': Amanda. Hur, genom databasen och en query, får du reda på vad Amandas husdjur heter? Namnet på personen är 'Amanda' och det ligger i 'customers' tabellen, men namnet på husdjuret ligger i en annan tabell 'pets'. För att komma åt båda två så används en join, och det kan se ut såhär för exemplet:

```postgresql
SELECT pets.name FROM pets INNER JOIN customers ON pets.id = customers.petId WHERE customers.name = 'Amanda'; 
```

Låt oss bryta ned det:
1. `SELECT pets.name FROM pets` - Hämta ut namn på husdjur från 'pets' tabell
2. `INNER JOIN customers ON pets.id = customers.petId` - Slå ihop 'pets' med 'customers' så att det matchar med `pets.id` och `customers.petId`, vilket resulterar i:
```
PETS + CUSTOMERS (JOIN)
|*id*| name      | favoriteFood | type  | id | name    | email           |*petId*|
| 1  | Ben       | banana       | horse | 2  | Amanda  | amanda@mail.com | 1     | 
| 2  | Supergirl | apple        | dog   | 1  | Bob     | bob@mail.com    | 2     |
| 3  | Hulk      | candy        | dog   | 4  | Rachel  | rachel@mail.com | 3     |
| 4  | Voldemort | cucumber     | cat   | 3  | Alex    | alex@mail.com   | 4     |
```
3. `WHERE customers.name = 'Amanda'` - Sök efter 'Amanda'

Med andra ord, slå ihop tabellerna, hämta raden för 'Amanda' och returnera namnet på husdjuret.

## Typer av joins

### INNER JOIN

Returnerar endast matchande rader från båda tabellerna. Om två rader inte matchar så ignoreras raden.

```sql
SELECT customers.name, pets.name 
FROM customers 
INNER JOIN pets ON pets.id = customers.petId;
```

### LEFT JOIN (LEFT OUTER JOIN)

Returnerar alla rader från vänstra tabellen och matchande rader från högra tabellen. Om två rader inte matchar så inkluderas fortfarande den vänstra tabellen.

```sql
SELECT customers.name, pets.name 
FROM customers 
LEFT JOIN pets ON pets.id = customers.petId;
```

### RIGHT JOIN (RIGHT OUTER JOIN)

Returnerar alla rader från högra tabellen och matchande rader från vänstra tabellen. Om två rader inte matchar så inkluderas fortfarande den högre tabellen. (Detta är samma som LEFT JOIN fast i andra riktningen.)

```sql
SELECT customers.name, pets.name 
FROM customers 
RIGHT JOIN pets ON pets.id = customers.petId;
```

### FULL JOIN (FULL OUTER JOIN)

Returnerar alla rader när det finns en match i någon av tabellerna.

```sql
SELECT customers.name, pets.name 
FROM customers 
FULL JOIN pets ON pets.id = customers.petId;
```

## Praktiska exempel

Låtsas att du har följande tabeller:

```
PETS
| id | name      | favoriteFood | type  |
| 1  | Ben       | banana       | horse | 
| 2  | Supergirl | apple        | dog   |
| 3  | Hulk      | candy        | dog   |
| 4  | Voldemort | cucumber     | cat   |

CUSTOMERS
| id | name    | email           | petId |
| 1  | Bob     | bob@mail.com    | 2     |
| 2  | Amanda  | amanda@mail.com | 1     |
| 3  | Alex    | alex@mail.com   | 4     |
| 4  | Rachel  | rachel@mail.com | 3     |
```

### Exempel på queries

```sql
-- Hitta alla kunder och deras husdjur
SELECT 
    customers.name as customer_name,
    pets.name as pet_name,
    pets.type
FROM customers
INNER JOIN pets ON pets.id = customers.petId;

-- Hitta alla hundar och deras ägare
SELECT 
    customers.name as owner,
    pets.name as dog_name
FROM pets
INNER JOIN customers ON customers.petId = pets.id
WHERE pets.type = 'dog';

-- Visa alla kunder, även de utan husdjur
SELECT 
    customers.name,
    COALESCE(pets.name, 'Inget husdjur') as pet
FROM customers
LEFT JOIN pets ON pets.id = customers.petId;
```

## Tips för joins

1. Använd alltid tydliga tabellnamn eller alias
2. Var noga med att välja rätt typ av join för ditt användningsfall
3. Tänk på prestanda - joins kan vara resurskrävande på stora tabeller
4. Använd WHERE-villkor efter JOIN-satsen för att filtrera resultatet

## Visualisering av join typer

```
INNER JOIN       LEFT JOIN        RIGHT JOIN       FULL JOIN
   A ∩ B           A U (A ∩ B)      (A ∩ B) U B      A U B
    ###             #####                ###          #####
  ##***##         ##***##            ##***##       ##***##
    ###             ###                #####         #####
```

Där:
- \# representerar rader som endast finns i en tabell
- \* representerar matchande rader mellan tabellerna

Kom ihåg att joins är ett kraftfullt verktyg för att kombinera data från olika tabeller, men använd dem med endast om nödvändigt, för att undvika komplexitet och prestandaproblem.