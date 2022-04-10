# 💾 ORM

Een **Object Relational Mapper** (ORM) maakt de data in een relationele database toegankelijk in een object-georienteerde programmeertaal op een manier die zo goed mogelijk aansluit bij object-orientatie. Zonder ORM kan object-georienteerde code slecht onderhoudbaar worden als er gebruik gemaakt wordt van relationele databases. Met een ORM zijn echter niet alle problemen verholpen: de **object-relational impedance mismatch** voorkomt dat relationele databases zonder veel problemen in object-georienteerde programma's gebruikt kunnen worden.&#x20;

Een voor de hand liggende vraag is waarom ondanks zulke problemen relationele databases toch zoveel gebruikt worden bij object georienteerde programmatuur. Een reden is dat relationele databases al&#x20;

Vanwege SQL, declearatief

Waarom SQL uberhaupt? Industry standaard voor persistency, makkelijk voor niet programmeurs

## Geen ORM

De onderstaande code laat zien hoe er zonder ORM studenten uit een tabel in een relationele database op `localhost` opgevraagd kunnen worden en daarna worden uitgeprint.&#x20;

{% tabs %}
{% tab title="C#" %}
```csharp
// using System.Data.Sqlclient
string c = "Server=localhost;Database=STUDENTEN;User Id=root;Password=wachtwoord;";
string sql = "SELECT id, voornaam, achternaam, leeftijd FROM Studenten";

SqlConnection con = new SqlConnection(c);
SqlCommand cmd = new SqlCommand(sql, con);
connection.Open();
SqlDataReader reader = cmd.ExecuteReader();

while (reader.Read())
{
    Console.Write("ID: " + Convert.ToInt32(reader["id"]));
    Console.Write(", Leeftijd: " + Convert.ToInt32(reader["leeftijd"]));
    Console.Write(", Voornaam: " + reader["voornaam"].ToString());
    Console.WriteLine(", Achternaam: " + reader["achternaam"].ToString());
}
reader.Close();
```
{% endtab %}

{% tab title="Java" %}
```java
// import java.sql.*;
String c = "jdbc:mysql://localhost/STUDENTEN";
String sql = "SELECT id, voornaam, achternaam, leeftijd FROM Studenten";

Connection con = DriverManager.getConnection(c, "root", "wachtwoord");
Statement stmt = con.createStatement();
ResultSet rs = stmt.executeQuery(sql);

while (rs.next()) {
   System.out.print("ID: " + rs.getInt("id"));
   System.out.print(", Leeftijd: " + rs.getInt("Leeftijd"));
   System.out.print(", Voornaam: " + rs.getString("Voornaam"));
   System.out.println(", Achternaam: " + rs.getString("Achternaam"));
}
```
{% endtab %}
{% endtabs %}

Aan deze code lijkt misschien niet veel mis, maar dit is geen makkelijk uitbreidbare en goed onderhoudbare code.&#x20;

* Voor de programmeur moet bij het opvragen van data van de `Studenten` tabel, elke keer de waarden in alle kollomen expliciet worden geconverteerd naar het juiste type. Als er op meerdere plekken in het programma wordt ge-query'd uit deze tabel, dan geeft dat duplicate code.&#x20;
* Bij het invoegen van data op deze manier, moet er een `INSERT INTO` SQL statement worden geschreven, waar de programmeur telkens opnieuw niet moet vergeten om een **parameterized query** te gebruiken, om de invoer data te escapen, om SQL injecties te voorkomen.&#x20;

{% tabs %}
{% tab title="C#" %}
```csharp
string sql = "INSERT INTO Studenten(id,leeftijd,voornaam,achternaam)" + 
             "VALUES(@param1,@param2,@param3,@param4)";
SqlCommand cmd = new SqlCommand(sql, c);
cmd.Parameters.Add("@param1", SqlDbType.Int).Value = id;  
cmd.Parameters.Add("@param2", SqlDbType.Int).Value = leeftijd;
cmd.Parameters.Add("@param2", SqlDbType.VarChar, 50).Value = voornaam;
cmd.Parameters.Add("@param3", SqlDbType.VarChar, 50).Value = achternaam;
cmd.CommandType = CommandType.Text;
cmd.ExecuteNonQuery(); 
```
{% endtab %}

