# 1. Gebruik van R

R is een softwareprogramme voor datamanipulatie, berekening en het grafisch voorstellen van data. Het heeft onder meer: 

1. Een effectieve gegevensbeheer- en opslagfaciliteit
2. Een reeks operatoren voor berekeningen op arrays, in het bijzonder matrices
3. Een grote verzameling van instrumenten voor data-analyse
4. Grafische faciliteiten voor data-analyse en weergave
5. een goed ontwikkelde, eenvoudige en effectieve programmeertaal

Hulpfaciliteit: 

```
> help (functie)
```

of

```
> ?functie
```

## 1.1 Commando's opslaan en output uitvoeren

Commandos op extern bestand: 
```
> source ("commands.R")
```

Uitvoer van console naar bestand wegschrijven
```
> sink ("bestand.lis")
```

## 1.2 R omgeving en workspace

Entiteiten die R creëert staan bekend als objecten zoals variabelen of arrays va cijfers. Tijdens een R sessie worden deze opgeslagen op naam.

Alle objecten tonen:
```
> objects()
```

Objecten verwijderen:
```
> rm(x, y)
```

## 1.3 Toewijzing

De meest directe manier om een lijst met nummers op te slaan is via het c-commando, dit staat voor combine. Een andere term voor een lijst nummers is een **vector**
```
> variabele <- c(10.4, 5.6, 3.1)
```

Een nummer gebruiken:
```
> x[2]
```

## 1.4 Een csv file lezen

Gegevensbestanden lezen gebeurt via het read.csv() commando.

Weergeven van gedefinieerde kolommen: 
```
> names(computers)
```

Alles vermelden dat gebruikt wordt om variabele te beschrijven:
```
> attributes(computer)
```

## 1.5 Data types

We kijken naar enkele manieren waarop R gegevens kan opslaan en organiseren. Dit is echter een inleiding dus beschouwen we maar een kleine subset van de verschillende datatypes die door R worden herkend

### Numbers

De meest eenvoudige manier om een nummer op te slaan is om een variabele van een enkel getal te nemen:
```
> a <- 3
```

Hiermee kunt u allerlei basisoperaties doen en opslaan:
```
> b <- sqrt (a*a+3)
> b
 [1] 3.464102
```

Lijst met nummers initialiseren via **numeric**
```
> a <- numeric(10)
> a
 [1] 0 0 0 0 0 0 0 0 0 0
> typeof(a)
 [1] double
```

### Strings

Een tekenreeks wordt gespecifieert door gebruik te maken van aanhalingstekens. Zowel enkelvoudig als dubbele werken.
```
> a <- "hello"
> a
 [1] "hello"
> b <- c("hello", "there")
> b
 [1] "hello" "there"
>b[1]
 [1] "hello"
```

### Factors

Vaak bevat een experiment proeven voor verschillende niveaus van een verklarende variabele. Bijvoorbeeld een nominale variabele die gecodeerd wordt met een integer. De verschillende niveaus worden ook factoren genoemd

Je geeft aan dat een variabele een factor is met behulp van het **factor command**

### Data Frames

Data kan woren opgeslagen aan de hand van dataframes. Dit is een manier om verschillende vectoren van verschillende types te nemen en ze op te slaan in dezelfde variabele. Dataframe kan verschillende vectoren bevatten en elke lijst kan een vector zijn van factoren, strings of nummers.

```
> a <- c(1,2,3,4)
> b <- c(2,4,6,8)
> levels <- factor(c("A", "B", "A", "B"))
> bubba <- data.frame(first=a, second=b, f=levels)
> bubba
  first second f
1     1      2 A
2     2      4 B
3     3      6 A
4     4      8 B
> summary(bubba)
     first          second    f    
 Min.   :1.00   Min.   :2.0   A:2  
 1st Qu.:1.75   1st Qu.:3.5   B:2  
 Median :2.50   Median :5.0        
 Mean   :2.50   Mean   :5.0        
 3rd Qu.:3.25   3rd Qu.:6.5        
 Max.   :4.00   Max.   :8.0        
> bubba$first
[1] 1 2 3 4
> bubba$second
[1] 2 4 6 8
> bubba$f
[1] A B A B
Levels: A B
```

### Logische variabelen

Er zijn twee vooraf gedefinieerde variabelen: **TRUE** en **FALSE**

### Tables

Een andere manier om informatie op te slaan is in een tabel. We kijken alleen naar het maken en definiëren van een tabel.

```
> a <- factor(c("A", "A", "B", "A", "B", "B", "C", "A", "C"))
> results <- table(a)
> results
a
A B C
4 3 2
> attributes(results)
$dim
[1] 3

$dimnames
$dimnames$a
[1] "A" "B" "C"


$class
[1] "table"

> summary(results)
Number of cases in table: 9
Number of factors: 1
```

Als je rijen wil toevoegen, voeg dan nog en vector toe als argument van de tableopdracht.

