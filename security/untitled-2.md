# Architecture and Design

Stap 2 in de OWASP Software Development Lifecycle gaat om Securityarchitectuur en ontwerpcriteria m.b.t. de beveiliging

## Threat modeling

Bedreigingsmodellering is een gestructureerd proces met de volgende doelstellingen: beveiligingsvereisten identificeren, beveiligingsbedreigingen en potentiële kwetsbaarheden lokaliseren, de mate van bedreiging en kwetsbaarheid kwantificeren en prioriteit geven aan herstelmethoden. De volgende stappen worden doorlopen:

![](<../.gitbook/assets/image (8) (1).png>)

Er zijn veel methoden voor het modelleren van bedreigingen ontwikkeld en ook meerdere tools (Irius Risk, Microsoft, OWASP, OVVL). We noemen hier 3 verschillende bedreigingsmodellen STRIDE, PASTA en LINDDUN.

### STRIDE

STRIDE past een algemene reeks bekende bedreigingen, die samen de naam vormen, toe als een geheugensteun voor veiligheidsbedreigingen.

* **S**poofing: misbruik van de gebruikersidentiteit (schending van authenticatie), namelijk zich als een ander voordoen. Voorbeeld is phishing (toegang wordt verleend door de gebruiker zelf).
* **T**ampering: sabotage, schending van de Integriteit.
* **R**edupiation: weerlegbaarheid van authenticatie(\*). Het ontkennen dat men een actie heeft uitgevoerd. Integriteit en herkomst van gegevens aantonen. Non-repudiation (onweerlegbaarheid) is het bewijs van de integriteit en oorsprong van de data.
* **I**nformation Disclosure: het lekken van data, schenden van vertrouwelijkheid (privacy)
* **D**enial of Service: schending beschikbaarheid van het systeem
* **E**levation of Priviledge: misbruik van bevoegdheden, schending autorisatie.

### PASTA

PASTA (Process for Attack Simulation and Threat Analysis) is een risicogericht modelleerproces voor aanvalssimulatie en dreigingsanalyse dat in 7 stappen doelstellingen en vereisten definieert, aanvallen simuleert, analyseert en in kaart brengt op dreigingsscenario’s om zo procedures, processen en beleid voor bedreigingsbeheer te ontwikkelen.&#x20;

1. Definieer de bedrijfs- en beveiligingsdoelstellingen.&#x20;
2. Definieer de technische scope.&#x20;
3. Splits de applicatie op in essentiële elementen (b.v. gebruikers, servers, gegevensactiva) voor verdere analyse (gebruik een DFD, zie H.2.5.1).&#x20;
4. Analyseer de dreigingen.&#x20;
5. Analyseer de kwetsbaarheden.&#x20;
6. Analyseer de aanvallen (m.b.v. attack trees bijvoorbeeld).&#x20;
7. Risico- en impact-analyse en ontwikkeling van tegenmaatregelen.

### LINDDUN

LINDDUN dit model richt zich op privacyproblemen en kan worden gebruikt voor gegevensbeveiliging. De 3 stappen, modelleer het systeem, eliciteer en beheer bedreigingen (linddun, 2020), worden systematisch doorlopen en geanalyseerd vanuit het oogpunt van de 7 LINDDUN privacy-bedreigingscategorieën:

* **L**inkability: Koppelbaarheid (mogelijkheid tot het koppelen van gegevens), wanneer er een link tussen meerdere datapunten herkent kan worden.&#x20;
* **I**dentifiability: Identificeerbaarheid (mogelijkheid om een persoon te identificeren uit verschillende gegevens), wanneer de identiteit van een bepaald object (gebruiker) achterhaald kan worden.&#x20;
* **N**on-repudiation: Onweerlegbaarheid (de waarborg dat ontvangst en/of verzending van een contract of een bericht niet kan worden ontkend door de beide betrokken partijen, respectievelijk de ontvanger en de verzender), wanneer het mogelijk is om bewijs te verzamelen waardoor een subject niet kan ontkennen een actie uitgevoerd te hebben.&#x20;
* **D**etectability: Detecteerbaarheid, wanneer het mogelijk is om te ontdekken dat een item of interest bestaat, kan daaruit informatie afgeleid worden. Vb. als je weet dat een beroemdheid is opgenomen in een afkickcentrum, dan kan je daaruit 14 afleiden dat hij/zij een verslavingsprobleem heeft, zonder dat je bij de daadwerkelijke informatie kunt.&#x20;
* **D**isclosure of Information: Ontsluiten van Informatie, wanneer informatie wordt vrijgegeven aan partijen die geen toegang tot die informatie behoren te hebben.&#x20;
* **U**nawareness: Onwetendheid, verlies van controle over je gegevens (minimalisatie) onbewustheid over wat er met de informatie die je zelf verstrekt gebeurt. Het systeem moet de gebruiker helpen bij het nemen van privacy-beslissingen door meer transparantie en privacyvriendelijke beslistechnieken.&#x20;
* **N**on-compliance: Niet-naleving, het systeem voldoet niet aan de wetgeving, het aangegeven beleid of de aangegeven toestemming van de gebruiker.&#x20;

## Bedreigingen

De OWASP heeft een top 10 opgesteld van de meest voorkomende security-bedreigingen die de kwetsbaarheden van Web Applicaties aangeven. Dit is een 3 jaarlijks overzicht van meest voorkomende kwetsbaarheden in (web gebaseerde) software. Er is ook Sans 25 die (zoals de naam al doet vermoeden) uitgebreider is maar niet alleen voor web gebaseerde applicaties (CWE/SANS TOP 25 Most Dangerous Software Errors, 2021). Een applicatieontwikkelaar moet zich bewust zijn van het feit dat de OWASP top 10 slechts als een waarschuwing vooraf en een checklist achteraf gebruikt kan worden zodat je kunt zeggen: ‘we hebben in ieder geval de meest voorkomende dreigingen gedekt’, maar dat er nog veel meer dreigingen afgedekt dienen te worden.

_The OWASP Top 10 is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications (OWASP TopTten, 2017)._

De onlangs gepresenteerde top 10-2021 (OWASP TopTten, 2021) heeft de volgende wijzigingen t.o.v. die van 2017 (Bijlage 2)_. _

![(OWASP TopTten, 2021)](<../.gitbook/assets/image (7) (1).png>)

### De OWASP top 10-2021 bedreigingen, kwetsbaarheden en maatregelen

Hieronder een beschrijving van de bedreigingen uit de OWASP top 10-2021 met daarbij aangegeven wat de kwetsbaarheden (zwakke plekken) zijn waar de beveiliging zich op moet richten en wat mogelijke maatregelen zijn die een incident kunnen voorkomen.

#### A01-Broken Access Control

Beperkingen op wat geautoriseerde gebruikers mogen doen, worden vaak niet goed afgedwongen. Aanvallers kunnen dit misbruiken om ongeautoriseerd toegang te krijgen tot functionaliteit en/of gegevens, zoals toegang tot accounts van andere gebruikers, het bekijken van gevoelige bestanden, wijzigen van gegevens van andere gebruikers, wijzigen van toegangsrechten, etc.&#x20;

Acces control gaat over de vraag ‘wie heeft toegang tot wat’, welk type gebruiker heeft toegang tot welke informatie. Een falende controle op rollen en rechten is de zwakke plek.&#x20;

Access Controle begint bij het goed opzetten én controleren én updaten van de rechten van gebruikers (identity management en access management(\*)), het detecteren van inbreuken op de toegangsregels en het registreren ervan. Zorg voor functiescheiding zodat een proces of taak niet in zijn geheel door één persoon gedaan kan worden en ken alleen de voor de functie noodzakelijke rechten toe, ‘need to know’: alleen toegang indien noodzakelijk en ‘deny by default’: standaard géén rechten.&#x20;

#### A02-Cryptographic Failures

De hernieuwde focus t.o.v. de vorige 2017-versie ‘Sensitive Data Exposure’ ligt op fouten in verband met cryptografie, wat vaak leidt tot blootstelling aan gevoelige gegevens of systeemcompromissen. Veel webapplicaties beschermen gevoelige gegevens, zoals credit cards, btw-nummers, en inloggegevens, niet goed. Aanvallers kunnen deze gegevens stelen of deze slecht beschermde gegevens wijzigen om credit card fraude, identiteitsdiefstal of andere misdrijven mee te plegen, bijvoorbeeld door Man-in-the-middle-attacks of het bemachtigen van crypto keys. Dit gebeurt voornamelijk wanneer de data op transport is bijvoorbeeld bij dataoverdracht in clear-tekst (unencrypted) of data in rest bij veelal verouderde databases waarbij de data niet versleuteld opgeslagen is. Zorg altijd voor Encryptie(\*) van data tijdens transport én tijdens opslag.

