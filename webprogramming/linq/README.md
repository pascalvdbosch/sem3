# LINQ

Webapplicaties staan meestal verbonden met databases om gegevens te kunnen opslaan en ophalen (zie [Client-server](../untitled.md)). Ongeacht het database paradigma (relationele databases, document databases, etc.), moeten de collecties data in de databases uiteindelijk in C# gerepresenteerd kunnen worden in iets wat er van de buitenkant uit ziet als een datastructuur (zoals een `List`). De object relational mapper (zie [ORM](../orm/)) zorgt hiervoor. LINQ rust de ontwikkelaar uit met een aantal methoden om op een SQL-achtige manier data in abstracte datastructuren te query'en. 

Het volgende voorbeeldje laat zien hoe zo'n query er uit kan zien, als gebruik gemaakt wordt van de LINQ methoden `Where` en `Count`. Het volgende voorbeeldje laat zien dat we met LINQ vragen _wat_ we willen, en minder _hoe_ we dat willen (meer daarover [later](./#functioneel-programmeren)). 

```csharp
// zonder LINQ
int aantalGoedeStudenten = 0
foreach (Student s in studenten)
    if (s.StudiePunten > 45)
        aantalGoedeStudenten++;

// met LINQ
int aantalGoedeStudenten = studenten.Where((s) => s.StudiePunten > 45).Count();
```

Meestal is het mogelijk om de data te representeren als objecten in een lijst, maar het inladen van alle data in de database in een `List` in C# zou het intern geheugen snel doen vollopen. Er zal daarom onvermijdelijk gebruik gemaakt moeten worden van een datastructuur dat zich slechts gedraagt als `List`, d.w.z. `IList` implementeert. Dit is echter nog te specifiek. In databases is de volgorde van verschillende instanties van hetzelfde entiteit namelijk meestal niet bewaard. Bijvoorbeeld in een relationele database worden de rijen in een tabel niet in een specifieke volgorde bewaard, en is het bijvoorbeeld [niet gegaradeerd](https://stackoverflow.com/questions/8746519/sql-what-is-the-default-order-by-of-queries) dat twee keer de SQL query `SELECT *` , de resultaten in dezelfde volgorde terug geeft. De juiste interface om in C# de data in databases te representeren is dus niet `IList`, maar eerder een `IEnumerable`. `IEnumerable`'s zijn de typen waarvoor een `foreach` werkt, d.w.z., typen waarvoor er `Enumerators` bestaan. Hoewel er inderdaad LINQ methoden gedefinieerd zijn voor `IEnumerable`'s, zijn het de `IQueryable`'s waarop LINQ methoden gedefinieerd kunnen zijn die niet alle data uit de database hoeven in te laden. Meer daarover in [later](./#extensie-methoden). 

Omdat LINQ methoden gedefinieerd zijn voor een abstracte IEnumerable, kan het gebruikt worden voor een groot aantal verschillende datastores, zonder dat daarvoor de methode-aanroepen aangepast hoeven te worden. 

LINQ methoden kunnen worden gebruikt voor `IEnumerable`'s als de type's in de namespace `System.Linq` worden geimporteerd d.m.v.

```csharp
using System.Linq;
```

De LINQ [extensie methoden](../c/extensie-methoden.md) zullen door deze regel beschikbaar worden in het type `IEnumerable`. We zullen later zien dat er ook bibliotheken zijn die LINQ methoden definieren op types concreter dan de `IEnumerable` (namelijk bijvoorbeeld de `IQueryable`), en dat de implementatie daarvan voor hogere performance en betere schaalbaarheid kan zorgen. 

De belangrijkste LINQ methoden zijn samengevat in de onderstaande tabel. 

| Methoden                                                                        | Return-type       | Beschrijving                                                |
| ------------------------------------------------------------------------------- | ----------------- | ----------------------------------------------------------- |
| `Where`, `Select`, `OrderBy`, `Skip`, `Take`, `Join`, `Aggregate`, `Distinct`   | `IEnumerable<T>`  | Allerlei                                                    |
| `ToList`                                                                        | `List<T>`         | Stop de lazy evaluatie en maak daadwerkelijk een lijst.     |
| `First`, `Last`, `Single`, `FirstOrDefault`, `LastOrDefault`, `SingleOrDefault` | `T`               | Pak een specifiek element, geef anders een error of default |
| `Any`, `All`                                                                    | `bool`            | Voldoet een voorwaarde bij tenminste een of alle?           |
| `Max`, `Min`, `Sum`, `Average`                                                  | `int` of `double` | Rekenen met getallen                                        |
| `ForEach`                                                                       | `void`            | Voer een actie uit met elk element                          |

## Query syntax

In C# 

### Yield

