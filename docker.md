# Docker

Docker är ett system som används för att hantera olika program. Det är bra för att hantera databaser och servrar exempelvis. Docker, eller liknande system, är även vad många molntjänster använder för att köra program.

I denna kurs så kan vi använda Docker för att enkelt installera en eller flera databaser. 

För att använda Docker lokalt så behöver det installeras först. Gör det genom Dockers webbplats. Man kan använda terminalen eller Docker Desktop. Docker Desktop är enklare för de flesta, speciellt för de som är ovana med terminalen. Docker terminal är dock bättre träning. Avgör själv vad som passar dig bäst.

## Koncept

Det finns två koncept som är viktiga att förstå: containers och images.

Containers är program som körs i Docker. De fungerar för det mesta som vanliga program (Teams, VSCode, Webbläsare) men hanteras lite annorlunda eftersom de körs innanför Docker. Man kan starta och stoppa containers precis som med vanliga program. Varje container är isolerad och har sitt eget filsystem. Säkerheten ökas betydligt på grund av detta, då kod i en container inte enkelt kan nå andra saker utanför containern.

Images är mallar för containers. När man vill starta en container så gör man det genom en "image". Man skulle kunna säga att en image är koden för ett program. Om man exempelvis vill starta en databas så måste man först ladda ned en image för databasen (exempelvis MySQL eller MongoDB). Sedan så kan man starta databasen (en container) utifrån image:n som man laddade ned. Det blir väldigt lätt att hantera olika versioner med images och man får sällan konflikter i containers eftersom de alltid körs med samma image (version).

## Docker Terminal

Ladda ned images med: `docker pull <image name>`.

Lista alla nedladdade images med: `docker images`.

Skapa och starta en ny container ifrån image med: `docker run --name <container name> -d mongo`.

Stoppa container med: `docker stop <container name>`.

Starta existerande container med: `docker start <container name>`.

Se startade containers med: `docker ps`.

För fler kommandon: `docker --help`

Exempel med MongoDB:
1. Hämta MongoDB [image](https://hub.docker.com/_/mongo/): `docker pull mongo`
2. Se att den laddades ned: `docker images`
3. Starta MongoDB container: `docker run --name my-mongo-db -d mongo`
4. Se att den har startat: `docker ps`
5. (Vid behov) Stoppa container: `docker stop my-mongo-db`
6. Starta container igen (om den är stoppad): `docker start my-mongo-db`
7. Anslut till MongoDB shell (för att modifiera databas): `docker exec -it my-mongo-db bash` och sedan `mongosh`
8. Gå ut ur shell: `exit` och sedan `exit` igen