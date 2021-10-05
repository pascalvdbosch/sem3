# Een uitgebreid voorbeeld

niet geschikt voor recursieve dingen

```csharp
class Sport
{
    public string Naam { get; set; }
    public int AantalFans { get; set; }
    public bool TeamSport { get; set; }
    public bool BalSport { get; set; }
    public Sport(string Naam, int AantalFans, bool TeamSport, bool BalSport)
    {
        Naam = naam;
        AantalFans = aantalFans;
        TeamSport = teamSport;
        BalSport = balSport;
    }
    public static List<Sport> Sporten { get; set; } = new List<Sport>() {
        new Sport("Voetbal", 3500000000, true, true)
        new Sport("Cricket", 2500000000, true, true)
        new Sport("Hockey", 2000000000, true, true)
        new Sport("Tennis", 1000000000, false, true)
        new Sport("Volleybal", 900000000, true, true)
        new Sport("Tafeltennis", 850000000, false, true)
        new Sport("Honkbal", 500000000, true, true)
        new Sport("Golf", 450000000, false, true)
        new Sport("Basketbal", 400000000, true, true)
        new Sport("American Football", 400000000, true, true)
    }
}

```

### Vraag 1



