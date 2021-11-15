# 🧪 Testen

## Wéér testen?

In semester 2 is in het semester Agile & OO Programmeren bij de vakken OPT2 en OPT3 aandacht besteed aan het testen van software. Bij het vak [OPT2](https://blackboard.hhs.nl/webapps/blackboard/content/listContent.jsp?course\_id=\_82316\_1\&content\_id=\_3213329\_1) is het testframework [JUnit5](https://junit.org/junit5/docs/current/user-guide/) gebruikt om JAVA applicaties te unittesten en bij het vak [OPT3](https://blackboard.hhs.nl/webapps/blackboard/content/listContent.jsp?course\_id=\_82316\_1\&content\_id=\_3213330\_1) zijn de softwareontwikkelmethoden TDD en Pairewise Testing aan bod geweest en er zijn testtechnieken bestudeerd die gebruikt kunnen worden om te bepalen wat er met welke coverage getest moet worden.&#x20;

Alle stappen die in OPT2 zijn gezet, worden in [deze paragraaf](testen.md#undefined) nagedaan voor het testframework xUnit in de programmeertaal C#. De softwareontwikkelmethoden en testtechnieken uit OPT3 zullen hier niet opnieuw aan bod komen en zullen ook niet hier worden toegepast. Dit betekent absoluut niet dat ze niet toepasbaar zijn bij het testen van ASP.NET MVC Core webapplicaties.&#x20;

## Unittesten

In tegenstelling tot wat we gedaan hebben bij OPT2, wordt er voor xUnit testen meestal een apart project aangemaakt voor de testen. Eerst wordt de map `test` aangemaakt, dan wordt het project aangemaakt en tenslotte wordt er een reference van het testproject naar het te testen project gemaakt.&#x20;

Eerst schrijven we een [test](testen.md#undefined) die test of 1+1=2 en die dus niet afhankelijk is van een bestaand te testen project, daarna schrijven we een [test](testen.md#een-console-applicatie) voor een console applicatie, en daarna schrijven we een [test](testen.md#undefined) voor een webapplicatie.&#x20;

### Hello world

#### Een testproject aanmaken

De eenvoudige manier om een testproject aan te maken is om het commando `dotnet new xunit` te gebruiken. Er wordt een projectbestand `.csproj` aangemaakt en een `UnitTest1.cs` met een lege test. Van een bestaand project een testproject aanmaken is ook niet moeilijk: alleen de packages `Microsoft.NET.Test.Sdk` en `xUnit` moeten toegevoegd te worden.&#x20;

#### Een eenvoudige test schrijven

De volgende test kan worden gebruikt om te controleren dat het testproject het juist doet. Normaal zou er in de methode `VermenigvuldigTest` gebruik gemaakt worden van bijvoorbeeld een `Number` klasse in een ander project dat getest moet worden. In het [volgende hoofdstuk](testen.md#console-applicatie) wordt er inderdaad een dergelijke project reference aangemaakt.&#x20;

```csharp
using Xunit;

namespace MijnTesten
{
    public class GetallenTest
    {
        [Fact]
        public void VermenigvuldigTest()
        {
            Assert.Equal(12, 3 * 4);
        }
    }
}
```

Het attribuut `[Fact]` geeft aan dat de methode een unittest is, zoals de annotatie `@Test` dat deed in JUnit in OPT2. Voor testen die op elkaar lijken is er het attribuut `[Theory]`, zoals de `@ParameterizedTest` voor JUnit.&#x20;

```csharp
using Xunit;
namespace MijnTesten
{
    public class GetallenTest
    {
        [Theory]
        [InlineData(12, 3, 4)]
        [InlineData(1, -1, -1)]
        [InlineData(0, 34, 0)]
        [InlineData(1, int.MaxValue, int.MaxValue)]
        public void VermenigvuldigTest(int uitkomst, int a, int b)
        {
            Assert.Equal(uitkomst, a * b);
        }
    }
}
```

De meestgebruikte methoden van `Assert` zijn:&#x20;

* `Equal(expected, actual)`: om te checken dat de verwachtte (`expected`) waarde overeenkomt met de eigenlijke (`actual`) waarde.&#x20;
* `True(condition)`: om te checken dat er aan een bepaalde voorwaarde voldaan is.&#x20;
* `Throws<exception>(method)`: om te checken of een methode of lambda-expressie een bepaalde exceptie throwt.&#x20;

De meeste methoden in `Assert` (zoals `Contains`, `Empty`, en `Null`) spreken voor zich.&#x20;

### Console applicatie



#### De bestandstructuur

Het is mogelijk om het testproject naast het te testen project te zetten in dezelfde map, maar dat wordt niet aangeraden, vanwege overzichtelijkheid en omdat het onwenselijk is dat tests de te testen code beïnvloeden. Het is gebruikelijk (een wordt aangeraden op de [MSDN](https://docs.microsoft.com/en-us/dotnet/core/tutorials/testing-with-cli)) dat de mappen `src` en `test` naast elkaar staan, op dezelfde manier als we dat gewend zijn bij OPT2. In de map `src` worden projecten gezet die getest worden door testprojecten in de map `test`. In de afbeelding lijken de extra submappen `ConsoleApplicatieHHS` en `ConsoleApplicatieHHSTests` overbodig, omdat dit de enige mappen zijn in respectievelijk `src` en `test`, maar in het algemeen kunnen er bijvoorbeeld meerdere projecten zijn die getest moeten worden: misschien komt er naast het `ConsoleApplicatieHHS` project in de toekomst ook wel het `WebApplicatieHHS` te staan.&#x20;

![Bestandstructuur](<../.gitbook/assets/image (7).png>)

#### Intermezzo: de terminal

De volgende commando's zijn handig in de terminal van Linux of Windows (in PowerShell) om de bestandstructuur aan te maken. Dit is in het bijzonder belangrijk tijdens een computertoets met VS Code in de browser, als je geen toegang heb tot een bestandsverkenner zoals in Windows.&#x20;

* `cd`: om van map te veranderen (**c**hange **d**irectory). Bijvoorbeeld: `cd src`.&#x20;
* `mkdir`: om een map aan te maken (**m**a**k**e **dir**ectory). Bijvoorbeeld: `mkdir test`.&#x20;
* `ls`: om de bestanden en mappen in de huidige map te tonen (**l**i**s**t). In Windows gebruiken we soms `dir`.&#x20;

De map `..` is de map waar de huidige map in zit, dus `cd ..` gaat een map omhoog in de boom.&#x20;

####

### Webapplicatie

![Bestandstructuur](<../.gitbook/assets/image (9).png>)