{% tab title="Java" %}
```java
string sql = "INSERT INTO Studenten(id,leeftijd,voornaam,achternaam)" + 
             "VALUES(?,?,?,?)";
PreparedStatement stmt = con.prepareStatement(sql);  
stmt.setInt(1,id);
stmt.setInt(2,id);
stmt.setString(3,voornaam);
stmt.setString(4,achternaam);
stmt.executeUpdate();  
```
{% endtab %}
{% endtabs %}

* Er is niet goed gebruik gemaakt van object georienteerd programmeren: de tabel `Studenten` bevat entiteiten die ook goed gerepresenteerd zouden kunnen worden in een klasse `Student`. Maar let op: het is niet altijd zo dat tabellen 1-op-1 corresponderen met klassen in C#, denk bijvoorbeeld eens aan de tussentabel `StudentLeentBoek`, nodig voor een veel op veel relatie tussen `Student` en `Boek`.&#x20;

Deze drie problemen worden allen in ORM's verholpen.&#x20;

ORM conceptueel



```
class Student {                                   CREATE TABLE Student (
    public int Leeftijd { get; set; }       ORM       Leeftijd int,
    public String Voornaam { get; set; }    --->      Voornaam varchar(50),
    public String Achternaam { get; set; }            Achternaam varchar(100)
}                                                 );
```

De ID

In relationele databases in de eerste normaalvorm horen er primaire sleutels te zijn die de rijen uniek identificeren, maar in een objectgeorienteerde programmeertaal is zo'n unieke sleutel niet altijd even eenduidig te definieren. Soms wordt de reference (het geheugen adresje) van objecten gebruikt om gelijkheid te definieren (`==` bij `class`), soms krijgt de methode `Equals` een alternatieve implementatie.&#x20;

Het is niet ongewoon om in klassen die door de ORM uit/in de database worden gehaald/opgeslagen, een extra attribuut of property `int Id` te geven, als in de database zo'n kolom wordt gebruikt als primaire sleutel. Dit betekent wel dat in het programma twee objecten gelijk zijn als en slechts als zij dezelfde `Id` hebben.&#x20;

Normaliseren en redudantie

Door normaliseren is er zelden redudantie in databases, maar bij het objectgeorienteerd programmeren is er niet een overeenkomstige bezigheid of streven.&#x20;

* Het attribuut `Naam` hieronder, hoeft niet in de database te worden bewaard.&#x20;

```csharp
public String Naam {
    get {
        return Voornaam + " " + Achternaam: 
    }
    set {
        string[] woorden = value.Split(" ");
        Voornaam = woorden[0];
        Achternaam = woorden.Aggregate((a, b) => a + " " + b);
    }
}
```

* De volgende code is een voorbeeld van een bidirectional relatie. In UML zouden er pijlen aan twee kanten van de associatie staan om de navigabiliteit aan te geven.&#x20;

```csharp
class Student
{
    // ...
    public List<Resultaat> ToetsResultaten { get; set; }
}

class Resultaat
{
    public int Cijfer { get; set; }
    public Toets Toets { get; set; }
    public Student Student { get; set; }
}
```

Omdat er in objectgeorienteerde programmeertalen geen `Join` operatie bestaat die alle objecten van het type `Student` naast een object van type `Resultaat` kan leggen om de bijbehorende student bij een gegeven resultaat te vinden, wordt `Student Student` in Resultaat bewaard.&#x20;

Hoe werkt overerving

```
class Persoon {
    public int Leeftijd { get; set; }
    public String Voornaam { get; set; }
    public String Achternaam { get; set; }
}

class Student : Persoon {
    public int BehaaldePunten { get; set; }
}

class Docent : Persoon {
    public String Expertise { get; set; }
}
```

Verschillende datatypes

Many to many

performance

Bomen zijn niet relationeel: graph database

Redudantie vs. normaliseren

Validatie, waar? Consistentie, referential integrity

Triggers?

Fluent API

Annotaties in de code

Java voorbeeld

Automatisch

Database first vs code first

IQueriable

Entity Framework Core

Nuget package manager

Migraties

Kijk uit met ORM's, ze zijn niet perfect:&#x20;

[https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/](https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/)

Ook laten zien hoe parameters moeten worden meegegeven met&#x20;

Andere database paradigma's

Object-georienteerde databases

Document databases

Document Relational Mapper
