# Entity Framework Core

De volgende stappen zijn nodig voor een 'hello world' voor EF Core applicaties:

* Installeer de `dotnet ef` commando's: `dotnet tool install --global dotnet-ef`. Vanwege de `--global` hoeft dit maar eenmalig gedaan te worden, voor alle toekomstige projecten. 
* Voeg de benodigde assembly references toe. 
  * `dotnet add package Microsoft.EntityFrameworkCore.Design`
  * `dotnet add package Microsoft.EntityFrameworkCore.Tools`
  * `dotnet add package Microsoft.EntityFrameworkCore.Sqlite`
* Maak een C\# klasse aan, bijvoorbeeld: 

```csharp
public class Student {
    public int Id { get; set; }
    public string Naam { get; set; }
}
```

* Maak de `DbContext` aan: 

```csharp
public class MyContext : DbContext {
   protected override void OnConfiguring(DbContextOptionsBuilder b) =>
       b.UseSqlite("Data Source=database.db");
   public DbSet<Student> Student { get; set; }
}
```

* Voeg een migratie toe: `dotnet ef migrations add MijnEersteMigration`
* Voer de migratie uit: `dotnet ef database update`
* Bekijk of de database juist is aangemaakt. 



