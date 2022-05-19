**Java 2 Enterprise Edition** _(J2EE)_ is een ontwikkelplatform van het [softwarebedrijf](https://nl.wikipedia.org/wiki/Software "Software") [Sun Microsystems](https://nl.wikipedia.org/wiki/Sun_Microsystems "Sun Microsystems"). J2EE biedt een methode voor het ontwerpen, ontwikkelen, samenstellen en gebruiken van bedrijfsapplicaties. Het platform biedt een [multitier](https://nl.wikipedia.org/wiki/Multitierarchitectuur "Multitierarchitectuur"), gedistribueerd model, herbruikbare componenten, een beveiligingsmodel, flexibel transactiebeheer en ondersteuning voor [webservices](https://nl.wikipedia.org/wiki/Webservice "Webservice") door middel van een geïntegreerde data-uitwisseling gebaseerd op [XML](https://nl.wikipedia.org/wiki/XML "XML").

Bij het gebruik van [Javaprogramma](https://nl.wikipedia.org/wiki/Programmeertaal_Java "Programmeertaal Java")'s op [servers](https://nl.wikipedia.org/wiki/Server "Server"), wordt vaak J2EE gebruikt. J2EE is een uitbreiding van de [J2SDK](https://nl.wikipedia.org/wiki/J2SDK "J2SDK"), Java 2-standaardeditie, met bibliotheken die een groot aantal klassen bevatten voor het programmeren van serverapplicaties, het communiceren met [databases](https://nl.wikipedia.org/wiki/Database "Database") en het gebruik van allerlei generieke diensten.

Het J2EE-platform is een [platformonafhankelijke](https://nl.wikipedia.org/wiki/Multiplatform "Multiplatform"), volledig geïntegreerde oplossing die een open markt creëert waarin elke leverancier kan leveren aan elke klant. Zo'n soort markt moedigt leveranciers aan om te innoveren en voorkomt dat klanten afhankelijk worden gemaakt van een eigen technologie. Op deze manier profiteren klanten meer door verbeterde producten en betere ondersteuning.

## Inhoud

## Conceptuele basis

De Java 2 Enterprise Edition is een uitbreiding van de standaard-[J2SDK](https://nl.wikipedia.org/wiki/J2SDK "J2SDK") die erop gericht is het makkelijker te maken om [gedistribueerde applicaties](https://nl.wikipedia.org/wiki/Distributed_computing "Distributed computing") te schrijven in [Java](https://nl.wikipedia.org/wiki/Programmeertaal_Java "Programmeertaal Java"). Om dit te bereiken is J2EE opgetrokken rond het principe van het uitbreiden van de al bestaande, "normale" Java-taal met [API's](https://nl.wikipedia.org/wiki/Application_Programming_Interface "Application Programming Interface") die een aantal zaken toevoegen die noodzakelijk zijn om gedistribueerde applicaties te schrijven voor een zakelijke omgeving. Tot de zaken die J2EE toevoegt, behoren onder andere:

*   Ondersteuning voor de infrastructuur van een gedistribueerde, zakelijke applicatie
*   Ingebouwde ondersteuning voor authenticatie
*   Ingebouwde ondersteuning voor gelijktijdig gebruik door verscheidene gebruikers
*   Geautomatiseerde data-opslag
*   Ingebouwde methoden om data-integriteit te garanderen
*   Ingebouwde ondersteuning voor Enterprise Application Integration, om oudere applicaties mee te ontsluiten
*   Ingebouwde ondersteuning voor _naming services_

## Architecturele basis

J2EE is opgetrokken rond het idee van een _three-tier_\-applicatie: een voorkant die de [interface](https://nl.wikipedia.org/wiki/Interface "Interface") voor de gebruiker regelt, een achterkant waarin data opgeslagen liggen (bijvoorbeeld een [database](https://nl.wikipedia.org/wiki/Database "Database")) en een tussenlaag (of _middle tier_) waar het eigenlijke werk van de applicatie gedaan wordt (de zogeheten _businesslogica_). Deze opzet volgt het principe van de [Model-View-Controller](https://nl.wikipedia.org/wiki/Model-View-Controller-model "Model-View-Controller-model")\-architectuur en het hele J2EE-concept is gebaseerd op de schaalbaarheid van deze architectuur.

Op basis van deze architectuur ondersteunt J2EE de ontwikkeling van twee klassen van applicaties: lichtere applicaties of webapplicaties en de zeer zware, zakelijke applicaties.

### Kleinere, lichtere applicaties en webapplicaties

Kleinere, lichtere applicaties in J2EE worden over het algemeen opgetrokken uit een _view-tier_ aan de voorkant bestaande uit webpagina's (een combinatie van [HTML](https://nl.wikipedia.org/wiki/HyperText_Markup_Language "HyperText Markup Language") en [Java Server Pages](https://nl.wikipedia.org/wiki/JavaServer_Pages "JavaServer Pages")) en een _model-tier_ of _database-tier_ aan de achterkant, al dan niet in combinatie met een _controller-tier_ opgebouwd uit [servlets](https://nl.wikipedia.org/wiki/Servlet "Servlet") ertussenin.

In deze situatie bestaat de interface richting de gebruiker uit een combinatie van statische webpagina's en webpagina's die (gedeeltelijk) dynamisch opgesteld worden bij het laden vanaf de webserver -- dit laatste wordt gedaan door de [JSP](https://nl.wikipedia.org/wiki/JavaServer_Pages "JavaServer Pages")'s (webpagina's met stukken code erin verweven) of door servlets (uitbreidingen op een webserver die in staat zijn desnoods hele webpagina's aan een stuk te genereren). De dynamische webpagina's zijn in principe in staat om direct met de achterliggende database te communiceren, in welk geval ze ook de zakelijke logica bevatten en dus feitelijk de _middle tier_ "absorberen".

Een uitgebreidere variant van dit principe is die waarin servlets ingezet worden als een _middle-tier_\-technologie. In deze opzet bestaat de web/JSP-voorkant alleen maar om data te presenteren aan de gebruiker, het echte werk wordt door servlets gedaan in de _middle tier_. Het is nu feitelijk mogelijk dat servlets ook de uiteindelijke webpagina's genereren, maar vaker wordt ervoor gekozen om data-presentatie puur in webpagina's/JSP's te gieten en de servlets alleen de data af te laten vangen.

De laatste opzet biedt een aantal voordelen boven de eerste. Deze voordelen bestaan voornamelijk uit toegenomen flexibiliteit van de applicatie (in de vorm van de mogelijkheid om verscheidene presentatievormen of JSP's te combineren met dezelfde data zoals aangeleverd door een servlet) en betere onderhoudbaarheid en schaalbaarheid (door het verdelen van taken over verscheidene, losse onderdelen en het vermijden van de spaghetti-structuur die pure websites met een enkele _tier_ eigen is).

### Zwaardere, zakelijke applicaties

Het bijzondere nadeel van de bovenstaande benadering is dat de ontwikkelaar van een dergelijke applicatie er zelf zorg voor moet dragen dat zijn applicatie zichzelf niet gaat "bijten" bij gelijktijdig gebruik door verscheidene mensen (transactionaliteit e.d. zijn bij servlets en JSP's niet ingebakken).

Anders is dit als er gebruik wordt gemaakt van [Enterprise JavaBeans](https://nl.wikipedia.org/wiki/Enterprise_JavaBeans "Enterprise JavaBeans") (min of meer het kroonstuk van de J2EE). EJB's zijn bedoeld als een laatste laag tussen de servlets (of JSP's) en de modellaag. EJB's zijn bedoeld om diensten (interessante, zakelijke logica) aan te bieden aan een geïnteresseerde client-applicatie en om een abstractie te vormen van de inhoud van de database. EJB's maken het voor een ontwikkelaar makkelijk om te abstraheren van database en database-structuur, van specifieke server-infrastructuur en indeling. En EJB's bieden de ontwikkelaar de mogelijkheid om transactionaliteit van zijn applicaties voor zich geregeld te krijgen, om de hele omgang met de database automatisch te laten regelen, om op een mooie manier zijn applicaties te integreren met _legacy_\-systemen en te communiceren met allerhande systemen.

Om dit te bereiken maakt een EJB stevig gebruik van een zogeheten _decorerende proxy_, in J2EE-termen een _container_ geheten. In dit systeem schrijft de ontwikkelaar min of meer alleen maar de interessante stukken van de EJB en laat hij zijn code verder door de applicatieserver inpakken in een pakket dat een poort vormt naar zijn code. Toegang tot zijn code gaat uitsluitend via de poort. En de poort kan hieraan uiteraard ook functionaliteit toevoegen, zoals transactionele communicatie met de database, gesynchroniseerde toegang tot de interessante code enzovoort. De precieze werking van de poort is zelf geen strikt programmeerwerk, maar behoort tot het domein van de declaratieve ontwikkeling -- dit gedrag wordt volledig bepaald door configuratiebestanden, niet door code.

EJB's maken het aanzienlijk eenvoudiger om betrouwbare, schaalbare applicaties te bouwen in afzienbare tijd. Het nadeel van het gebruik van dit systeem is echter dat het veel configuratiewerk vergt en wellicht ook extra netwerkcommunicatie (dus langzamer is). Gebruik is daarom alleen een realistische optie voor zware, zakelijke applicaties die veel klanten kunnen verwachten of zeer betrouwbaar moeten zijn.

## Ondersteunende API's

Hoewel J2EE als platform ongetwijfeld het meest bekend is om zijn component-technologieën ([EJB](https://nl.wikipedia.org/wiki/Enterprise_JavaBeans "Enterprise JavaBeans") en [servlets](https://nl.wikipedia.org/wiki/Servlet "Servlet") plus [JSP](https://nl.wikipedia.org/wiki/JavaServer_Pages "JavaServer Pages")'s) die het de ontwikkelaar mogelijk maken om een gedistribueerde, zakelijke applicatie op te bouwen uit conceptueel nette, gescheiden en herbruikbare componenten, schuilt de werkelijke kracht van J2EE als ontwikkelplatform niet direct in de aanwezigheid van deze component-technologieën.

Veel belangrijker dan de aanwezigheid van de componenten zelf is ongetwijfeld dat J2EE de component-technologie combineert met [API's](https://nl.wikipedia.org/wiki/Application_Programming_Interface "Application Programming Interface") om een aantal zaken te regelen, en niet alleen dat, maar ook deze API's op een dusdanige wijze integreert dat de ontwikkelaar van in ieder geval EJB's zich nauwelijks bewust hoeft te zijn (wel _kan_ zijn, maar niet _hoeft_ te zijn) van de toepassing van die API's.

Tot de ondersteunende technologieën van het J2EE-platform behoren: de JTA ([Java Transaction API](https://nl.wikipedia.org/wiki/Java_Transaction_API "Java Transaction API")), om interactie met databases en andere systemen transactioneel te laten verlopen; de API [JDBC](https://nl.wikipedia.org/wiki/JDBC "JDBC"), om toegang tot de database te abstraheren en zo applicaties te kunnen ontwikkelen die direct overgezet kunnen worden van eender welke database naar eender welke andere database; de API [JNDI](https://nl.wikipedia.org/w/index.php?title=JNDI&action=edit&redlink=1 "JNDI (de pagina bestaat niet)"), die het mogelijk maakt om allerhande zaken binnen een server en netwerk snel terug te kunnen vinden middels een soort "telefoonboek"; de API [JMS](https://nl.wikipedia.org/wiki/JMS "JMS"), die het mogelijk maakt om data uit te wisselen met zeer uiteenlopende applicaties en zo _legacy_\-systemen te ontsluiten; de API [JAAS](https://nl.wikipedia.org/w/index.php?title=JAAS&action=edit&redlink=1 "JAAS (de pagina bestaat niet)"), die in een uniforme beveiliging voor toegang tot applicaties voorziet; en ingebouwde, zeer uitgebreide ondersteuning voor netwerkcommunicatie.

Al deze API's staan de J2EE-ontwikkelaar ter beschikking. De ontwikkelaar van EJB's kan veel ervan vaak zelfs gebruiken zonder erbij na te hoeven denken, omdat ze verstopt zitten in de Bean-container.

## Leverancierneutraal

De J2EE-specificatie geeft een uitgebreide opsomming van de faciliteiten die een J2EE-omgeving moet bieden, inclusief API's en andere technieken. Door nu alleen maar te kijken naar de specificatie, kan een ontwikkelaar applicaties ontwikkelen voor een generieke J2EE-omgeving en zou moeten kunnen verwachten dat die applicatie verder vlekkeloos draait in iedere J2EE-omgeving die de specificatie ondersteunt. J2EE biedt dus in principe niet alleen een aantrekkelijk ontwikkelplatform, maar ook een mechanisme dat een gebruiker van een applicatie en ook de ontwikkelaar ervan loskoppelt van een specifieke omgeving. Dit biedt voordelen met betrekking tot toekomstige ondersteuning en toepasbaarheid van ouder wordende applicaties. De specificatie geeft echter niet aan hoe een en ander geïmplementeerd dient te worden - dat laat de specificatie over aan de maker/verkoper van de omgeving. Het gevolg hiervan in de praktijk is dat de verschillende J2EE implementaties van de diverse leveranciers niet 100% compatibel met elkaar zijn; wat op het ene platform ontwikkeld is, draait niet noodzakelijkerwijs zonder aanpassingen op de applicatieserver van een andere leverancier. Daarmee wordt ingeleverd op de ambitie van J2EE om een platform te zijn waarin de omgeving voor softwareontwikkeling en die voor het gebruik van de software volledig los van elkaar staan en ligt een _[vendor lock-in](https://nl.wikipedia.org/wiki/Vendor_lock-in "Vendor lock-in")_ ("koppelverkoop") toch weer op de loer.

## Nadelen van J2EE

Het belangrijkste nadeel van J2EE is ongetwijfeld de complexiteit van het opzetten en beheren van het ondersteunende systeem. Servermachines, applicatieservers, databaseserver, instellingen van al deze onderdelen, het zijn allemaal zaken die tot in de puntjes verzorgd moeten zijn om een goed functionerend systeem te verkrijgen. Dit geldt zeker als gebruik wordt gemaakt van EJB's.

Bij de overweging J2EE en EJB's te gaan gebruiken, is het daarom van belang goed na te gaan of de complexiteit van het systeem opweegt tegen de voordelen die het biedt. J2EE is op zich geschikt voor de ontwikkeling van allerlei soorten gedistribueerde applicaties, maar zeker het inzetten van een EJB-architectuur is door de complexiteit eigenlijk voorbehouden voor de zwaardere toepassingen, waar heel wat geld achter zit.

## J2EE-applicatieservers

Om een J2EE-applicatie te kunnen gebruiken is een J2EE-applicatieserver nodig. De applicatieserver implementeert de door de J2EE-specificatie beschreven API's. Een J2EE-applicatieserver bestaat functioneel gezien uit een tweetal _containers_: een webcontainer en een EJB-container. De webcontainer wordt gebruikt om servlets en JSP's te draaien, de EJB-container draait de EJB's.

Er zijn allerlei applicatieservers beschikbaar: