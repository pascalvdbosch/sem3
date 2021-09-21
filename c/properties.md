# Properties

In Java zijn attributen in klassen meestal `private`, met bijbehorende `public` getters en setters, bijvoorbeeld:

```java
class Persoon
{
    private String naam;
    public String getNaam() {
        return naam;
    }
    public void setNaam(String naam) {
        this.naam = naam;
    }
}

// en verderop...

Persoon p = new Persoon();
p.setNaam("Johan");
System.out.println(p.getNaam());
```

Als er nu een attribuut `Leeftijd` zou worden toegevoegd, met getters en setters, dan zou er veel dezelfde soort code nodig zijn. Dit wordt boilerplate code genoemd, de taal Java is verbose in dit opzicht. 

Omdat deze constructie \(private attribuut met public getters en setters\) vaak voorkomt, is er in C\# een speciale syntax voor bedacht: de **property**. Bij de declaratie en definitie van de property is er nogsteeds bijna evenveel vrijheid: de body van de getter \(in C\# **accessor**\) en setter \(in C\# **mutator**\) moeten nog een invullingen gegeven worden. Bij het gebruik van de property is het de schrijfwijze echter wel verkort en intuitiever geworden. 

```csharp
class Persoon
{
    private String naam;
    public String Naam
    {
        get
        {
            return naam;
        }
        set
        {
            naam = value;
        }
    }
}

// en verderop...

Persoon p = new Persoon();
p.Naam = "Johan";
System.out.println(p.Naam);
```

Het is belangrijk om niet te vergeten dat op regel 18 er een getter wordt aangeroepen, waardoor regel 6 de volgende uit te voeren regel code is, waarna de executie weer terugsprint naar regel 18 om het resultaat als parameter mee te geven aan `println`. Hetzelfde geldt voor de setter. Het is een goede oefening om er met de debugger eens doorheen te stappen. Sommige IDE's stappen automatisch over properties heen en moeten ingesteld worden om ook in properties te kunnen stappen \(in Visual Studio: _Options &gt; Debugging &gt; "Step over properties and operators"_\). 

Het is mogelijk om de setter weg te laten, dus in principe zou je van alle niet-void methoden zonder parameter, properties zonder setter kunnen maken. Toch 

Voorbeelden van het gebruik van properties



In veel programmeertalen bestaan properties zoals in C\#: Python, JS, PHP, etc. In sommige programmeertalen \(zoals D, en Scala een beetje\) zijn zelfs alle parameterloze methoden getters en alle void methoden met één parameter setters, die gebruikt kunnen worden alsof het data is zoals in C\#. Bij de methode aanroep van een parameterloze methode zijn dan geen haakjes nodig en het aanroepen van een methode met één parameter, kan dan als toekenning. 

#### Oefening

Waarom crasht het volgende programma? Tip: goed kijken. 

{% tabs %}
{% tab title="Vraag" %}
```csharp
class Main
{
    private static int teller;
    public static int Teller
    {
        get
        {
            return Teller;
        }
        set
        {
            Teller = value;
        }
    }
    public static void Main(String[] args)
    {
        Teller = 123;
    }
}
```
{% endtab %}

{% tab title="Antwoord" %}
```
Bij de definitie van de getter van de property Teller, wordt "Teller" gereturned. 
Op dat moment wordt er een recursieve aanroep gedaan en uiteindelijk resulteert
dat in een StackOverflowException. In regel 9 en 12 had er "teller" moeten staan. 
```
{% endtab %}
{% endtabs %}

