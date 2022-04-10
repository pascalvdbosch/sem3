# Andere EF Core Database Providers

## Database Providers

LINQ en EF Core is zo ingericht dat er geen afhankelijkheid is naar een specifiek soort DBMS. Dat betekent dat de code voor een applicatie die gebruik maakt van EF Core als ORM naar SQL Server, (vrijwel) niet aangepast hoeft te worden als de data moet worden opgeslagen in een MySQL database, bijvoorbeeld als de SQL Server licentie van het bedrijf verloopt, of als er dramatisch grote security problemen zijn met SQL Server.&#x20;

Met een _Database Provider_ voor EF Core wordt een bibliotheek bedoeld die EF Core laat werken met een nieuw specifiek soort DBMS. In de [lijst](https://docs.microsoft.com/en-us/ef/core/providers/) van database providers voor EF Core valt op dat er slechts een paar niet voor relationele databases zijn, bijvoorbeeld de NoSQL database Azure Cosmos.&#x20;

## Het installeren van een nieuwe database provider

Als voorbeeld bekijken we de database provider voor PostgreSQL

1. Voeg de assembly reference toe: `dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL`.&#x20;
2. Importeer de namespace: `using Npgsql.EntityFrameworkCore.PostgreSQL`.&#x20;
3. Roep de juiste `Use...` methode aan in de `OnConfiguring` van de `DbContext`:  `UseNpgsql`.&#x20;

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder b) => 
    b.UseNpgsql("...");
```

## Het gebruik van SQLite

Voor de meeste DMBS'en is de installatie ingewikkeld, bestaat de database uit een heleboel bestanden en configuratie, en runt er op de achtergrond een service die luistert, via een specifieke port (voor SQL Server bijvoorbeeld port 1433), naar andere programmatuur die commando's willen uitvoeren of willen queryen. De architectuur die hiervoor wordt gebruikt is Client-Server. De client in dit geval is bijvoorbeeld de webapplicatie en de server is in dit geval de DBMS. In dit soort DBMS'en kan de database op een hele andere locatie staan dan de webapplicatie en is concurrency vaak goed ondersteund.&#x20;

![Dit is niet hoe SQLite werkt](../../.gitbook/assets/clientserversql.svg)

SQLite is slechts een bibliotheekje waarmee programmeurs een relationele database kunnen benaderen, die opgeslagen staat in één bestandje. Er is dus geen process dat query's en commando's accepteert zoals we gewend zijn in Client-Server applicaties.&#x20;

SQLite heeft vooral als voordeel dat het heel lichtgewicht en portable is: slecht een bibliotheekje en één bestandje is genoeg. In embedded applicaties zou SQLite dus goed toegepast kunnen worden, maar ook is het handig tijdens ontwikkeling. Wij gebruiken het vanwege de eenvoud, maar als een webapplicatie in productie is genomen hoort er nooit een SQLite database gebruikt te worden.&#x20;

### De connectie string

In de connectie string hoeft alleen de bestandsnaam te worden aangegeven. Geef deze connectiestring als argument mee aan de `UseSqlite` methode.&#x20;

```
Data Source=databaseje.db;
```

### De SQLite database bekijken

[Dit](https://sqlitebrowser.org) is een heel simpel klein tooltje voor allerlei operating systemen waarmee je SQLite databases kunt openen en bekijken.&#x20;

### De SQLite database bekijken vanuit VS Code

Er is [een extensie](https://marketplace.visualstudio.com/items?itemName=alexcvzz.vscode-sqlite) voor VS Code waarmee je SQL Servers kunt queryen en aanpassen vanuit VS Code.&#x20;

* Zoek op "_SQL Server_" in de Extensions tab van VS Code en klik op "_Install_".&#x20;
* Open de Command Pallete (control-shift-P in Windows), typ "_SQLite_", en klik op "_SQLite: Open Database_".&#x20;
* Selecteer jouw SQLite database bestand.&#x20;
* Klik rechtsonderin op "_SQLite Explorer_".&#x20;

![](<../../.gitbook/assets/image (3).png>)

## Het gebruik van SQL Server

### Installeer SQL Server

* Installeer SQL Server. [Dit](https://www.microsoft.com/nl-nl/sql-server/sql-server-downloads) is de download link en [dit](https://docs.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server) is een uitgebreide handleiding van de installatie.&#x20;
* Om te controleren of SQL Server inderdaad draait, run het commando `sqlcmd` in een nieuwe terminal. SQL Server is geinstalleerd en staat aan als je geen error krijgt. Typ bijvoorbeeld de volgende commando's om de databases te weergeven en daarna `sqlcmd` af te sluiten:&#x20;

```
select name from sys.databases
go
quit
```

{% hint style="warning" %}
Veelgemaakte fout: als je direct na de installatie `sqlcmd` uitvoert in een al geopende terminal, dan is de nieuwe PATH omgevingsvariabele nog niet geladen. Open daarom altijd een nieuwe terminal (en herstart VS Code als je de terminal in VS Code gebruikt) om programma's uit te voeren na installatie.&#x20;
{% endhint %}

### De connectie string

Een voorbeeld connectiestring ziet er als volgt uit. Geef deze connectiestring als argument mee aan de `UseSqlServer` methode. De nieuwe regels zijn niet noodzakelijk.&#x20;

```
Data Source=(LocalDb)\MSSQLLocalDB;
Initial Catalog=master;
Integrated Security=True;
Connect Timeout=30;
Encrypt=False;
TrustServerCertificate=False;
ApplicationIntent=ReadWrite;
MultiSubnetFailover=False
```

{% hint style="warning" %}
Veelgemaakte fout: sommige karakters die de connectiestring typisch bevat, moeten worden ge-escaped in C# string literals.&#x20;
{% endhint %}

### De SQL Server database bekijken

Open Microsoft SQL Server Management Studio ![](<../../.gitbook/assets/image (2).png>) en vul `localhost` in bij Server name.&#x20;

![](<../../.gitbook/assets/image (5).png>)

In de _Object Explorer_ (_View_ ⇨_Object Explorer_) kan je vervolgens de tabellen zien in de databases.&#x20;

![](<../../.gitbook/assets/image (4).png>)

### De SQL Server database bekijken vanuit VS Code

Er is [een extensie](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) voor VS Code waarmee je SQL Servers kunt queryen en aanpassen vanuit VS Code.&#x20;

* Zoek op "SQL Server" in de Extensions tab van VS Code en klik op Install
* Klik op het icoontje ![](<../../.gitbook/assets/image (1).png>) in de toolbar aan de zijkant.&#x20;
* Klik op "_Add Connection_".&#x20;
* Vul `localhost` in als server name en laat de database name leeg. Kies "_Integrated_" als "_authentication method_".&#x20;

![](<../../.gitbook/assets/image (6).png>)

## Het gebruik van InMemoryDatabase

{% hint style="info" %}
Dit gedeelte ontbreekt nog.&#x20;
{% endhint %}

## Het gebruik van MySQL

{% hint style="info" %}
Dit gedeelte ontbreekt nog.&#x20;
{% endhint %}
