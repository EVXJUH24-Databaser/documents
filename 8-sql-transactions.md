# SQL transactions

I vissa situationer behöver man som utvecklare köra flera queries i rad, som är beroende av varandra. Men i fall går saker snett. Transaktioner är som en slags "undo" knapp.

En transaktion är en serie av databasoperationer som behandlas som en enda enhet. Antingen genomförs alla operationer i transaktionen, eller ingen av dem. Detta följer ACID-principerna:

- **Atomicity** (Atomäritet): Alla operationer lyckas, eller ingen
- **Consistency** (Konsistens): Databasen förblir i ett giltigt tillstånd
- **Isolation** (Isolation): Transaktioner påverkar inte varandra
- **Durability** (Hållbarhet): Genomförda ändringar är permanenta

Innan en transaction appliceras så sparas all information, sedan körs alla queries, och vid slutet när alla queries är färdiga så färdigställs operationen. Om något skulle gå fel så återställs databasen till så den var innan hela transaktionen påbörjades. På så sätt kan man implementera en slags felhantering direkt i databasen; om man vet att man kommer behöva göra några queries som måste utföras i ordning, och något sedan skulle förhindra det (det uppstår ett fel), så förlorar man ingen data och det skapas även ingen icke-använd data eftersom transaktionen helt har avbrutit och återställts.

## Grundläggande syntax

```sql
-- Starta transaktion
BEGIN;

-- Utför operationer
UPDATE konton SET saldo = saldo - 1000 WHERE id = 1;
UPDATE konton SET saldo = saldo + 1000 WHERE id = 2;

-- Slutför transaktion
COMMIT;

-- Eller ångra vid fel
ROLLBACK;
```

## Praktiska exempel

### Överföring mellan Konton

```sql
BEGIN;

-- Kontrollera tillräckligt saldo
DO $$
DECLARE
    saldo_finns DECIMAL;
BEGIN
    SELECT saldo INTO saldo_finns FROM konton WHERE id = 1;
    IF saldo_finns < 1000 THEN
        RAISE EXCEPTION 'Otillräckligt saldo';
    END IF;
END $$;

-- Genomför överföring
UPDATE konton SET saldo = saldo - 1000 WHERE id = 1;
UPDATE konton SET saldo = saldo + 1000 WHERE id = 2;

COMMIT;
```

### Orderhantering

```sql
BEGIN;

-- Skapa order
INSERT INTO orders (customer_id, order_date)
VALUES (1, CURRENT_TIMESTAMP)
RETURNING id INTO order_id;

-- Lägg till orderprodukter
INSERT INTO order_items (order_id, product_id, quantity)
VALUES 
    (order_id, 1, 2),
    (order_id, 3, 1);

-- Uppdatera lagersaldo
UPDATE products
SET stock = stock - 2
WHERE id = 1;

UPDATE products
SET stock = stock - 1
WHERE id = 3;

COMMIT;
```

## Felhantering

```sql
BEGIN;

DO $$
BEGIN
    -- Försök utföra operationer
    UPDATE konton SET saldo = saldo - 1000 WHERE id = 1;
    
    -- Kontrollera resultat
    IF NOT FOUND THEN
        RAISE EXCEPTION 'Konto finns inte';
    END IF;
    
    -- Fortsätt med andra operationer
    UPDATE konton SET saldo = saldo + 1000 WHERE id = 2;
    
EXCEPTION WHEN OTHERS THEN
    -- Vid fel, ångra allt
    RAISE NOTICE 'Ett fel uppstod: %', SQLERRM;
    ROLLBACK;
    RETURN;
END $$;

COMMIT;
```

Kom ihåg att transaktioner är viktiga för databasintegritet men kan påverka prestanda vid felaktig användning.