#### A03-Injection

Injectie-mogelijkheden, zoals SQL injectie en XSS kunnen optreden wanneer onbetrouwbare gegevens als deel van een commando of query naar een interpreter gestuurd worden.

Bij SQL-injection plaatst men commando’s waar alleen data hoort. Deze commando’s van de aanvaller kunnen ervoor zorgen dat de interpreter onbedoelde commando’s uitvoert of zich toegang verschaft tot gegevens zonder hiervoor de juiste rechten te hebben. SQL injectie is erop gericht om informatie uit de database te stelen of de data in de database aan te passen.&#x20;

De kwetsbaarheid bij SQL-injection ligt bij de invoer van gebruikers. Dit kan invoer zijn in bijvoorbeeld webformulieren, een plaatje dat wordt geüpload, maar ook de URL in de adresbalk. Vertrouw dus nooit de input van gebruikers. Door het ‘injecteren’ van SQLcommando’s kan een hacker gegevens stelen, aanpassen, verwijderen en ontoegankelijk maken. De kwetsbaarheid is dus gebrekkige inputvalidatie. Verdedigen van het script kan bijvoorbeeld door het casten van data-typen en het gebruiken van prepared statements. Casten van datatypen wil zeggen dat je bij het aanmaken ervan bepaald wat voor een datatype het is; er kan dan geen ander datatype ingevuld worden. Prepared statements zijn SQL-queries met placeholders die later opgevuld worden met de gewenste data.

Cross-site Scripting (XSS) gebreken doen zich voor wanneer een aanvraag met onbetrouwbare gegevens wordt verstuurd naar een webserver zonder de juiste validatie of ‘escape”. XSS aanvallers hebben de mogelijkheid om in browsers van gebruikers scripts uit te voeren om zodoende bijvoorbeeld de gebruikerssessie te kapen, websites te beschadigen, of de gebruiker naar kwaadaardige websites (om) te leiden. XSS-aanvallen kunnen worden gebruikt om gebruikers om te leiden naar websites waar aanvallers gegevens van hen kunnen stelen.

Gebrek aan invoervalidatie kan incidenten veroorzaken, de applicatie mag geen enkele invoer vertrouwen en moet daarom valideren. Fuzzers (via tests beveiligingsgaten of programmeerfouten vinden) zullen alles vinden wat niet gevalideerd wordt. Zorg voor veilige foutafhandeling oftewel exception handling(\*): een fout mag nooit leiden tot een onveilige situatie.

#### A04-Insecure Design

Een nieuwe categorie voor 2021, met een focus op risico's gerelateerd aan ontwerpfouten. Beveiliging naar links verschuiven betekent het invoeren van veiligheidscontroles en werkzaamheden tijdens de ontwikkelfase. Oftewel de visualisatie van softwareontwikkeling als een links naar rechts voortgang van taken, te beginnen met ontwerp en architectuur en dan naar implementatie. Deze werkwijze vraagt om gebruik van dreigingsmodellering, veilige ontwerppatronen en -principes en referentiearchitecturen. Het doel is ervoor te zorgen dat de code vanaf het begin veilig is ontworpen, in plaats van aan het einde van het proces te controleren op beveiligingsproblemen.

#### A05-Security Misconfiguration

Het gaat hierbij om het niet of niet afdoende implementeren van beveiligingscontroles. Wanneer beveiligingsinstellingen , voor de applicatie, frameworks, applicatie-server, web server, database server, en het platform niet zijn gedefinieerd en geïmplementeerd en slechts standaardwaarden worden gehanteerd, kun je problemen verwachten.&#x20;

