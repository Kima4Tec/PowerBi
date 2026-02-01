# DAX
*from Learn Microsoft*

## Syntax
<img width="474" height="142" alt="qsdax_1_syntax" src="https://github.com/user-attachments/assets/664cdf45-eadd-471c-aef0-a1856541149c" />

Denne formel indeholder følgende syntakselementer:

A. Målingens navn, Total Sales.

B. Lighedstegns-operatoren (=), som angiver starten på formlen. Når den beregnes, returnerer den et resultat.

C. DAX-funktionen SUM, som lægger alle tallene sammen i kolonnen Sales[SalesAmount]. Du lærer mere om funktioner senere.

D. Parenteser (), som omslutter et udtryk, der indeholder ét eller flere argumenter. De fleste funktioner kræver mindst ét argument. Et argument sender en værdi til en funktion.

E. Den refererede tabel, Sales.

F. Den refererede kolonne, [SalesAmount], i tabellen Sales. Med dette argument ved SUM-funktionen, hvilken kolonne den skal summere.


Her er min løsning på opgaven:

<img width="391" height="198" alt="qsdax_3_chart" src="https://github.com/user-attachments/assets/ff2dfaca-20c5-47d4-9012-db8cb01716b2" />


**Funktionsreferencen er god i læringsprocessen:**
https://learn.microsoft.com/da-dk/dax/


## Funktioner (Functions)
Funktioner er foruddefinerede formler, der udfører beregninger ved hjælp af specifikke værdier, kaldet argumenter, i en bestemt rækkefølge eller struktur. Argumenter kan være andre funktioner, en anden formel, udtryk, kolonnereferencer, tal, tekst, logiske værdier såsom SAND eller FALSK eller konstanter.

DAX indeholder følgende kategorier af funktioner: Dato og klokkeslæt , Tidsintelligens , Information , Logisk , Matematisk , Statistisk , Tekst , Forældre/underordnede og Andre funktioner. Hvis du er bekendt med funktioner i Excel-formler, vil mange af funktionerne i DAX ligne hinanden. 

## Context
Kontekst er et af de vigtigste DAX-koncepter at forstå. Der er to typer kontekst i DAX: rækkekontekst og filterkontekst. Vi vil først se på rækkekontekst.

#### Rækkekontekst
Rækkekontekst kan lettest betragtes som den aktuelle række. Den anvendes, når en formel har en funktion, der anvender filtre til at identificere en enkelt række i en tabel. Funktionen vil i sagens natur anvende en rækkekontekst for hver række i tabellen, som den filtrerer over. Denne type rækkekontekst anvendes oftest på målinger.

#### Filtrer kontekst
Filterkontekst er lidt sværere at forstå end rækkekontekst. Du kan nemmest tænke på filterkontekst som: Et eller flere filtre anvendt i en beregning, der bestemmer et resultat eller en værdi.

Filterkontekst findes ikke i stedet for rækkekontekst; den gælder snarere ud over rækkekontekst. For f.eks. yderligere at indsnævre de værdier, der skal inkluderes i en beregning, kan du anvende en filterkontekst, som ikke kun angiver rækkekonteksten, men også angiver en bestemt værdi (filter) i den rækkekontekst.

Filterkontekst er let at se i dine rapporter. Når du f.eks. tilføjer TotalCost til en visualisering og derefter tilføjer År og Region, definerer du en filterkontekst, der vælger et delmængde af data baseret på et givet år og en given region.

Hvorfor er filterkontekst så vigtig for DAX? Du har set, at filterkontekst kan anvendes ved at tilføje felter til en visualisering. Filterkontekst kan også anvendes i en DAX-formel ved at definere et filter med funktioner som ALLE, RELATEREDE, FILTER, BEREGN, efter relationer og efter andre målinger og kolonner. Lad os f.eks. se på følgende formel i en måling med navnet Butikssalg:

<img width="735" height="149" alt="qsdax_4_context" src="https://github.com/user-attachments/assets/5b9fa5ce-41d0-4d1f-a671-79da08bf1163" />

For bedre at forstå denne formel kan vi opdele den, ligesom med andre formler.

Denne formel indeholder følgende syntakselementer:

A. Målenavnet, Butikssalg .

B. Lighedstegnet ( = ), som angiver starten af formlen.

C. Funktionen CALCULATE , som evaluerer et udtryk som et argument i en kontekst, der er ændret af de angivne filtre.

D. Parenteser () , som omgiver et udtryk, der indeholder et eller flere argumenter.

E. En måling [Samlet salg] i den samme tabel som et udtryk. Målingen for samlet salg har formlen: =SUM(Salg[Salgsbeløb]).

F. Et komma ( , ), der adskiller det første udtryksargument fra filterargumentet.

G. Den fuldt kvalificerede refererede kolonne, Channel[ChannelName] . Dette er vores rækkekontekst. Hver række i denne kolonne angiver en kanal, f.eks. Butik eller Online.

H. Den specifikke værdi, Store , som et filter. Dette er vores filterkontekst.

Denne formel sikrer, at kun salgsværdier defineret af målet Total Sales beregnes for rækker i kolonnen Channel[ChannelName], hvor værdien Store bruges som filter.