```
> a <- c("Sometimes", "Sometimes", "Never", "Always", "Always", "Sometimes", "Sometimes", "Never")
> b <- c("Maybe", "Maybe", "Yes", "Maybe", "Maybe", "No", "Yes", "No")
> results <- table(a,b)
> results
           b
a           Maybe No Yes
  Always        2  0   0
  Never         0  1   1
  Sometimes     2  1   1
```

### Matrix

Een matrix is een verzameling van gegevens die zijn aangebracht in een tweedimensionale rechthoekige indeling.

```
> A = matrix(
+   c(2,4,3,1,5,7),
+   nrow=2,
+   ncol=3,
+   byrow=TRUE)

> A
     [,1] [,2] [,3]
[1,]    2    4    3
[2,]    1    5    7

> A[2,3]
[1] 7

> A[2, ]
[1] 1 5 7

> A[ ,c(1,3)]
     [,1] [,2]
[1,]    2    3
[2,]    1    7
```

# 2. Het onderzoeksproces

## 2.1 De wetenschappelijke methode

Verschillende methodes om kennis te vergaren: 

* Wetenschappelijke methode
* Niet-wetenschappelijke methode

### Niet-wetenschappelijk:

* Autoritair: Iemand geldt als autoritair en wordt als betrouwbaar bestempeld, alles wat die zegt wordt gezien als waarheid
* Deductief: Gegeven een set va, veronderstellingen gaat men op een welbepaalde manier conclusies trekken. Afhankelijk van de waarheid van de veronderstellingen.

### Wetenschappelijk:

Een kenmerk van de wetenschappelijke methode is **empirische validering** gebaseerd op ervaring en directe observatie. Dus een uitspraak is geldig indien het overeen komt met wat geobserveerd wordt.

Aan de hand van empirisch onderzoek kunnen we verschillende doelen behalen:

1. Exploratie: bestaat iets of gebeurt er iets
2. Beschrijving: wat zijn de eigenschappen van deze gebeurtenis
3. Voorspelling: is een bepaalde gebeurtenis gerelateerd aan een andere en kan ik deze zo voorspellen
4. Controle: kan ik een gebeurtenis volledig voorspellen aan de hand van andere zaken

### Onderzoeksdoelstellingen

Er zijn 2 grote onderzoekdoelen die we willen behalen: 

* **Generalisatie**: we gaan vaak maar een onderzoek doen op een bepaalde, beperkte groep van de totale groep, als de conclusies voor de subgroep ook gelden voor de totale groep, hebben we een correcte generalisatie gevonden.
* **Specialisatie**: toepassen van algemene kennis op een specifiek domein of probleem.

2 Soorten generalisatie: 

* Over 1 enkel fenomeen
* Over verbanden tussen fenomenen

Waarom zijn verbanden zo belangrijk?

1. Volledig verstaan van een fenomeen
2. Verbanden kunne zorgen voor een voorspelling
3. Casuale verbanden: een van de fenomenen heeft dat andere fenomeen tot gevolg

### Fundamenteel vs Toegepast onderzoek

#### Fundamenteel onderzoek

* Om oplossingsmethoden te ontwikkelen
* Minder rekening gehouden met prakische toepassingen
* Om kennis in vakgebied uit te breiden

#### Toegepast onderzoek

* Begint bij een concreet probleem
* Probleem moet opgelost worden
* Meestal in bedrijfscontext

## 2.2 Basisconcepten in onderzoek

### Meetniveaus

In statistiek werken we met variabelen en waarden

* Variabele: algemene eigenschap van een object waardoor we objecten van elkaar kunnen onderscheiden _bv. lengte_
* Waarde: specifieke eigenschap, invulling voor die variabele _bv. 1m98_

Er worden 4 meetniveaus gebruikt, het niveau bepaalt welke methodes bruikbaar zijn

* **Nominaal meetniveau**: Slechts keuze uit bepaald aantal categorieën, waarbij geen volgorde aanwezig is tussen de antwoorden _bv. man of vrouw_
* **Ordinaal meetniveau**: Een variabele die is ingedeeld in categorieën, waar er echter wel een logische volgorde is tussen de categorieën _bv. goud zilver brons_
* **Intervalniveau**: variabelen die niet n categorieën voorkomen, en waarbij berekeningen kunnen worden uitgevoerd, maar zonder absoluut nulpunt _bv. teperatuur: temperatuur kan 0 zijn maar dit wil niet zeggen dat er geen temperatuur is_
* **Rationiveau**: intervalniveau met nulpunt, je kunt hierdoor verhoudingen berekenen tussen verschillende waarden op de schaal, als een waarde 0 is dan staat dat voor afwezigheid _bv. leeftijd of inkomen_

### Onderzoeksproces

Kan worden opgedeeld in 6 grote fasen: 

1. Formuleren van de probleemstelling -> _Wat is de onderzoeksvraag_
2. Exacte informatiebehoefte definiëren -> _Welke vragen stellen?_
3. Uitvoeren van het onderzoek -> _Enquêtes, simulaties, ..._
4. Verwerken van de gegevens -> _Statische software_
5. Analyseren van de gegevens -> _Uitvoeren statische methodes_
6. Conclusies schrijven -> _Schrijven onderzoeksverslag_

