---
description: Dit hoofdstuk bevat voorbeelden van niet-auto-implemented properties.
---

# Voorbeelden

## Event-listeners

De getters en setters kunnen ook gebruikt worden om te loggen

```csharp
public class Auto
{
    private int benzine = 100;
    public int Benzine
    {
        get
        {
            return benzine;
        }
        set
        {
            benzine = value;
            if (value < 10)
                System.Console.WriteLine("Waarschuwing! Benzine bijna op!");
        }
    }
}
```

```csharp
public class Geldzaken
{
    private double? bitcoinKoers = null;
    public double BitcoinKoers
    {
        get
        {
            if (bitcoinKoers == null)
                BitcoinKoers = HTTPVerzoekNaarBank("bitcoinkoers")
            return bitcoinKoers;
        }
        set
        {
            bitcoinKoers = value;
        }
    }
}
```

```csharp
public class Persoon
{
    private IEnumerable<Persoon> kinderen;
    public IEnumerable<Persoon> Kinderen
    {
        get
        {
            return kinderen.AsReadOnly();
        }
        set
        {
            kinderen = kinderen.Clone();
        }
    }
    public void Adopteer(Persoon kind)
    {
        if (kinderen.Count() >= 5)
            throw new Exception("U heeft teveel kinderen");
        kinderen.Add(kind);
    }
}
```

```csharp
public class Student
{
    private int Id;
    public IEnumerable<Persoon> Resultaten
    {
        get
        {
            Database.MaakConnectie();
            return Database.Query("SELECT * FROM Resultaten WHERE student=" + Id);
        }
        set
        {
            Database.MaakConnectie();
            foreach (Resultaat res in value)
                Database.Query(res.InsertIntoQuery());
        }
    }
}
```

```csharp
public class Persoon
{
    private Persoon partner;
    public Persoon Partner
    {
        get
        {
            return partner;
        }
        set
        {
            partner = value;
            value.partner = this; 
        }
    }
}
```