Configuratiefouten en verouderde software zijn hier de bedreiging en tevens de kwetsbaarheid. Als gevolg van slecht geconfigureerde XML-processors kunnen XXEaanvallen voorkomen waarbij aanvallers op de plaats van de URL een XML bestand meegegeven waarin kwaadwillige code staat (external entity) of een verwijzing naar een (kwaadwillig) bestandspad. Hiermee kan toegang verkregen worden tot bestanden en programma’s. Het toestaan van externe XML-entiteiten, door een gedateerde of slecht geconfigureerde xml-verwerker die externe entiteiten niet signaleert, is hier de kwetsbaarheid.

Beveiligingsinstellingen moeten daarom worden gedefinieerd, geïmplementeerd en onderhouden. Zorg voor gepatchte systemen (patchbeheer), regelmatige software updates, firewallbescherming en vermijd het gebruik van een “standaard account” instelling. Sommige frameworks hebben default accounts, en deze kan je beter uitzetten. Installeer alleen de softwarepakketten die je echt gebruikt, zo min mogelijk dus.

#### A06-Vulnerable and Outdated components

Componenten, zoals libraries, frameworks en andere software modules, worden bijna altijd met volledige privileges uitgevoerd. Als een kwetsbaar component wordt misbruikt kan een dergelijke aanval ernstige verlies van gegevens of een server overname vergemakkelijken.&#x20;

Gebruik van software met (bekende) kwetsbaarheden kunnen de beveiliging ondermijnen. De Exploit-DB (Exploit Database, 2021) een archief van exploits(\*) en kwetsbare software die, als het goed is, allemaal gemeld zijn bij de leverancier. Zodra een kwetsbaarheid door de ontwikkelaar is opgelost, wordt deze bekend gemaakt. Het is essentieel om je software te patchen voordat hackers misbruik kunnen maken van deze kwetsbaarheden.&#x20;

#### A07-Identification and Authentication Failures

Functies in applicaties met betrekking tot authenticatie(\*) en sessie management worden vaak niet correct toegepast, waardoor hackers wachtwoorden, sleutels of sessie tokens, gebruikersaccount-informatie en andere details kunnen onderscheppen, of andere gebreken van de implementatie benutten om de identiteit van andere gebruikers over te nemen.&#x20;

De Authenticatie(\*) wordt ‘verbroken’ als een hacker credentials heeft weten te bemachtigen waarmee hij direct toegang krijgt tot gegevens die hij niet zou mogen zien. Men doet dit door bijvoorbeeld gebruik te maken van eerdere datalekken (credential stuffing) of door Brute Force-, Dictionary-, of Rainbow table attacks. De aanvaller doet zich vervolgens voor als een legale gebruiker. Een slecht ontwerp en slechte implementatie van identiteits- en toegangscontroles is de kwetsbaarheid.&#x20;

Om te voorkomen dat een hacker een sessie kan stelen (“session hijacking”) is het goed om te weten dat session-id’s niet in URL’s horen.&#x20;

Maatregelen aan de gebruikerskant die genomen kunnen worden om ongeautoriseerde toegang tegen te gaan zijn bijvoorbeeld het instellen van multifactor authentication en een weak-password check (zodat geen wachtwoord uit de 1000 slechtste wachtwoorden gekozen wordt), stel altijd minimale eisen aan wachtwoorden (hoofdletters, speciale tekens, aantal karakters) en beperk het aantal inlogpogingen. Limiteer gebruikers sessies door gebruikers uit te loggen en sessies te invalideren na een bepaalde tijd van niet gebruik. Zorg ook dat de gebruiker geïnformeerd wordt over belangrijke gebeurtenissen met meldingen zoals: ‘Uw wachtwoord is gewijzigd’ of ‘U probeert in te loggen vanaf een nieuwe locatie’.&#x20;

#### A08-Software and Data Integrity Failures

Dit is een nieuwe categorie voor 2021, gericht op het maken van aannames met betrekking tot software-updates, kritieke gegevens en CI/CD-pijplijnen zonder de integriteit te verifiëren. CI/CD is een methode voor het frequent leveren van applicaties naar (eind)klanten waarbij vrijwel alle stappen geautomatiseerd zijn.

Continuous Integration, Continuous Delivery en Continuous Deployment (CI/CD) is vaak een lineair proces. Vandaar dat men spreekt van een CI/CD pipeline. De code van de ontwikkelaar moet eerst door alle geautomatiseerde integratietests, om vervolgens door alle delivery-tests te gaan en als laatst door de deploymenttests.&#x20;