**Oorzakelijk verband**: Een variabele veroorzaakt een oorzakelijk verband wanneer een verandering in die variabele op een betrouwbare manier een geassocieerde verandering van een andere variabele tot gevolg heeft , op voorwaarde dat alle andere potentiële oorzaken geëlimineerd zijn

# 3. Analyse op 1 variabele

## 3.1 Beschrijvende statistiek

Met beschrijvende statistiek bedoelen we een verzameling van technieken om data te synthetisch voor te stellen en samen te vatten.

## 3.2 Gemiddelde

Het gemiddelde van een set waarden is de som van al deze waarden gedeeld door het aantal waarden. Gemiddelde is gevoelig aan outliers.

![image](./images/Gemiddelde.png)

Waarbij: 

* xi de waarden zijn
* n het aantal waarden is

## 3.3 Mediaan

Indien we alle cijfers sorteren van klein naar groot, is de mediaan het middelste cijfer, of het 2e middelste cijfer als het aantal even is. Mediaan is niet gevoelig voor outliers.

## 3.4 Modus

Het cijfer dat het meeste voorkomt in een set cijfers

* Heeft geen zin als alle cijfers evenveel voorkomen
* Bimodaal: twee modi
* Multimodaal: meerdere modi

## 3.5 Range / Bereik

Het bereik in een set van getallen is de absolute waarde van het verschil tussen het laagste en het grootste getal

## 3.6 Kwartielen & kwartielafstand

De kwartielen zijn de waarden die een gesorteerde lijst van nummers in 4 gelijke delen deelt. Elk deel vormt dus een kwart van de dataset

Dus:

* Het eerste kwartiel Q1 is de getalwaarde die de laagste 25% van de reeks afscheidt
* Het tweede kwartiel Q2 is de getalwaarde die de laagste 50% van de reeks afscheidt
* Het derde kwartiel Q3 is de getalwaarde die de laagste 75% van de reeks afscheidt

De **kwartielafstand** is het verschil tussen Q3 en Q1

![image](./images/Kwartiel.png)
![image](./images/Kwartiel1.png)

## 3.7 Variantie en standaardafwijking

De variantie is het gemiddelde van de kwadraten van de verschillen tussen de waarde van de dataset en het gemiddelde

![image](./images/Variantie.png)

De standaardafwijking wordt dan gedefinieerd als de vierkantswortel van de variantie

![image](./images/StandaardAfwijking.png)

Dit geeft ons dus zicht op wat normaal en abnormaal is. Kleine SA wil zeggen dat waarden dicht bij centrummaat liggen en grote SA wil zeggen dat de waarden verspreid liggen.

## 3.8 Centrummaat en Spreidingsmaat toepassen

![image](./images/Meetniveaustabel.png)

## 3.9 Grafieken

### Boxplot

De boxplot wordt gevormd door een rechthoek begrensd door de kwartielwaarden, ook de mediaan wordt er in getekend. De stelen bevatten de rest van de waarnemingen op uitschieters op extremen na.

* **Uitschieter**: waarde die meer dan 1,5 keer de interkwartielafstand boven/onder het derde/eerste kwartiel ligt. Aangeduid met cirkeltje.
* **Extremum**: waarde die meer dan 3 keet de interkwartielafstand boven/onder het derde/eerste kwartiel ligt. Aangeduid met sterretje.

![image](./images/BoxplotVoorbeeld.png)

## 3.10 R

Zodra u een vector in het geheugen hebt, zijn de meeste basisbewerkingen beschikbaar. Indien je een operatie uitvoert op meerdere vectoren, is het vaak nodig dat de vectoren allemaal hetzelfde aantal elementen hebben

```
> a <- c(1,2,3,4)
> a
[1] 1 2 3 4
> a + 5
[1] 6 7 8 9
> a - 10
[1] -9 -8 -7 -6
> a * 4
[1]  4  8 12 16
> a / 5
[1] 0.2 0.4 0.6 0.8

> b <- a - 10
> b
[1] -9 -8 -7 -6

> sqrt(a)
[1] 1.000000 1.414214 1.732051 2.000000
> exp(a)
[1]  2.718282  7.389056 20.085537 54.598150
> log(a)
[1] 0.0000000 0.6931472 1.0986123 1.3862944
> exp(log(a))
[1] 1 2 3 4

> c <- (a + sqrt(a))/(exp(2)+1)
> c
[1] 0.2384058 0.4069842 0.5640743 0.7152175

> a + b
[1] -8 -6 -4 -2
> a * b
[1]  -9 -16 -21 -24
> a / b
[1] -0.1111111 -0.2500000 -0.4285714 -0.6666667
> (a+3)/(sqrt(1-b)*2-1)
[1] 0.7512364 1.0000000 1.2884234 1.6311303
```
