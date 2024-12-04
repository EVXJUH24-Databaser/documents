# SQL relationer

I de flesta applikationer så finns det data som relaterar till varandra. Tänk att du skriver en blogg:

På bloggen så kan användare ladda upp inlägg, och kommentarer kan skrivas på inläggen.

Det finns en 'Blogg' med 'Kommentar' och det finns även 'Användare'. Varje kommentar är kopplad till ett blogginlägg. Detta är med andra ord en relation. Eftersom en kommentar tillhör ett blogginlägg så är den relaterad till blogginlägget. Kommentarer är även skrivna av användare så det finns en relation mellan dem också, vilket även stämmer för blogginlägg och användare. Relationerna är:
- Användare <-> Blogg
- Kommentar <-> Blogg
- Användare <-> Kommentar

Med kod så skulle det kunna se ut såhär:

```java
class User {
	String name;
	List<Comment> comments;
	List<BlogPost> posts;
}

class Comment {
	String content;
	User creator;
	BlogPost post;
}

class BlogPost {
	String title;
	String content;
	User creator;
	List<Comment> comments;
}
```

Här så innehåller alla blogginlägg kommentarer, och de kopplar även till en användare. Men i SQL databaser så fungerar det lite annorlunda. Varje klass (kallad 'Entity' eller 'Model') får en egen tabell. Sedan, för att hantera relationer, så skapar man vad som kallas för 'nycklar'. För exemplet ovan så hade det funnits tre tabeller: 'user', 'comment' och 'bloggpost'.

Det finns två typer av nycklar: primary och foreign. En primary key är ett identifierande värde för ett objekt. (Primary-)Nyckeln för ett blogginlägg skulle exempelvis kunna vara titeln. Det är även vanligt att skapa nya nycklar för objekt. Det kan se ut såhär:

```java
class User {
	int id;
	String name;
	List<Comment> comments;
	List<BlogPost> posts;
}

class Comment {
	int id;
	String content;
	User creator;
	BlogPost post;
}

class BlogPost {
	int id;
	String title;
	String content;
	User creator;
	List<Comment> comments;
}
```

I fallet ovanför så namnger vi den primära nyckeln 'id' vilket är väldigt vanligt. Id står för identifier. Nyckeln kan vara vad som helst, men typiskt sätt så är det ett nummer eller en speciell typ som exempelvis 'GUID' eller 'UUID'. Det viktiga med primära nycklar är att de är unika. Det får inte finnas två objekt (eller 'rader') med samma nyckel. Av den anledningen så används ofta 'GUID' och 'UUID' då de kan genereras helt slumpmässigt utan att krocka med andra värden. Av samma anledning kan 'int' användas då den alltid kan räknas uppåt: 1..2..3..4..5... och så vidare.

En foreign key är en referens till en primary key. Ta exemplet från tidigare med bloggen: 

Om du vill skapa tabeller för modellerna så skulle 'User' och 'Comment' se ut såhär:

```
USER
| id | name     |
| 0  | Ironman  |
| 1  | Superman |

COMMENT
| id | content | creator |
| 0  | Hej!    | ?       |
| 1  | Hallå!  | ?       |
```

Eftersom 'creator' är en 'User', och 'User' är en egen tabell, så måste vi ha något sätt att referera till en viss user för 'Comment'. Här använder man en foreign key:
```
USER
| id | name     |
| 0  | Ironman  |
| 1  | Superman |

COMMENT
| id | content | creator |
| 0  | Hej!    | 1       | // 'Superman' skapade denna kommentar
| 1  | Hallå!  | 0       | // 'Ironman' skapade denna kommentar
```

Tanken här är att vi kopplar till användaren genom id:t. Som exempel, om 'creator' är 1 så länkar det till användaren med id 1, vilket är 'Superman'. Värdet som står i 'creator' kallas då foreign key, och de kopplar alltid till en primary key. Den primära nyckeln i detta fallet, för 'User', är då 'id'. För att förtydliga, i tabellerna ovanför så finns det fyra primary keys och två foreign keys:
- Två användare med varsitt 'id' = två primary keys
- Två kommentarer med varsitt 'id' = två primary keys
- Två kommentarer med varsin 'creator' = två foreign keys