`yield` is een keyword dat in C# gebruikt kan worden om lazy `IEnumerable`s te maken. Een methode returneert dan een `IEnumerable`, waarvan de elementen pas bepaald worden als er een `IEnumerator` nodig is (bijvoorbeeld bij een `foreach`). Om te zien hoe dit werkt, bekijken we eerst de volgende voorbeeld methode `EvenGetallen`: 

```csharp
public static IEnumerable<int> EvenGetallen(int tot)
{
    List<int> lijst = new List<int>();
    for (int i = 0; i < tot; i += 2)
        lijst.Add(i);
    return lijst;
}
```

Als we echter van tevoren niet weten hoeveel even getallen we nodig hebben



```csharp
using System.Collections.Generic;

public class MyClass
{
    public static IEnumerable<int> EvenGetallen(int tot)
    {
        for (int i = 0; i < tot; i += 2)
            yield i;
    }
    
    public static void Main(string[] args)
    {
        foreach (int i in EvenGetallen(100)) {
            int j = i * i;
            Console.WriteLine(j);
        }
    }
}
```

#### ⭐ Hoe werkt yield van binnen?

Dat `yield` meer is dan alleen syntactic sugar, is te zien aan de compilergegenereerde code. Om erachter te komen wat de compiler allemaal stiekem doet op de achtergrond, wordt code soms gedecompileerd na het compileren. **Decompileren** is het terugvertalen van bytecode naar broncode, in dit geval C#. Niet de hele oorspronkelijke broncode is altijd terug halen, omdat bijvoorbeeld commentaar en variabelenamen geen effect hebben runtime, en dus niet nodig zijn. Ook is het vaak helemaal niet gewenst dat hackers eenvoudig de oorspronkelijke broncode kunnen terughalen. Om kleine performance winsten te halen is het soms goed om de gegeneerde bytecode te bestuderen. Offline decompilers als ILSpy, dotPeek of JustDecompile kunnen leerzaam zijn, maar online [sharplab.io](https://sharplab.io) is misschien het makkelijkst. 

Klik op de tab "C# gedecompileerd" hieronder, om te zien wat voor `MyMethodHelper` klasse aangemaakt (zie de annotatie `[CompilerGenerated]`) wordt door C#. Het valt op dat `mijnvariabele`, een lokale variabele in de methode MyMethodHelper, nu een attribuut is geworden. Op deze manier wordt de waarde dus bewaard terwijl we 'uit de methode' `MyMethod` springen. Ook leerzaam is het om te zien dat de toestand `state` een getal is dat aangeeft op welke plek we zijn gebleven in de methode `MyMethod`. 

{% tabs %}
{% tab title="C#" %}
```csharp
using System.Collections.Generic;

public class MyClass
{
    public static IEnumerable<int> MyMethod()
    {
        yield return 123;
        int mijnvariabele = 100;
        mijnvariabele++;
        yield return mijnvariabele;
        yield return 1000 + mijnvariabele;
    }
}
```
{% endtab %}

{% tab title="C# gedecompileerd" %}
```csharp
// using ...

public class MyClass
{
    [CompilerGenerated]
    private sealed class MyMethodHelper : IEnumerable<int>, IEnumerator<int>
    {
        private int state;
        private int current;
        private int mijnvariabele;

        int IEnumerator<int>.Current
        {
            [DebuggerHidden]
            get
            {
                return current;
            }
        }

        [DebuggerHidden]
        public MyMethodHelper(int state)
        {
            this.state = state;
        }

        private bool MoveNext()
        {
            switch (state)
            {
                default:
                    return false;
                case 0:
                    state = -1;
                    current = 123;
                    state = 1;
                    return true;
                case 1:
                    state = -1;
                    mijnvariabele = 100;
                    mijnvariabele++;
                    current = mijnvariabele;
                    state = 2;
                    return true;
                case 2:
                    state = -1;
                    current = 1000 + mijnvariabele;
                    state = 3;
                    return true;
                case 3:
                    state = -1;
                    return false;
            }
        }

        [DebuggerHidden]
        void IEnumerator.Reset()
        {
            throw new NotSupportedException();
        }

        [DebuggerHidden]
        IEnumerator<int> IEnumerable<int>.GetEnumerator()
        {
            if (state == -2)
            {
                state = 0;
                return this;
            }
            return new MyMethodHelper(0);
        }
    }

    [IteratorStateMachine(typeof(MyMethodHelper))]
    public static IEnumerable<int> MyMethod()
    {
        return new MyMethodHelper(-2);
    }
}
```
{% endtab %}
{% endtabs %}



### Lazy evaluation

#### ⭐ Hoe werkt lazy evaluation van binnen?

Hoe is het mogelijk dat bij het uitvoeren van de volgende query, de lambda expressie in de `Select` mogelijk niet op alle elementen uit de lijst `studenten` uitgevoerd hoeft te worden? Hoe is het mogelijk dat nadat `Select` is aangeroepen, er nog niets gedaan is met de lijst `studenten`? 