Onveilige deserialisatie uit 2017 maakt nu deel uit van deze grotere categorie. Onveilige deserialisatie (het creëren van een object uit een opeenvolging van bytes) kan een aanvaller in staat stellen om code in de applicatie op afstand uit te voeren, geserialiseerde (geschreven naar schijf) objecten te wijzigen of verwijderen, injectie aanvallen uit te voeren en rechten op te heffen.

Bij serialiseren wordt een object omgezet naar een universeel berichtformaat. Het terugvertalen is deserialiseren. Wanneer het ‘terugvertalen’ zonder controle plaatsvindt kunnen operating system commando’s worden ingevoerd; daar zit dus de kwetsbaarheid.

Een maatregel die genomen kan worden om deze bedreiging tegen te gaan is het implementeren van integriteitscontroles.

#### A09-Security Logging and Monitoring Failures

De identiteitsstatus van de user wordt gemonitord. Je houdt hier bij wat er met het systeem gebeurt nadat de gebruiker een bepaalde Autorisatie(\*) heeft gekregen. Onvoldoende logging en inefficiënte integratie met systemen voor beveiligingsincidenten stellen aanvallers in staat om regelmatig over te stappen naar andere systemen en bedreigingen en dit gedurende weken of maanden lang in stand te houden voordat het wordt ontdekt.&#x20;

Continu loggen en monitoren is nodig om problemen snel te ontdekken en op te lossen en de kwetsbaarheid ligt dus in het onvoldoende uitvoeren hiervan.&#x20;

#### A10-Server-Side Request Forgery

Bij een SSRF-aanval (Server-Side Request Forgery) kan de aanvaller functionaliteit op de server misbruiken om interne bronnen te lezen of bij te werken. De aanvaller kan een URL leveren of wijzigen waarnaar de code die op de server draait, gegevens leest of verzendt, en door de URL's zorgvuldig te selecteren, kan de aanvaller de serverconfiguratie lezen, zoals AWS-metagegevens, verbinding maken met interne services zoals http ingeschakelde databases of het uitvoeren van postverzoeken naar interne services die niet bedoeld zijn om openbaar te worden gemaakt.

Deze nieuwe categorie voor 2021 komt uit de OWASP community survey, waarin community members wijzen op het belang van deze dreiging, ook al wordt dit op dit moment niet geïllustreerd in de gegevens (lage melding van incidenten).

## Beveiligingsprincipes

Voor het inbouwen van beveiliging in het ontwerp van de applicatie (SbD) bestaat een aantal richtlijnen, ook wel ontwerpcriteria of beveiligingsprincipes genoemd (Houten, Spruit, & Wolters, 2019).

Onderstaande ontwerpprincipes zijn door Jerome Saltzer en Michael Schroeder opgesomd in hun artikel uit 1975 ‘De bescherming van informatie in computersystemen’ en opgenomen in Informatiebeveiliging onder Controle H.9.2 (Houten, Spruit, & Wolters, 2019).

