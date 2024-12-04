# SQL normalisering

Normalisering är ett sätt att strukturera data och designa tabeller. När man skapar tabeller så har man oftast relationer mellan vissa tabeller. För att designa tabellerna så bra som möjligt så skall man använda normalisering. 

Föreställ dig att du har följande tabeller:

```
CUSTOMERS
| name | email | 

ORDERS
| customer | product | amount |

PRODUCTS
| name | description | amount | price |
```

Och sedan följande data:

```
CUSTOMERS
| name    | email           | 
| Ben     | ben@mail.com    | 
| Alfons  | alfons@mail.com | 
| Eva     | eva@mail.com    | 

ORDERS
| customer | product | amount |
| Eva      | Brö     | 2      |
| Eva      | Godis   | 1      |
| Ben      | Brö     | 2      |
| Ben      | Godis   | 3      |
| Alfons   | Brö     | 5      |
| Alfons   | Glass   | 5      |

PRODUCTS
| name  | description      | amount | price |
| Brö   | Gott bröd.       | 40     | 10    |
| Godis | Full av socker.  | 20     | 30    |
| Glass | Päron smak.      | 15     | 5     |
```

Orders har två kolumner med foreign keys: customer som länkar till customer tabellens 'name', och product som länkar till product tabellens 'name'.

Tänk nu att du har en webbsida där kunder kan köpa produkter (därav tabellerna). En dag så upptäcker du att du har råkat lägga in fel namn på en produkt så du går för att ändra den i tabellen:

```
		 PRODUCTS
         | name | description | amount | price |
(gammal) | Brö  | Gott bröd.  | 40     | 10    |
(ny)     | Bröd | Gott bröd.  | 40     | 10    |
```

Allt känns bra. Men fem minuter senare så börjar det trilla in en massa klagomål från kunder om att de inte kan köpa en viss produkt, den produkten som du nyss ändrade på. Problemet som har uppstått är att relationen mellan `orders` och `products` har kapats av. Orders ser nu till del ut såhär:
```
ORDERS
| customer | product | amount |
| Eva      | Brö     | 2      |
| Ben      | Brö     | 2      |
| Alfons   | Brö     | 5      |

PRODUCTS
| name | description | amount | price |
| Bröd | Gott bröd.  | 40     | 10    |
```

Men du ändrade precis namnet på en product (primary key) utan att också ändra namnet på orders foreign key. Med andra ord så ligger en gammal koppling kvar, från en order till en produkt som har fått ett nytt namn. Lösningen här är att också ändra på alla orders som har med den specifika produkten att göra:
```
	 ORDERS
     | customer | product | amount |
(ny) | Eva      | Bröd    | 2      |
     | Eva      | Godis   | 1      |
(ny) | Ben      | Bröd    | 2      |
     | Ben      | Godis   | 3      |
(ny) | Alfons   | Bröd    | 5      |
     | Alfons   | Glass   | 5      |
```

Detta utgör dock två problem. För det första så måste du skicka in en ny query som uppdaterar alla orders, och detta innebär mer belastning på databasen. För det andra så måste du också hålla koll på alla relationer och uppdatera dem om nödvändigt. Här kommer normalisering in.

Normalisering handlar om att förhindra dessa problem genom att strukturera och designa tabeller bättre. Det finns flera versioner av normalisering, och även flera principer, men den viktigaste addresserar problemet som nyss har visats: att relationer försvinner om vi behöver ändra eller radera data.

När man designar tabeller med normalisering så lägger man nästan alltid till en ny kolumn, som ofta får namnet `id`, och den får som ansvar att identifiera raden (e.g. en kund eller produkt). Id:t är ett värde som aldrig får ändras; när det väl har bestämts så ligger samma värde kvar för all tid. Alla relationer kopplas sedan med dessa id:n. För att återgå till exemplet från tidigare så hade de nya tabellera strukturerats såhär:

```
CUSTOMERS
| id | | name | email | 

ORDERS
| id | customerId | productId | amount |

PRODUCTS
| id | name | description | amount | price |
```

Som sedan kan fyllas med data likt tidigare, men med `id` denna gången:

```
CUSTOMERS
| id | name    | email           | 
| 1  | Ben     | ben@mail.com    | 
| 2  | Alfons  | alfons@mail.com | 
| 3  | Eva     | eva@mail.com    | 

ORDERS
| id | customerId | productId | amount |
| 1  | 3          | 1         | 2      |
| 2  | 3          | 2         | 1      |
| 3  | 1          | 1         | 2      |
| 4  | 1          | 2         | 3      |
| 5  | 2          | 1         | 5      |
| 6  | 2          | 3         | 5      |

PRODUCTS
| id | name  | description      | amount | price |
| 1  | Brö   | Gott bröd.       | 40     | 10    |
| 2  | Godis | Fullt av socker. | 20     | 30    |
| 3  | Glass | Päron smak.      | 15     | 5     |
```

Notera också att `orders` har ändrats till `customerId` och `productId`. På det sättet så kan man fritt ändra i vilken kolumn som helst, exempelvis product name, utan att förstöra existerande relationer och kopplingar. Detta är för att relationen skapas genom `id`, som aldrig förändras.