```csharp
bool erIsEenBuitenlandseStudent = studenten
    .Select((student) => student.Adres)
    .Where((adres) => adres.Land != "Nederland")
    .Any()
```

Wat volgt is een mogelijke implementatie, dus dit is niet de enige manier om lazy evaluation te implementeren. Het idee is dat de lazy evaluerende methoden objecten returnen die de opdracht beschrijven, zonder deze uit te voeren. 

{% tabs %}
{% tab title="LazyWhere" %}
```csharp
class LazyWhere : Iterable
{
    public Func<...> Functie { get; set; }
    public IEnumerable Data { get; set; }
}
```
{% endtab %}

{% tab title="Where" %}
```csharp
static class LINQExtensieMethoden
{
    public IEnumerable<...> Select(Func<...> Functie)
    {
        
    }
}
```
{% endtab %}

{% tab title="Any" %}
```
```
{% endtab %}
{% endtabs %}

## Extensie methoden

LINQ methoden in `System.Linq` zijn gedefinieerd als extensie methoden op de interface `IEnumerable`. Deze extensie methoden maken gebruik van de (enige) methode gedeclareerd in `IEnumerable`: `IEnumerator GetEnumerator()`. Voor veel databronnen zijn er echter efficientere manieren om data te zoeken. Bijvoorbeeld voor relationele databases zou het veel trager zijn om in C# alle rijen in een tabel door te lopen, dan om een SQL query uit te voeren op de database. De extensiemethoden gedefinieerd voor `IQueryable`'s (een specialisatie van `IEnumerable`), genereren wel echte queries voor data stores. Daarvoor moet wel de LINQ providers van de bijbehorende data store voor worden ingeladen. In [het hoofdstuk hierna](../orm/) wordt de ORM EF Core behandeld, die een LINQ provider bevat voor relationele databases. Dat wil zeggen dat de LINQ methoden worden omgezet in echte SQL queries. Hier zit wat duistere, erg ingewikkelde, magie achter, want hoe wordt de C# code `.Where((student) => student.Name.StartsWith("J"))` vertaald naar een SQL query? Bestaat de methode `StartsWith` ook in SQL? Met behulp van [reflection](https://en.wikipedia.org/wiki/Reflective_programming) worden er door de LINQ methoden [expression trees](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/expression-trees/) gebouwd, die door de LINQ provider worden omgezet in query taal. 

:warning: Waarschuwing: extensie methoden gedefinieerd op de afgeleide `IQueryable`, overriden niet de extensie methoden gedefinieerd op de base `IEnumerable`. Het resultaat `John` in de onderstaande code is hetzelfde, maar de performance is een stuk hoger bij de eerste `Where`. 

```csharp
// laat data een IQueryable zijn, dan is
Student John = data.Where((x)=>x.Id==6).Single()

// iets heel anders dan
IEnumerable<Student> data2 = data;
Student John = data2.Where((x)=>x.Id==6).Single()
```

Deze waarschuwing geldt ook voor PLINQ. In de onderstaande code is `source` van het type `IEnumerable`, dus ondanks regel 3, zijn de LINQ methoden in regel 4 uit `IEnumerable`, en niet uit `ParallelQuery`, zoals in regel 8. 

{% tabs %}
{% tab title="Kort voorbeeld" %}
```csharp
// laat data een IEnumerable zijn, dan is
var source = data;
source = source.AsParallel();
(from num in source where Moeilijk(num) select num).Count();

// veel trager dan
var source2 = data.AsParallel();
(from num in source2 where Moeilijk(num) select num).Count();
```
{% endtab %}

{% tab title="Volledig voorbeeld" %}
```csharp
using System.Linq;
using System.Diagnostics;

class Voorbeeld
{
    public static bool Moeilijk(int num)
    {
        for (int i = 0; i < 30000000; i++)
            i++;
        return true;
    }
    public static void Main(string[] s)
    {
        Stopwatch sw = new Stopwatch();

        sw.Start();
        var source = Enumerable.Range(1, 100);
        source = source.AsParallel();
        (from num in source where Moeilijk(num) select num).Count();
        sw.Stop();
        System.Console.WriteLine("Elapsed={0}", sw.Elapsed);
        sw.Reset();

        sw.Start();
        var source2 = Enumerable.Range(1, 100).AsParallel();
        (from num in source2 where Moeilijk(num) select num).Count();
        sw.Stop();
        System.Console.WriteLine("Elapsed={0}", sw.Elapsed);
        sw.Reset();
    }
}
```
{% endtab %}
{% endtabs %}

### SQL queries onderscheppen



## Vergelijking met Java

In Java zijn er sinds versie 8 Streams

## PLINQ

Parallel LINQ breidt

## Functioneel programmeren