1. **Isolatie (Economy of mechanism)** Het principe van economy of mechanism stelt dat beveiligingsmechanismen, de hardware en software die relevant zijn voor de beveiliging, zo eenvoudig mogelijk moeten zijn. Als een ontwerp en implementatie eenvoudig zijn, zijn er minder mogelijkheden voor fouten. Het controle- en testproces is minder complex, omdat er minder componenten en cases hoeven te worden getest. Keep security simple, complexe architecturen vergroten de kans op fouten.&#x20;
2. **Veilige defaults (Fail-safe Defaults)** Het systeem mag alleen toegang verlenen na expliciete permissie; alles wat niet expliciet is toegestaan, is verboden.&#x20;
3. **Volledigheid (Complete Mediation)** Elke vorm van toegang mag pas plaatsvinden na Autorisatie(\*) door het systeem. Gebruikers en processen dienen zich daartoe altijd eerst te legitimeren.&#x20;
4. **Open ontwerp** Een goede beveiligingsarchitectuur is niet gebaseerd op het geheimhouden van de gebruikte interne mechanismen (security by obscurity = zwakke plekken in de beveiliging ‘verstoppen’), maar gaat juist uit van een open ontwerp. Bij een gesloten ontwerp bestaat het risico dat de werking van interne mechanismen op den duur toch aan het licht komt, bijvoorbeeld door het toepassen van reverse engineering en het uitlekken van ontwerpdocumenten. Het voordeel van een open ontwerp is dat het intensiever kan worden getest en eenvoudiger kan worden verbeterd.&#x20;
5. **Functiescheiding** Waar mogelijk moeten functies in het systeem worden gesplitst, waarbij de onderscheiden deelfuncties aan verschillende functionarissen moeten worden toegewezen (fraude voorkomen). Een bijzondere vorm van functiescheiding is het ‘4-ogen’-principe, waarbij voor gevoelige handelingen de Autorisatie() van meerdere functionarissen nodig is.&#x20;
6. **Beperking (Least Privilege)** Het systeem moet zo opgezet zijn, dat bepaalde gebruikers en processen niet meer functies mogen uitvoeren of hulpbronnen mogen gebruiken dan strikt noodzakelijk is. Dit principe staat ook bekend onder de namen least privilege, need to know en need to use.&#x20;
7. **Compartimenten** Het systeem moet bestaan uit verschillende compartimenten, segmenten of modules, zodat een mogelijk veiligheidsprobleem tot het specifieke compartiment, etc. beperkt blijft.&#x20;
8. **Ergonomie** Het systeem moet zo ontworpen zijn dat de kans op menselijke fouten zo klein mogelijk is. Fouten van eindgebruikers kunnen worden beperkt door het ontwerpen van een ‘intuïtief’ gebruikersinterface, het integreren van beveiligingsmaatregelen in processtappen en het uitvoeren van automatische controles tijdens het gebruik van het systeem. Fouten in het beheer kunnen alleen worden beperkt als de hulpmiddelen voor het beheer en de beheerprocessen zelf voldoende transparant zijn, zodat alle betrokkenen inzicht hebben in wat er van ze wordt verwacht.&#x20;
9. **Redundantie** De beveiligingsarchitectuur moet bestaan uit een combinatie van maatregelen, zodat de beveiliging niet afhankelijk is van één enkele maatregel (The principle of Defence in depth, OWASP): verschillende type security controles (lagen) aanbrengen. Meerdere beveiligingscontroles die risico's op verschillende manieren benaderen. Bijvoorbeeld in plaats van een gebruiker te laten inloggen met alleen een gebruikersnaam en wachtwoord ook een IP-controle, een Captcha-systeem, logboekregistratie van hun inlogpogingen, brute force-detectie gebruiken.&#x20;
10. **Diversiteit** De beveiligingsarchitectuur moet bestaan uit meerdere maatregelen die wezenlijk van elkaar verschillen, zodat het doorbreken van één beveiligingsmaatregel niet automatisch leidt tot de val van het gehele systeem.&#x20;

De OWASP geeft aanvullend hierop nog de volgende beveiligings-ontwerpprincipes (Security by Design Principles according to OWASP, sd) waaraan programmeurs zich zouden moeten houden om het risico op een succesvolle cyberaanval te verminderen.

* Minimise attack surface area: beperk de (online)functies waartoe gebruikers toegang hebben, om mogelijke kwetsbaarheden te verminderen.
* Establish secure defaults: de defaults zijn zo veilig als mogelijk, instellen van veilige standaardinstellingen (bijvoorbeeld hoe vaak wachtwoorden moeten worden bijgewerkt, hoe complex wachtwoorden moeten zijn)
* Fail securely: zorg dat eventuele fouten niet tot onveilige situaties leiden (o.a. goede detectie / logging).
* Don’t trust services: ga er in principe van uit dat third-party (externe) services niet te vertrouwen zijn, de applicatie moet altijd de geldigheid controleren van de gegevens die services van derden verzenden en die services geen machtigingen op hoog niveau binnen de app geven.
* Fix security issues correctly: Als er een beveiligingsprobleem is vastgesteld in een toepassing, moet de hoofdoorzaak achterhaald, gerepareerd en getest worden. Als gebruik gemaakt is van design patterns is de kans groot dat de fout aanwezig is in meerdere systemen, alle betrokken systemen moeten derhalve getest worden.

Privacyontwerpstrategieën