Varje rad i en tabell har alltid en primary key, men foreign keys är helt frivilliga. Man har foreign keys om man vill ha relationer, vilket man dock oftast vill.

## Typer

Man kan skilja på olika typer av relationer:
- One-to-One - en rad refererar till en annan rad
- One-to-Many - en rad refererar till flera rader
- Many-to-One (samma som One-to-Many men från andra hållet, om man byter perspektiv)
- Many-to-Many - flera rader refererar till flera rader

Dessa refererar till hur relationerna ser ut. Ta exemplet från tidigare med bloggen igen:
```
Varje användare kan skriva flera kommentarer och skapa flera inlägg. Med andra ord så tillhör varje inlägg en användare och varje kommentar tillhör en användare. 
Relationen mellan användare och blogginlägg blir One-to-Many. 'One' representerar användare och 'Many' representerar blogginlägg. 
Relationen mellan användare och kommentarer blir One-to-Many. 'One' representerar användare och 'Many' representerar kommentarer. 

Varje kommentar tillhör ett blogginlägg. Med andra ord så har varje blogginlägg flera kommentarer.
Relationen mellan blogginlägg och kommentarer blir One-to-Many.
```

Ett annat exempel, helt orelaterad, med GitHub är:
```
Varje användare kan ha flera repos, och varje repo kan ha flera användare.
Relationen mellan användare och repo blir Many-to-Many. En användare -> flera repos, ett repo -> flera användare.
```

Ett annat exempel: Om vi skulle säga att varje användare har max en email så skulle relationen bli One-to-One eftersom det är en användare som kopplar till just en email. Som ett annat exempel, om varje användare kan ha flera emails och användare även kan dela emails - med andra ord att en email kan vara kopplad till flera användare - så skulle relationen bli Many-to-Many. Many-to-Many relationen skulle kunna se ut såhär i kod:
```java
public class Email {
	int id;
	String address;
	List<User> users;
}

public class User {
	int id;
	String name;
	List<Email> emails;
}
```

## Praktiska exempel

### Bloggplattform

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255) UNIQUE
);

CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200),
    content TEXT,
    author_id INTEGER REFERENCES users(id)
);

CREATE TABLE comments (
    id SERIAL PRIMARY KEY,
    content TEXT,
    post_id INTEGER REFERENCES posts(id),
    user_id INTEGER REFERENCES users(id)
);
```

### E-handelsplattform

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    order_date TIMESTAMP,
    customer_id INTEGER REFERENCES customers(id)
);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200),
    price DECIMAL(10,2)
);

CREATE TABLE order_items (
    order_id INTEGER REFERENCES orders(id),
    product_id INTEGER REFERENCES products(id),
    quantity INTEGER,
    PRIMARY KEY (order_id, product_id)
);
```

## Best practices

1. Använd alltid primärnycklar
2. Namnge foreign keys konsekvent (t.ex. `tabell_id`)
3. Sätt lämpliga restriktioner (ON DELETE, ON UPDATE)
4. Indexera foreign keys för bättre prestanda
5. Använd junction-tabeller för many-to-many relationer

## Vanliga SQL-operationer med relationer

```sql
-- Hämta alla användare med deras inlägg
SELECT users.name, posts.title
FROM users
LEFT JOIN posts ON posts.author_id = users.id;

-- Räkna inlägg per användare
SELECT users.name, COUNT(posts.id) as post_count
FROM users
LEFT JOIN posts ON posts.author_id = users.id
GROUP BY users.id, users.name;

-- Hitta alla projekt en användare deltar i
SELECT projects.name
FROM projects
INNER JOIN user_projects ON user_projects.project_id = projects.id
WHERE user_projects.user_id = 1;
```

Kom ihåg att väl designade relationer är grunden för en effektiv databas. Det är viktigt att ta tid att planera din databasstruktur och relationer innan implementation.
