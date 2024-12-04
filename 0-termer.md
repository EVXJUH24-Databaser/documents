# Termer

## Databas

Ett program som är till för att lagra massor av data.

## Tabell

Ett del i en **databas** som sparar data av en viss typ, exempelvis kunder, bilder och blogginlägg.

Tabeller innehåller **rader** och **kolumner**.

## Query

Ett meddelande som skickas till en **databas** för att hämta data, ladda upp data, ändra data eller radera data.

## SQL (Structured Query Language)

Ett databas språk som vissa databaser använder för sina **queries**.

## Relation

En koppling mellan två **tabeller** i en **databas**.

Det finns olika typer av relationer:

- One-to-One
- One-to-Many
- Many-to-One
- Many-to-Many

## CRUD

En förtkortning för: **c**reate, **r**ead, **u**pdate & **d**elete. Dessa är vanligt förekommande operationer vid databasanvändning. Varje del har ett eget nyckelord i **SQL**.

## INSERT

En **SQL query** som laddar upp data till en **databas**.

C:et i CRUD.

## SELECT

En **SQL query** som laddar upp data från en **databas**.

R:et i CRUD.

## UPDATE

En **SQL query** som ändrar/uppdaterar data i en **databas**.

U:et i CRUD.

## DELETE

En **SQL query** som raderar data från en **databas**.

D:et i CRUD.

## Rad (row)

Ett "object" i **databas** sammanhang. Rader är databas-versionen av kod objekt. En rad innehåller flera egenskaper/värden/**kolumner** av olika datatyper.

## Kolumn (column)

En egenskap för en **rad** i **databas** sammanhang. Kolumner är databas-versionen av fält i objekt. En kolumn innehåller ett värde av en viss datatyp.

## Key (primary key & foreign key)

**Primary key** är ett värde som identifierar en **rad**. Den har ofta en egen **kolumn**.

**Foreign key** är ett värde som refererar till en **primary key** i en annan **rad**, oftast annan tabell. De har ofta en egen **kolumn**.

## Transaction

En lista på **SQL queries** som skall utföras i ordning.

## Normalisering

Ett sätt att designa och strukturera **tabeller** som uppmuntrar integritet och förhindrar data duplicering.

## Join

Ett nyckelord som används i **queries** för att slå ihop två eller flera **tabeller** för att komma åt datan mellan dem.

## Docker

Ett verktyg/program som används för att installera andra program, exempelvis (och för vår användning) databaser.

## ORM

Förkortning för object-relational mapping. En ORM är ett verktyg som automatiskt omvandlar kod till **SQL tabeller** och **queries**.

Två exempel på ORMs är:

- Entity Framework (EF) - C#
- Hibernate - Java

## Hashing

En process som omvandlar strängar, med en speciell algoritm, till nya strängar som verkar slumpade. Det går inte att få tillbaka till den originella strängen. Detta används exempelvis i databaser för att spara och säkra lösenord.
