# Java och PostgreSQL

När man arbetar med databaser i Java så behöver man ett sätt att kommunicera med databasen. För PostgreSQL används pgJDBC, vilket är ett bibliotek som installeras via Gradle (eller Maven.) När det är installerat kan vi börja med grunderna - att ansluta till databasen.

Lägg in följande i `build.gradle` (och ladda om Intellij vid behov):

```kts
dependencies {
	// ...

  implementation ("org.postgresql:postgresql:42.7.4")
}
```

En anslutning till PostgreSQL kräver en "connection string", vilket är en text som innehåller information om var databasen finns och hur vi ska logga in. Den ser ut såhär:

```
jdbc:postgresql://host/database?user=user&password=password
```

Vi använder den, med rätt information, för att ansluta till databasen:

```java
// 'testingdb' är namnet på den interna databasen i PostgreSQL som har skapats med `CREATE DATABASE`
String connectionString = "jdbc:postgresql://localhost/testingdb?user=postgres&password=password";
Connection conn = DriverManager.getConnection(connectionString);
```

Låt säga att det finns en tabell i databasen, som redan har lite data, och kan användas i kommande exempel. Den ser ut såhär:

```sql
CREATE TABLE pets (
    id SERIAL PRIMARY KEY,
    name TEXT,
    favoriteFood TEXT,
    type TEXT
);
```

För att arbeta mot databasen genom kod så används flera klasser och metoder. Här är ett exempel som hämtar alla rader i tabellen:

```java
// Skapa anslutningen
// Om detta misslyckas så kastas en exception som kan hanteras med try/catch
Connection conn = DriverManager.getConnection(connectionString);

// Skapa ett 'statement' som används för att exekvera queries
Statement st = conn.createStatement();

// Kör en query
ResultSet rs = st.executeQuery("SELECT * FROM pets");

// Hämta resultat av query
while (rs.next()) {
	// Hämta kolumner med getDatatype(columnName)
  System.out.println("Pet - " + rs.getString("name"));
  System.out.println(" id = " + rs.getInt("id"));
  System.out.println(" type = " + rs.getString("type"));
  System.out.println(" favoriteFood = " + rs.getString("favoriteFood"));
}

// Avsluta query när den är klar
rs.close();
st.close();

// Stäng anslutning till databas när allt är klart
conn.close();
```

Om man kör denna kod mot en databas så kommer den hitta alla husdjur och skriva ut dem.

Detta exempel visar några viktiga koncept:

1. **Connection** - Vi skapar en anslutning till databasen med `DriverManager.getConnection`
2. **Statement** - Vi skapar ett kommando med vår SQL-query
3. **ResultSet** - Vi läser resultatet rad för rad

Man vill ofta kunna köra andra typer av queries, exempelvis `INSERT`, och i många fall så vill man även kunna skicka in argument till queries. För att göra det på ett säkert sätt måste man använda `PreparedStatement` och parametrar:

```java
// Skapa anslutningen
Connection conn = DriverManager.getConnection(connectionString);

// Definiera variabler (dessa kan vara dynamiska och komma från user-input exempelvis)
String petName = "Ironman";
String petType = "Cat";
String petFavoriteFood = "Fish";

// Skapa ett 'prepared statement' och definiera parametrar
// VIKTIGT: Skriv aldrig queries med string concatenation, det är farligt på grund av SQL injections
// Gör exempelvis aldrig såhär: "INSERT INTO pets (name) VALUES (" + petName + ");"
PreparedStatement st = conn.prepareStatement("INSERT INTO pets (name, type, favoriteFood) VALUES (?, ?, ?)");

// Lägg in argument på parametrar (i ordning)
st.setString(1, petName);
st.setString(2, petType);
st.setString(3, petFavoriteFood);

// Kör query, men ignorera resultatet eftersom det endast är en 'INSERT'
// Om något skulle gå fel så kastas en exception och det kan hanteras med try/catch
st.execute();

// Avsluta query när den är klar
st.close();

// Stäng anslutning till databas när allt är klart
st.close();
```

När du arbetar med databaser i Java är det viktigt att tänka på:

1. **Använd alltid parametrar** - Bygg aldrig SQL-strängar genom att sätta ihop text, det kan leda till säkerhetshål
2. **Stäng dina resurser** - Använd `close()` för att stänga anslutningar och andra resurser
3. **Hantera fel** - Använd try-catch för att fånga och hantera potentiella fel

Om man vill så kan man skapa .sql filer, fylla dem med queries, och ladda in dem med kod. Detta är ofta bra om man har många och stora queries.
