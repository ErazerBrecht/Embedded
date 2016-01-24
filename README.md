# Embedded 5

***Gelieve niet the pushen naar de master branch! Fout gevonden? Open aub een pull-request! Commits in de master die niet van mezelf zijn worden gereset! </br> Dit is makkelijk op GitHub (2de radiobutton indien je deze file edit)***

Deze README bevat mijn antwoorden op de vragenlijst van de te kennen leerstof voor het examen van Embedded.
De 1ste 18 vragen zijn voor na de herfstvakantie, de rest volgt later.

## Module 1

#### 0. Standaardbegrippen (Extra vraag van Brecht C)
**Gemiddelde**

Alle samples optellen en delen door het aantal samples. In de DSP wereled werken we met indexen zoals arrays. Dit betekend dus dat als we 512 sampels hebben, het van 0 tot 511 gaat (Vandaar die N-1).

In elektronica is het gemiddelde van een signaal de DC component.

![Gemiddelde](http://i.imgur.com/PHUZExK.png)

**Standaarddeviatie**

Beschrijft hoe hard het signaal afwijkt van zijn DC component. Het wordt ook wel de variatie genoemd.
Een hogere standaarddeviatie betekend dus ook een hogere piek tot piek waarde!

We bereken dit door van elke samplewaarde het gemiddelde af te trekken. We doen dit tot de tweede omdat we graag werken met vermogen i.p.v. spanning (de optelling van 2 ruissignalen is gelijk aan de optelling van beide hun vermogen, niet hun amplitude). Daarna nemen we de gemiddelde waarde van al deze getallen. (Let op delen door N -1, zie vraag 3.1 voor verklaring).

![Standaarddeviatie](http://i.imgur.com/P2E9Dx5.png)

**"Lopende" statistieken**

Om de standaarddeviatie te berekenen van een signaal heb je dus het gemiddelde nodig. Met de formules die we gezien hebben kunnen we pas het gemiddelde berekenen als het volledige signaal is opgeslagen in de DSP. We zouden dus altijd het volledige signaal moeten bufferen voor we er bewerkingen op kunnen doen, dit is absoluut niet de bedoeling!

We kunnen die oplossen door aan "lopende" statistieken te doen, of zogenaamd berekening doen elke keer er een nieuwe sample binnenkomt. Dit is veel efficiënter!

![Loopende statistieken](http://i.imgur.com/JjBc8Oz.png)

Bij deze formule kun je de gegevens elke sample "updaten", zo hoef je niet elke samplewaarde op te slagen in het geheugen!

**SNR / CV**

In sommige gevallen is het gemiddelde eigenlijk hetgeen je wouw meten, en is de standarddeviatie de ruis. In dit geval is het belangrijk dat je de verhouding tussen beide kent. **Signaal-to-noise verhouding** (SNR) is dus het gemiddelde gedeeld door de standarddeviatie. Hoe hoger de SNR hoe beter de kwaliteit van he signaal is (minder ruis). *Formule staat fout in de slides!*

CV is de **coefficient of variation**, dit is eigenlijk het tegenovergestelde. Dus standaarddeviatie gedeeld door het gemiddelde vermenigvuldigd met 100%. </br>
SNR = 50 => CV = 2%


TODO: gauss curve

#### 1. Welk nut heeft het om een histogram-op te stellen van een discreet signaal? Welke informatie kan je hieruit halen?
Bij een histogram kun je zien hoeveel keer bepaalde waarde voorkomt. 

Dit brengt enkele voordelen:
- Je kunt sneller het gemiddelde en de standaardeviatie afleiden van een verkregen signaal. Het gemiddelde en de standaarddeviatie berekenen vereist veel tijd (constant optellen en vermenigvuldigen). Indien je weet hoevaak elke waarde voorkomt moet je deze bewerking minder herhalen. Dit kan zorgen voor + 10x snellere berekeningen van het algoritme
- Bijvoorbeeld bij afbeeldingen (meer dan miljoenen sampels) is het makkelijker om bewerkingen te doen. (Zie photoshop en etc.)
  - Kleuren veranderen (filters)
  - Helderheid en etc. bewerken
  
#### 2. Welk nut heeft het om een pmf op te stellen van een discreet signaal? Welke informatie kan je hieruit halen?
**pmf = probability mass function**

Een pmf is een afgeleide van een histogram (zie volgende vraag). Een pmf zegt hoeveel kans er bestaat dat die bepaalde waarde voorkomt => PROBABILITY (KANS)

Een pmf kan belangrijk zijn in DSP, stel je hebt een signaal. Dan kun je via de pmf grafiek van dit signaal bepalen hoegroot de kans dat de volgende sample een bepaalde waarde is. Het is ook makkelijk om te bepalen hoe groot de kans is dat een waarde kleiner of groter is dan een bepaalde waarde (kansen optellen)

Je stelt een pmf op door de histogram te nemen en elke y - waarde te delen door het aantal sampels.
Soms zal dit niet voldoen en zijn er wiskundige technieken om de pmf te bepalen (zie volgende vraag waarom).

### 3.1 Wat is het verschil tussen een signaal en het onderliggend proces? (Extra vraag van Brecht C)
Een signaal is wat je meet.
Het onderligend proces is wat het signaal genereert. 

Makkelijk voorbeeld: Kop of munt.
Het proces bestaat uit 2 resultaten kop of munt. De kans dat je kop / munt hebt is de helft. Indien kop 0 voorsteld en munt 1 voorsteld heb je dus ook een gemiddelde van 0.5. </br>
Indien je nu 100 dit proces zou uitvoeren, heb je dus 100 sampels. De kans dat je hier perfect een gemiddelde van 0.5 zou hebben is klein. Het verschil tussen dat gemiddelde en het gemiddelde van 0.5 noemen we **statistische ruis**

Indien je het nu nog niet door hebt een klein ander voorbeeld:
Het nettoloon van een land is gemiddeld 2050 (het proces). </br>
Je neemt nu van 5 mensen hun loon (aka 5 sampels) 
- 2051
- 2053
- 2055
- 2050
- 2051

Het gemiddelde hiervan is 2052. Het verschil tussen 2050 n 2052 noemen we dus de statische ruis. Indien je nu de standaardeviatie berekent en je gebruikt als gemiddelde 2052 (het gemiddelde van het signaal) is dit eigenlijk fout want het feitelijke gemiddelde is 2050. Dit is ook de reden dat we door N - 1 delen om de standarddeviatie te berekenen i.p.v. door N.
Hierdoor wordt de statische ruis gecompenseert om zo dicht mogelijk bij het correcte beeld van het proces te komen!

### 3.2 Wat is/zijn de verschillen tussen een histogram, pmf en pdf bij digitaal signaal verwerking? Geef ook aan waarvoor ze nuttig zijn.
Het voornaamste verschil is de eenheid van de Y - as.

##### Histrogram
Y - as => Hoeveelheid. </br>
Zegt hoevaak een bepaalde waarde voorkomt in het *signaal* (dus niet in het onderliggend proces)

##### PMF (probability mass function)
Y - as => Kans (uitgedrukt van 0 tot 1, maal 100% en je weet het in procent)
Probeert een voorspelling te maken van hoevaak het *proces* een bepaalde waarde heeft. Dit kon je dus berekenen a.d.h.v. het histogram maar aangezien we hier een schets proberen te maken van het proces is het belangrijk dat we genoeg samples hebben!!! (Kan gecorrigeerd worden met Wiskunde maar dit valt buiten de scope van dit vak!)

##### PDF (probability density function)
Y - as => Kans dichtheid
Deze grafiek is hetzelfde als de PMF buiten dat deze gebruikt wordt bij analoge signalen (signalen die oneindig veel waardes kunnen hebben).

Waarom is hier een verschil in?:<br>
Een pdf van 0.03 bij een amplitude van 120,5 betekent niet dat een spanning van 120,5mV gedurende 3% van de tijd voorkomt in het signaal. Integendeel de kans dat het signaal exact 120,500000000000mV is, is bijzonder klein.
In werkelijkheid: Aangezien er oneindig mogelijke waarden (en schommelingen) zijn. Dit zorgt ervoor dat bij 120.5 mV ook 120,4997, 120,4998, 120,4999, …. Vallen

Hoe kun je dan den eigenlijke kans berekenen?
Stel dus dat bij 121 een kansdichtheid hoort van 0.03... </br>
Dan is de kans dat het signaal tussen 120 en 121 ligt => (121 - 120) * 0.03 = 0.03 => 3%
De kans dat het signaal tussen 120.5 en 120.4 ligt => (120.5 - 120.4) * 0.03 = 0.003 = 0.3% </br>
NOOT: Dit is het geval indien de kansdichtheid constant blijft tussen de twee getallen, indien dit niet het geval is moet je de oppervlakte onder de grafiek berekenen (aka Integraal, maar dit ligt buiten de scope van dit vak!)

### 4. Een signaal met piekwaarde 2 V wordt aangelegd aan een 12-bit ADC (met bereik 5 V). Indien op dit signaal een ruis van 2 mV piek aanwezig is, bepaal dan de totale hoeveelheid ruis na kwantisatie.
Zonder een extra ruis signaal is de ruis toegevoegd door qwantisatie maximum gelijk aan de helft van de LSB (afronden naar boven of onder dus in het slechtste geval zit je er 0.5LSB naast). 

![Kwantisatie Error](http://i.imgur.com/NkTWWpC.png)

Wat is dat nu de helft van de LSB (Least significant bit). Dit is de kleinste stap die je kunt zetten. Hoeveel spannings verschil komt overeen met een verandering van 0000 0000 0000 naar 0000 0000 00001. </br>
In ons geval is dit dus (5V / 2^12) => 5V / 4096 = 1,22mV! => De helft = 0.61mV
Dat betekend dus de maximum afwijking 0.61mV is.

Nu is de afwijing niet altijd  maximaal en volgens de statistiek is de variatie (standaard deviatie) dat er naar boven of naar beneden afgerond wordt dezelfde als een random getal tussen 0 en 1. In dat geval is de standaarddeviatie 0,29 LSB (Hoe die cursus aan dat getal komt, ik heb godverdomme geen idee).

Dus de ruis is gelijk aan de standaarddeviatie (zie vraag 0) en is dus dan gelijk aan **0.29 LSB** (leert er met leven, geen vragen stellen Brecht, Let It Goooooo) => 0,29 . 1,22mV = *0,3538 mV*.

Owkay we zijn er bijna, nu hebben we ook nog die extra ruis op het signaal van 2mV piek. 
Ik ga er even van uit gaan dat hij 2mV RMS bedoeld :) </br>
LSB was 1,22mV => 2mV = 1,64 LSB. Om dit nu te bereken gebruik je Pythagoras (oorspronkelijke ruis => 0,29LSB en nieuwe ruis uitgedrukt in LSB), dus √(0,29² + 1,64²) = 1,665 LSB * 1,22mV = **2,0313mV**

### 4.1 Dither (Extra vraaag van Brecht C)
Indien je een nagenoeg constant signaal aan het kwantiseren bent heb je kans dat de extra ruis die gegeneerd wordt door kwantisatie altijd maximaal is (1 / 2 LSB). Dit heb je bijvoorbeeld als je enkel de waarde 0 of 1 kunt hebben maar je een constant signaal hebt van 0,5. Elke sample zal de digitale waarde er 0,5 naast liggen!

In de praktrijk zal bij DSP het signaal nooit niet echt constant zijn (audio, ...). Toch kunnen we dit probleem voorhebben. We zeggen dan dat het digitaal signaal vast hangt.

![Stuck](http://i.imgur.com/NC1LbjE.png)

Je ziet hier duidelijk dat het analoge signaal veranderd maar dat de kwantisatie teweinig bits heeft om deze verandering te zien. Er wordt constant afgerond wat zorgt voor veel extra ruis.

We kunnen dit oplossen door extra random ruis te voegen. Deze ruis zal ervoor zorgen dat de afronding niet altijd dezelfde is. Dit klink misschien contra productief maar het toevoegen van ruis zorgt uitendelijk voor een kleinere standaarddeviatie!

![Dithering](http://i.imgur.com/HdYEgpX.png)

Dit proces noemen we **dithering**. Dit is dus eigenlijk ruis toevoegen bij het analoge signaal om ervoor te zorgen dat het digitaal signaal dichter ligt bij het oorsponkelijke analoge signaal.

### 4.2 Sampling theorema + Aliasing (Extra vraaag van Brecht C)
Indien je een signaal correct wilt sampelen, mag de frequentie van het signaal niet groter worden dan de helft van de sampel frequentie! Dit noemt het **Nyquist theorema**.

Indien je dit wel doet zullen de sample waardes niet het juiste signaal voorstellen. Het signaal die je dan verkrijgd noemen we een **alias** (vermomming). 

![Nyquist theorema](http://i.imgur.com/Z8T7yiv.png)

Het signaal op de linkse ADC heeft een frequentie van 0,31 keer de sample frequentie, volgens het Nyquist theorema moet deze correct gesampeld worden (0,31 < 0,50). Je kunt zien dat er maar één bepaalde sinus is die voldoet aan de sample punten, je kunt hier dus het correct signaal uithalen.

Het rechtste signaal heeft een frequentie van 0,95 keer de sample frequentie, deze zal volgens het Nyquist theorema niet correct gesampeld worde (0.95 !< 0,50). Je kunt uit de sample punten de sinus halen van het ingangssignaal. Maar je ziet duidelijk dat er nog een andere sinus ook voldoet aan de sample punten (een alias). Dit signaal was niet aanwezig in ons ingangsignaal en is daarom onwenselijk!

Je vermijd aliasing door altijd het signaal door een laagdoorlaatfilter te laten gaan. De cut - off frequentie (-3db punt) moet gelijk zijn aan minstens de helft van de sample frequentie. Zo zijn we zeker dat er geen aliasing kan ontstaant door te sampelen!

### 5. Hoeveel ruisvermogen levert een 8-bit ADC toe aan het signaal (na kwantisatie) als deze een bereik heeft tussen -5V en + 5V?
8 bit => 0 tot 255 </br>
0 = -5V</br>
255 = 5V</br>
Delta = 10V</br>

10V / 255 => 0,0392V => 39,216mV        (Kleinste stap die mogelijk is!!!) </br>
Maximum ruis => 0,5 LSB => 39,216 * 0,5 = 19,608 mV
 
Ruis (standaarddeviatie) => 0,29 LSB => 11,3725mV </br>
**Ruisvermogen => σ² => 11,3725mV² => 129,333 mW**

####Ben niet zeker, ik heb een mail gestuurd naar Patje


### 6. Leg het principe (algoritme) uit hoe je een digitale ruisgeneratie kan bekomen die een Gauscurve benadert. Geef ook aan voor welke toepassing(en) je deze digitale ruisgeneratie kan gebruiken
Digtitale ruisgeneratie kan gegenereerd door een **random number generator**.

Indien je een random getal laat genereren door je computer tussen nul en één is de standaarddeviatie 0,29 (1/√12), en het gemiddelde 0.5.
Indien je dit getal nu optelt met een nieuw random getal tussen nul en één, krijg je een standaarddeviatie van √(0.29² + 0.29²) = 0.41 (1/√6) en een gemiddelde van 1 </br>
Indien je dit in het totaal 12 keer doet zul je dus een standard deviatie van 1 hebben en een gemiddelde van 6.
Een gauscurve heeft een standard deviatie van 1 (zie vraag 0). 

![C# <3](http://i.imgur.com/HoB3mPH.png)

Ik heb even een C# programma geschreven waarin ik 65536 keer een getal genereer. Dit getal is de optelling van 12 random getallen tussen 0 en 1. Daarna heb ik deze getallen geplot in een grafiek, en inderdaad er komt een guass curve uit met een standaarddeviatie van 1. (En nee dat in Scilab maken was te mainstream!!!)

We moeten random ruis kunnen genereren om zo verschillende aspecten van ons algoritme te kunnen testen in het bijzijn van ruis. Bijvoorbeeld: Ruis heeft effect op hoever een radio kan communiceren, hoeveel radiatie er nodig is om een X-ray foto te kunnen maken.

### 7. Oefening convolutie
Gegeven: volgend ingangssignaal: **x[0] = 4, x[1]=-2, x[2] = 8, x[3] = -1, x[4] = -1, x[5] = -2.** </br>
Het functiesignaal h[n] bestaat uit de samples: **h[0] = -1, h[1] = 1, h[2] = -0,5, h[3]= +0,5.** </br>
Gevraagd: bepaal **y[n]** </br>

y[0] = x[0] * -1 = 4 * -1 = **-4** </br>
y[1] = (x[0] * 1) + (x[1] * -1) =  4 + 2 = **6** </br>
y[2] = (x[0] * (-0,5)) + (x[1] * 1) + (x[2] * -1) = -2 + (-2) + (-8) = **-12** </br>
y[3] = (x[0] * 0,5) + (x[1] * (-0.5)) + (x[2] * 1) + (x[3] * -1) = 2 + 1 + 8 + 1 = **12** </br>
y[4] = (x[1] * 0,5) + (x[2] * (-0.5)) + (x[3] * 1) + (x[4] * -1) = -1 + (-4) + (-1) + 1 = **-5** </br>
y[5] = (x[2] * 0,5) + (x[3] * (-0.5)) + (x[4] * 1) + (x[5] * -1) = 4 + 0,5 + (-1) + 2 = **5.5** </br>
y[6] = (x[3] * 0,5) + (x[4] * (-0.5)) + (x[5] * 1) = -0,5 + 0,5 + (-2) = **-2** </br>
y[7] = (x[4] * 0,5) + (x[5] * (-0.5)) = -0,5 + 0,5 + 1 = **0.5** </br>
y[8] = (x[5] * 0,5) = -2 * 0,5 = **-1**  </br>


### 7.1 Wat is convolutie + verklaar impulsrespontie en delta functie (Extra vraag van Brecht C)
Convolutie is een wiskundige bewerking voor het combineren van signalen. Het vereist 2 signalen en produceert een derde signaal!

In DSP gebruiken we decomposities om makkelijker bewerkingen te doen met signalen, de twee belangrijkste zijn **impuls decomposite** (vraag 11) en **fourieranalyse**. 

Een impuls waarbij de 1ste sample één is en alle andere 0, noemen we de **delta functie (δ[n])**. We kunnen elke impuls beschrijven in functie van de delta functie. Bijvoorbeeld sample 8 heeft een waarde van -3 (de rest is uiteraard 0). Dan kunnen we zeggen dat deze impuls gelijk is aan -3δ[n-8].

De **impulsrespontie** is het signaal dat uit de uitgang komt indien je de delta functie aanlegt aan de ingang van je systeem (vaak aangeduid door h[n]). 

Aangezien we met lineare systemen (vraag 10.1) werken kunnen we dankzij de homogenity eigenschap en de shift invariance eigenschap aantonen dat wanneer de deltafunctie (δ[n]) resulteert in de impulsrespontie (h[n]), dat dan -3δ[n-8] gelijk zal zijn aan -3h[n-8]. **Dit betekend dus dat als je de impulsrespontie kent, je de uitgang van elke impuls kunt bepalen!**

![Delta functie](http://i.imgur.com/n6kcPSt.png)

Waar komt nu convolutie te pas? De uitgang van een linear systeem is gelijk aan de convolutie tussen de ingang en de impulsrespontie. Het ster symbool in de foto hieronder is het wiskundig symbool voor convolutie (vergelijk het met + voor optellen)

![Convolutie](http://i.imgur.com/2EgfWKV.png)

Een makkelijk voorbeeld is een inverterende versterker / verzwakker.

![Convolutie voorbeeld](http://i.imgur.com/RAPXZ7A.png)

  De middelste grafiek is de impulsrespontie. Dit is dus het resultaat van de delta functie. Je ziet als resultaat dat de sample 15 stappen delay heeft, de amplitude verzwakt is (kleiner als 1) en dat deze geïnverteerd is (onder de nul).

De uitgang is de convolutie van de ingang en de impulsrespontie. Je ziet nu duidelijk dat de uitgang indererdaad 15 samples delay heeft. Dat de uitgang geïnverteerd is, en dat het signaal een beetje verzwakt is!

Laatste opmerking: Hoelang het ingangsignaal of de impulsrespontie is maakt niet uit. De lengte van het uitgangsignaal is de lengte van het ingang signaal + de lengte van de impulsrespontie - 1 </br>
Dus 81 + 31 - 1 = 111 (van 0 tot 110).

### 8. Verklaar met eigen woorden beknopt het begrip discrete afgeleide bij digitaal signaal verwerking. Leg ook uit hoe deze kan worden berekend (formule). Pas discrete afgeleide toe op een voorbeeld.
Discrete afgeleide, is zoals de naam zegt de afgeleide toegepast op een discreet (digitaal signaal). Er moest hier een aparte term voor komen om de verwarring tussen een afgeleide op een analoge / digitaal te voorkomen.

Zoals we weten van onze Wiskunde lessen, is de afgeleide van een signaal **een maat voor hoe hard het signaal varieert**. 
Bij analoge signalen is het moeilijk om de stijging te bepalen tussen twee punten, omdat theoretisch er oneindig veel punten zijn. (Daarom dat we bij afgeleiden in de Wiskunde gebruik maken van limitieten, delta x naar 0 => buiten de scope van dit vak). Gelukkige hebben we dit probleem niet bij digitale sampels, en is **de discrete afgeleide hier dus de huidige sample min de waarde van de vorige sample**.

We kunnen dit makkelijk oplossen met volgende impulsrespontie:

![Impulsrespontie discrete afgeleide](http://i.imgur.com/9Un6May.png)

Indien je deze toepast op een ingangsisgnaal:
- Waarde 0 ingangsignaal => maal 1 => Waarde 0 uitgangsignaal
- Waarde 0 ingangsignaal => maal - 1 + waarde 1 ingangsignaal maal 1 => Waarde 1 uitgangsignaal
- ...

Je ziet het misschien al maar er zit een simpel patroon in. Namelijk dit:

![Discrete afgeleide formule](http://i.imgur.com/Gegdl0X.png)

Voorbeeld:

![Discrete afgeleide voorbeeld](http://i.imgur.com/XAKapzT.png)

Convolutie oplossing (analoog aan vraag 7):

y[0] = x[0] * 1 = 0 * 1 = 0 </br>
y[1] = (x[0] * -1) + (x[1] * 1) = 0 + 1 = 1 </br>
y[2] = (x[1] * -1) + (x[2] * 1) = -1 + 1 = 0 </br>
y[3] = (x[2] * -1) + (x[3] * 1) = -1 + (-2) = -3 </br>
y[4] = (x[3] * -1) + (x[4] * 1) = 2 + 0 = 2 </br>
y[5] = (x[4] * -1) + (x[5] * 1) = 0 + 3 = 3 </br>
y[6] = (x[5] * -1) + (x[6] * 1) = -3 + 2 = -1 </br>
y[7] = (x[6] * -1) + (x[7] * 1) = -2 + (-2) = -4 </br>
y[8] = (x[7] * -1) + (x[8] * 1) = 2 + (-2) = 0 </br>
y[9] = (x[8] * -1) + (x[9] * 1) = 2 + (-1) = 1 </br>
y[10] = (x[9] * -1) + (x[10] * 1) = 1 + 1 = 2 </br>
y[11] = x[10] * -1 = -1

Formule oplossing:

y[0] = x[0] - x[-1] = 0 - 0 = 0 </br>
y[1] = x[1] – x[0] = 1 - 0 = 1 </br>
y[2] = x[2] – x[1] = 1 – 1 = 0 </br>
y[3] = x[3] – x[2] = -2 – 1 = -3 </br>
y[4] = x[4] – x[3] = 0 – (-2) = 2 </br>
y[5] = x[5] – x[4] = 3 – 0 = 3 </br>
y[6] = x[6] – x[5] = 2 – 3 = -1 </br>
y[7] = x[7] – x[6] = -2 – 2 = -4 </br>
y[8] = x[8] – x[7] = -2 – (-2) = 0 </br>
y[9] = x[9] – x[8] = -1 – (-2) = 1 </br>
y[10] = x[10] – x[9] = 1 – (-1) = 2 </br>
y[11] = x[11] – x[10] = 0 – 1 = -1

Zie ook nog voorbeeld bij de volgende vraag!


### 9. Verklaar met eigen woorden beknopt het begrip discrete integratie bij digitaal signaal verwerking. Leg ook uit hoe deze kan worden berekend (formule). Pas discrete integratie toe op onderstaand voorbeeld:
Discrete integratie (running sum), is de digitale variant van de integraal berekening de we kennen van Wiskunde. 

De integraal van een signaal is eigenlijk de oppervlakte berekenen van dit signaal. In analoge signalen is dit best moeilijk omdat er oneindig veel getallen tussen 2 getallen liggen! We hebben dit probleem niet bij digitale metingen. Bij een digitaal systeem kunnen we zeggen dat **de oppervlakte van één sample gelijk is aan een breedte van 1 maal de waarde van de sample (hoogte). Uitendelijk komt het er dus op neer dat de digitale integraal dus de som is van alle voorige samples + de huidige sample**. Dus de uitgang van sample 0, is 0. De uitgang van sample 1, is 0 + waarde sample 1. De uitgang van sample 2, is 0 + waarde sample 1 + waarde sample 3, ...

![Impulsresponsie discrete integratie](http://i.imgur.com/bzboYy0.png)

Deze spreekt nu eigenlijk voor zich. De sample waarde van de uitgang is gelijk aan de optelling van alle voorgaande samplewaarden. Indien je deze impulsresponsie toepast op een ingangsisgnaal:
- Waarde 0 ingangsignaal => maal 1 => Waarde 0 uitgangsignaal
- Waarde 0 ingangsignaal => maal 1 + waarde 1 ingangsignaal maal 1 => Waarde 1 uitgangsignaal
- Waarde 0 ingangsignaal => maal 1 + waarde 1 ingangsignaal maal 1 + waade 2 ingangsignaal => Waarde 2 uitgangsignaal
- ...

Je ziet het misschien al maar er zit een simpel patroon in. Namelijk dit:

![Discrete integratie formule](http://i.imgur.com/hd59k7W.png)

Voorbeeld:

![Discrete afgeleide voorbeeld](http://i.imgur.com/XAKapzT.png)

Convolutie oplossing (analoog aan vraag 7):

y[0] = x[0] * 1 = 0 * 1 = 0 </br>
y[1] = x[0] * 1 + x[1] * 1 = 0 + 1 = 1 </br>
y[2] = x[0] * 1 + x[1] * 1 + x[2] * 1 = 0 + 1 + 1 = 2 </br>
y[3] = x[0] * 1 + x[1] * 1 + x[2] * 1 + x[3] * 1 = 0 + 1 + 1 - 2 = 0 </br>
... (Hier ben ik echt te tam voor...)

Formule oplossing:

y[0[ = x[0] + y[-1] = 0 + 0 = 0 </br>
y[1] = x[1] + y[0] = 1 + 0 = 1 </br>
y[2] = x[2] + y[1] = 1 + 1 = 2 </br>
y[3] = x[3] + y[2] = -2 + 2 = 0 </br>
y[4] = x[4] + y[3] = 0 + 0 = 0 </br>
y[5] = x[5] + y[4] = 3 + 0 = 3 </br>
y[6] = x[6] + y[5] = 2 + 3 = 5 </br>
y[7] = x[7] + y[6] = -2 + 5 = 3 </br>
y[8] = x[8] + y[7] = -2 + 3 = 1 </br>
y[9] = x[9] + y[8]  = -1 + 1 = 0 </br>
y[10] = x[10] + y[9]  = 1 + 0 = 1 </br>
y[11] = x[11] + y[10] = 0 + 1 = 1 </br>
...

In de analoge Wiskunde zijn een integraal en een afgeleide het "omgekeerde" van elkaar (zoals een vermenigvuldiging en een deling). Indien je dus van een functie de afgeleide neemt er daarna de integraal kom je terug op de oorspronkelijke functie uit. In de digitale wereld is dit niet anders, als je de discrete afgeleide neemt van een signaal en daarna de discrete integraal kom je terug op het oorspronkelijke signaal.

![Discrete integraal vs Discrete afgeleide](http://i.imgur.com/JBKJVlS.png)

### 10.1 Wat is een linear systeem, geef enkele voorbeelden en eigenschappen (Extra vraag van Brecht C)
Een linear systeem is een systeem dat aan bepaalde voorwaarde moet voldoen. 

#####Homogeniteit:
1.	Verandering in amplitude in het ingangssignaal veroorzaakt een overeenkomstige verandering in amplitude van het uitgangssignaal. Bijvoorbeeld de relatie tussen de spanning over een weerstand en de stroom door de weerstand. Als het één een factor 5 stijgt, stijgt het ander ook een factor 5. Een voorbeeld van een niet homogeen systeem is de verhouding tussen de spanning over een weerstand en het vermogen dat gedissipeerd wordt in de weerstand. Als het ene met 5 stijgt, stijgt het andere met een factor 25.

#####Addiviteit:
1.	Signalen opgeteld aan de ingang produceren signalen opgeteld aan de uitgang.
2.	Formule vorm: X1[n]+ X2[n]=> Y1[n]+ Y2[n]
3.	Verschillende aangelegde signalen gaan door het system zonder interactie met elkaar. Bijvoorbeeld een telefoongesprek met achtergrond geluid. Indien je het gesprek afzonderlijk zou door zenden en het achtergrond geluid zou klinkt dit eigenlijk hetzelfde als samen. De signalen zijn niet in elkaar gemengd!

#####Shift invariantie (Geen Wiskundige eis, wel een DSP eis): 
1.	Een verschuiving van hetingangssignaal resulteert in een identieke verandering in het uitgangssignaal. Dit betekend dus dat het systeem niet veranderd in de tijd. 
2.	If X[n]=>Y[n] then X[n+s]=>Y[n+s] for any input signal and any constant s. Dit betekend dus als je eens signaal (array)  later nog eens terug komt dat deze dezelfde uitgang zal genereren. 

Deze voorwaarden zijn vrij wiskundig, je kunt het ook op een andere manier bekijken. Meer intuitief gericht, wij engineers vinden dat leuker, dan die Wiskunde :) !


#####Static linearity
Hoe varieert een signaal als het niet veranderd (statisch, zoals DC spanning). Bij een linear systeem is de uitgang gelijk aan de ingang vermenigvuldig met een constante. Bijvoorbeeld weerstand => stroom ifv spanning. Bij een niet linear systeem kun je dit niet doen, zoals bijvoorbeeld de stroom ifv spanning bij een diode. Er zit daar geen constant verband in! 

Wiskundige uitgelegd, indien de verhouding een eerste graadsvergelijking is => linear </br>
Anders => niet linear (exponentiële, logartimtes, tweedegraadsvergelijking, derdegraadsvergelijking, ...

#####Sinusoidal fidelity
Als de ingang bij een linear systeem sinusoidaal is, moet de uitang dit ook zijn! Dit betekend dus als er storing op je uitgang hebt en hierdoor je uitgang niet meer sinusoidaal is je een niet linear systeem hebt. Bijvoorbeeld bij clipping in een versterkerschakling (saturatie).

####Eigenschappen
Lineare systemen bevaten ook bepaalde eigenschappen die handig kunnen zijn om te weten, zodanig je deze in je voordeel kunt gebruiken!

#####Commutativiteit
Indien je twee lineare systemen hebt maakt het niet uit in welke volgorde deze systemen staan, deze uitgang zal dezelfde zijn.

#####Vermenigvuldigen
Je mag een signaal in een linear systemen vermenigvuldingen met een constante, het uigangssignaal zal nog steeds linear zijn. </br>
Je mag echter geen signaal vermenigvuldigen met een andere varieert signaal, indien je dit doet is het uitgangssignaal niet meer linear. Dit komt omdat het signaal niet meer voldoet aan de static linearity voorwaarde!


### 10.2 Verklaar wat decompositie en synthese is, waarom is het belangrijk? (Extra vraag van Brecht C)
Het proces van signalen met elkaar combineren noemen we **synthesis** (dankzij de vraag hierboven weten dat we enkel d.m.v. optellingen en vermenigvuldigen signalen kunnen combineren met elkaar in een linear systeem).

**Decompositie** is het tegenovergestelde. Hier gaat je een signaal opslitsen is meerdere deelsignalen. Dit is moeilijker dan synthesis. </br>
*Bijvoorbeeld:* Synthesis: 15 + 25 = 40. Decompositie: 40 = 1 + 39, 2 + 38, 15 + 25, ... </br>
Zoals je merkt kun je een signaal op meerdere manieren verkijgen, welke methodes er gebruikt worden bij DSP zie je in de volgende vragen!

Een signaal bestaat meestal uit meerdere signalen. We noemen dit **superpositie**

**Waarom?**
Je gebruikt dit misschien zelfs dagelijks in je leven. Stel je moet 2041 vermenigvuldigen met 4... </br>
Een makkelijke methode om dit op te losen is d.m.v. superpositie. Je kunt 2041 decomposeren in 2000 + 40 + 1. Indien je deze nu apart vermenigvuldigd met 4 en daarna alle resultaten optelt heb je het uiteindelijke signaal.

Net zoals dat het voor ons simpeler was om aan superpositie te doen is het in DSP toepassing ook makkelijker om zo te werken, hierdoor kun je je algoritmes versimpelen!

### 10.3 Verklaar wat stapdecompositie inhoud bij decompositie van een lineair systeem. Geef ook aan welke voordelen stapdecompositie biedt. 
Bij stapdecompositie zul je het signaal decomposeren in evenveel subsignalen als het volledige signaal sampels heeft. Bijvoorbeeld een signaal dat bestaat uit 128 samples zal gedecomposeerd worden in 128 verschillende signalen.

Dit betekend dus dat elke subsignaal maar 1 keer veranderd. **Bij stap compostie wordt elke keer gekeken hoe het signaal veranderd is t.o.v. de vorige sample.**

Stel samples: 5, 3, 0, -5, 10 </br>
**1.** Er is geen vorige sample dus beginnen bij 0 => Signaal stijgt met 5 </br>
**2.** 3 - 5 = - 2 => Signaal daalt met 2 </br>
**3.** 0 - 3 = -3 => Signaal daalt met 3 </br>
**4.** -5 - 0 = -5 => Signaal daalt met 5 </br>
**5.** 10 - (-5) = 15 => Signaal stijgt met 15 </br>

**Voorbeeld met grafieken: (Let op: niets te maken met voorbeeld hierboven!)**

![Stapdecompositie](http://i.imgur.com/eWMhYHL.png)

**Voordelen**
- Signalen kunnen onderzocht worden per samplestap 
- Beschrijft signalen aan de hand van het verschil tussen opeenvolgende samples 
- Beschrijft hoe systemen een respons geven op een verandering van het ingangsignaal

### 11. Verklaar wat impulsdecompositie inhoud bij decompositie van een lineair systeem. Geef ook aan welke voordelen impulsdecompositie biedt.
Bij impulsdecompositie zul je het signaal decomposeren in evenveel subsignalen als het volledige signaal sampels heeft. Bijvoorbeeld een signaal dat bestaat uit 128 samples zal gedecomposeerd worden in 128 verschillende signalen.

Dit betekend dus dat elke subsignaal maar 1 keer veranderd. **Bij impulsdecompositie wordt gewoon elke sample opgeslagen in een apart signaal. De rest van de samples is nul**. Je verkrijgt dus 128 signalen van 128 samples maar waarbij maar 1 sample afhankelijk is van het oorspronkelijke signaal, de rest van samples is dus nul!</br>
Het makelijkste voorbeeld is hier terug ons hoofdrekenen:</br>
Stel 2041: 2000, 40 en 1.

**Voorbeeld DSP**:

![Impulscompositie](http://i.imgur.com/agwRVBX.png)

**Voordelen**
- Signalen kunnen onderzocht worden per samplestap
- Systemen kunnen onderzocht worden hoe ze reageren op impulsen => weten hoe systeem reageert op een bepaalde impuls => systeemoutput kan berekend worden voor een bepaalde input = **convolutie**

### 12. Verklaar wat interlaced decompositie inhoud bij decompositie van een lineair systeem. Geef ook aan welke voordelen interlaced decompositie biedt.
Bij interlaced decompositie zul je het signaal decomposeren in twee subsignalen.

De even sampels (0, 2, 4, ...) worden apart opgeslagen in een subsignaal, de oneven sampels van het originele signaal zijn in dit subsignaal gewoon op 0 gezet. 

De oneven sampels (1, 3, 5, ...) worden apart opgeslagen in een subsignaal, de even sampels van het originele signaal zijn in dit subsignaal gewoon op 0 gezet. 

**Voorbeeld met grafieken**

![Interlacedcompositie](http://i.imgur.com/fQMKvT1.png)

**Voordelen**
Dit is de basis van FFT (Fast Fourier Transform). Fourier analyse is de belangrijkste decompositie in de hele DSP wereld (zie volgend examen). Echter waren de bestaande alogritmes traag. D.m.v. eerst interlaced decompositie toe te passen, daarna de fourier analyse toe te passen op de even en oneven subsignalen en deze resultaten daarna terug te syntheseren ging het wel 100 - 1000 keer sneller! (Versimpleld concept, zie volgende module voor meer).

### 13. Stel dat je een bepaald audiosignaal via een ADC wil overbrengen naar een digitaal signaal proces. Verklaar hoe je van dit audiosignaal het vermogen en peak-to-peak spanning kan bepalen binnen dit digitaal signaal proces.
Peak to peak bepalen kun je door te kijken wat de maximum waarde is in je array. Indien hier geen ingebouwde functie voor bestaat kun je deze zelf makkelijk maken d.m.v. for loop + if. </br>
Hierna gaat je opzoek naar de kleinste waarde. Trek je nu de grootste waarde af van de kleinste heb je de piek tot piek waarde!

Het vermogen van een signaal bereken je door de standaardeviatie van je signaal (array) tot de tweede te doen! Zie vraag 0!

### 14. Beschrijf beknopt aan de hand van tijd- en frequentiekarakteristieken hoe je digitaal – analoog conversie bekomt 
Eerste stap, we vormen onze gegevens in onze array om in **elektrische pulsen (impuls trein)**. Stel we hebben een 4 bit systeem. En we hebben toevallig een bereik van 0 tot 15V. Dan wordt 0000 omgezet in een korte puls van 0V, indien we 1111 hebben wordt dit omgezet in een korte puls van 15V. (Hoe IDK???).

Deze impulstrein kan perfect terug omgezet worden naar het analoge signaal. Dit komt omdat onder de Nyquist frequentie (1/2 sample frequentie), de impulstrein en het analoge signaal hetzelfde frequentie spectrum hebben. D.m.v. een laagdoorlaat filter zou je dus alles boven de Nyquist frequentie kunnen wegfilteren en klaar. Of toch wiskundig klaar, in de praktrijk is het heel moeilijk dunne (maar echte dunne eh) pulsen te maken! 

Grafiek (pas op andere waarden als mijn voorbeeld.)

![Stap 1 DAC](http://i.imgur.com/f2zRoJM.png?1)


Stap 2, de meeste DAC werken met een systeem dat de last waarde behouden wordt tot een nieuwe waarde is. Vergelijkbaar met sample and hold in de ADC. Maar in een DAC noemt dit **zeroth-order hold**. 

Dit heeft jammer genoeg een groot gevolg, het frequentie spectrum van dit signaal is niet meer vergelijkbaar met het te bekomen analoge signaal. Alle frequenties zijn vermenigvuldigd met de sinc functie.

We zouden dit kunnen negeren indien de gevolgen voor de gekozen toepassing niet belangrijk zijn. </br>
We kunnen deze extra sinc "functie" er terug analoog uit filteren (volgende stap). </br>
Of we zouden er software voor kunnen schrijven in onze DSP die dit probleem oplost (volgend examen).

![Stap 2 DAC](http://i.imgur.com/TpOgKS6.png)

Stap 3, we maken dus een analoge laag filter die alle signalen boven de Nyquist frequentie verwijderen! Daarnaast moeten de sinc functie weg corrigeren. De filter die deze twee zaken doet noemen we de reconstructie filter, deze heeft een volgend frequentiespectrum:

![Stap 3 DAC](http://i.imgur.com/4n0yb1c.png)

Uitendelijk hebben we dan terug ons analoog signaal:

![Stap 4 DAC](http://i.imgur.com/f3q10eh.png)

### 15. Welk van onderstaande systemen zijn een lineair systeem en welke niet? Verklaar telkens je antwoord.
![Oef 1 Linear](http://i.imgur.com/LeQKcHE.png)

Dit systeem is lineair omdat dit systeem aan de additieve vereiste voldoet: Verschillende aangelegde signalen gaan door het systeem zonder interactie met elkaar. </br>
Bijvoorbeeld: telefoneren met achtergrond geluid (beide signalen worden onafhankelijk doorgezonden).

![Oef 2 Linear](http://i.imgur.com/1GRf03W.png)

Linear.

![Oef 3 Linear](http://i.imgur.com/74qy6cc.png)

Dit systeem is niet lineair! De sinusoidal fidelity vereiste is niet voldaan! (zie vraag 10.1).
Door de clipping hebben we geen sinusoidiaal signaal meer op de uitgang!

### 16. Hoe kan je zien of een dicreet signaal een bepaalde faseverschuiving heeft of niet.
Je moet kijken naar de symetrie van het signaal!

![Phase](http://i.imgur.com/TOzRqvP.png)


### 17. Hoe kan je de faseverschuiving 0° maken in een lineair systeem.
Dit kun je enkel als het syteem een lineare faseverschuiving heeft. Indien je hier een faseverschuiving wilt bereiken van 0° moet je het symetriepunt naar sample 0 verschuiven (naar links of rechts shiften).

### 18. Wat is een casual systeem.
Indien we een impuls sturen in het signaal zullen we een uitgang (reactie) krijgen op de uitgang. Indien de reactie pas gebeurd nadat er een ingang is geweest spreken we van een **causal** systeem. Bijvoorbeeld de 8ste sample in het signaal beïnvloedt enkel sample nummer acht of hoger in het uitgangsignaal.

Dit klinkt logisch maar aangezien we in DSP werken met array's (geheugen) zouden we perfect in staat zijn om nog vorige samples aan te passen, in dit geval spreken we van een **non-causal** systeem.

Je kunt dit makkelijk herkennen indien je de impulsrespontie kent van een systeem. Als de impulsrespontie waardes heeft voor kleiner dan 0, dan is het *non-causel* anders is het *causel*. Je kunt door gewoon alle samples te shiften naar rechts van elk *non-causel* systeem een *causel* systeem maken.

![causel vs non-causel](http://i.imgur.com/V6cIOfm.png)

## Module 2

### 19. Hoe gebeurt Fourierontbinding en kan je daar een voorbeeld van geven?
Fourier analyse is een familie van wiskundige technieken. Het heeft als doel om een signaal te ontbinden in verschillende sinusoïdale signalen.

We gebruiken sinusoïdale signalen wegens de *sinus fidelity* eigenschap bij lineare systemen. Dit wilt zeggen indien de ingang een sinusoïdaal signaal is, de uitgang dit ook zal zijn.

HOE? IDK, dat is heel dieje boek typen...

![Fourier blok](http://www.brains-minds-media.org/archive/289/dippArticle-18.png)

### 20. In welke categorieën, die voortvloeien uit de vier basistypen van signalen, kan je de term Fourier-transformatie in onderverdelen?

**Reminder:**</br>
Continu: Analoog, oneindige precisie. </br>
Discreet: Digitaal, eindige precisie.


- Fourier transformatie: Signalen die continu en aperiodiek zijn </br>
  ![IDK](http://i.imgur.com/QFMn76P.png)
- Fourier series: Signalen die continu en periodiek zijn </br>
  ![IDK2](http://i.imgur.com/5xSye3U.png)
- Discrete Time Fourier Transform (DTFT): Signalen die discreet en aperiodiek zijn </br>
  ![IDK3](http://i.imgur.com/5xSye3U.png)
- Discrete Fourier Transform (DFT): Signalen die discreet en periodiek zijn  </br>
  ![IDK4](http://i.imgur.com/cQ73nHA.png)

### 21. Hoe kan je Fouriertransformatie gebruiken voor een eindig aantal samples?
Theoretisch gaat dit niet. Sinusoïdale signalen (sinus, cosinus) gaan oneindig lang verder.

Bij een DFT (periodiek), gaan we ervan uit dat ons aantal samples oneindig lang gerepeteerd wordt!

Bij een DTFT (aperiodiek), gaan we de rest opvullen met oneindig veel nullen! 

Een DTFT kan niet opgelost worden met behulp van een computer, de DFT daarentegen wel. Daarom dat we voor DSP de DFT gebruiken! 

### 22. Hoe kan je een aperiodiek signaal syntheseren?
Zie vorige vraag, je gebruikt hier DTFT voor. Dit is echter niet mogelijk met een computer (algoritme)!

### 23. Wat wordt verstaan onder een transformatie?
Een transformatie is het uitvoeren van een functie op meerdere waarden tegelijk. De in- en uitgang kunnen bestaan uit meerdere waarden.

Functie: </br>
y = 2 . x </br>
De uitgang is afhankelijk van de ingang

Transformatie: </br>
Signaal opgebouwd uit 100 samples het omzetten van deze 100 samples in een andere set van 100 samples is een transformatie!

### 24. Wat houdt een voorwaartse discrete fouriertransformatie in?
Voorwaarste discrete fouriertransformatie houdt in dat je van je tijddomein (oorspronkelijke signaal) naar het frequentie domein gaat. 
### 25. Wat houdt een inverse discrete fouriertransformatie in?
Het tijdsdomein opslitsen in het fequency domein. Het frequentie domein bestaat uit een reëel gedeelte (cosinussen) en een imaginiair gedeelte (sinussen). 

Dit is veel moeilijker! Er zijn immers meerdere combinaties (5 = 2 + 3, 5 = 4 + 1). Er is immers maar één correcte. Alles tesamen moet kloppen!

### 26. Stel een 128 punts tijdsdomeinsignaal opgenomen in x[ ]. Hoe verloopt het tijdsmoment van dit signaal en hoe verloopt het frequentiedomein?
Tijdsdomein: van 0 tot 127 (128 sampels)

Frequentiedomein: </br>
- Reëel (cosinusen): Van 0 tot 64 (65 sampels) 
  - Onderverdeeld in 33 verschillende cosinussen (1ste is DC, dan 32 cosinussen met steeds hogere frequentie)
- Imaginair (sinussen): Van 0 tot 64 (65 sampels)
  - Onderverdeeld 33 verschillende sinussen (1ste en laatste zijn nul, de rest steeds verhoging van frequentie)

![DFT](http://i.imgur.com/FSfOxf4.png)  

NOOT: </br> 
- De optelling van alle cosinusen => Re X[] => Reëel
- De optelling van alle sonussen => Im X[] => Imaginair

![IDK5](http://i.imgur.com/cwm6OcE.png)

### 27. Welk zijn de DFT-basisfuncties? Geef deze en benoem de verschillende delen hierin.
![IDK6](http://i.imgur.com/rLa8xYv.png)

k = Zoals hierboven verteld. Indien je frequentie domein uit 65 sampels bestaat heb je 33 verschillende cosinussen. k begint bij 0 (DC) en wordt dan verhoogd met stappen van 1 tot en met 32. 

i = Sample. Elke cosinus / sinus bestaat uit x aantal sampels! Dit is i, in het geval hierboven gaat i dus van 0 tot 64!

N = Totaal aantal sampels, in het geval hierboven dus 128!

### 28. Gegeven volgende vergelijking voor de synthesevergelijking:

In words, any N point signal, x[i], can be created by adding N/2% 1 cosine waves and N/2% 1 sine waves. The amplitudes of the cosine and sine waves are held in the arrays Im X¯[k] and Re X¯[k], respectively. The synthesis equation multiplies these amplitudes by the basis functions to create a set of scaled sine and cosine waves. Adding the scaled sine and cosine waves produces the time domain signal, x[i].

### 29. Een DFT kan bepaald worden op drie verschillende manieren. Welke zijn deze manieren en beschrijf deze beknopt.
- Simultane vergelijkingen </br>
  De optelling van sample X van alle frequentiedomeinen moet gelijk zijn aan sample X van het tijdomein. Dit geld voor elke sample, je   kunt dus een stelsel vergelijking oplossen. Dit zijn echter veel berekeningen en worden in DSP nooit gebruikt, het is wel nuttig      voor het begrijpen van DFT!
- Correlatie </br>
Om een bekende golfvorm te detecteren in een ander signaal worden beide signalen met elkaar vermenigvuldigd en worden alle punten samengevoegd in het resulterende signaal. Indien de optelling van al deze punten gelijk is aan het aantal sampels dan zijn beide signalen gelijk, indien de optelling van deze punten gelijk is aan nul komt het signaal niet voor in het andere signaal!
- FFT </br>
Fast Fourier Transform (FFT), is een slim algoritme dat een DFT met N aantal punten decomposeert in N aantal DFTs elk met maar één punt. Hierdoor zijn de berekingen veel makkelijk en sneller. Een FFT is (meer dan) 100 keer sneller dan correlatie!

### 29.2 Welke manier is het meest geschikt als de DFT minder dan 32 punten bevat?
Correlatie

### 29.3 Welke manier geniet de voorkeur als de DFT meer dan 32 punten bevat?
FFT

### 30. Gegeven een analoog sinusvormig signaal met frequentie 2 kHz, bestaande uit 800 samples met een samplefrequentie van 16 kHz.

*Gevraagd:*</br>
- Creëer de tijdsvector (sampletijdvector) hiervoor om dit signaal binnen scilab te kunnen weergeven.
- Geef weer hoe je dit signaal in scilab kan plotten

TODO

### 31. Hoe kan je een DC-component toevoegen bij een sinusvormig signaal in scilab?
```c
t = 0.0:0.01:5.0;
sin_30Hz = 1*sin(2*%pi*30*t);
sin_10Hz = 1*sin(2*%pi*10*t);
DC = 5; //DC signaal maken
signal = DC + sin_30Hz + sin_10Hz; //signaal toevoegen
plot(t, signal);
```

Uitleg: constante waarde (5) gewoon optellen met je bepaalde signalen/signaal.

### 32. Waarom heb je een antialias filter nodig om een analoog signaal te samplen?
Volgens het "Nyquist sampling theorem" moet de sample frequentie minstens twee keer de max freq van het signaal zijn. Indien dit niet het geval is, heb je last van aliasing (vervormingen door verkeerd samplen, zie module 1).

Voorbeeld bij een samplefrequenite van 100 Hz is dit maximaal 50 Hz. 

Deze filter wordt antialias filter genoemd. Dit is een laagdoorlaatfilter met dus een cut off rate van de helft van de sample frequentie. Hierdoor kunnen er geen 'verboden' frequenties is het te samplen signaal zitten!


### 33. Hoe kan je ASCII data in scilab binnenbrengen in de console via knippen en plakken (geef hiervan een voorbeeld)
```
d = [ //hier je data plakken (ctrl v)
1.1
1.2
]; //deze haak sluiten wanneer data is geplakt
```

1.1 en 1.2 zijn de geplakte waardes. d is nu een array met 2 waardes (1,1 en 1,2)!

### 34. Hoe kan je ASCII data rechtstreeks van een bestand inlezen in scilab?
```
y = read('testASCII.txt', 10, 3)
```

10, 3 => Staat voor groote matrix dat aangemaakt wordt </br>
- 10 rijen (enter)
- 3 kolomen (spatie)

**Probleem:**</br>
Tekst wordt ook beschouwd als data! Indien je dit niet wilt dat je begintekst meegenomen wordt als data (zoals bijvoorbeeld commentaar):

> fscanMat('data naam')

Tekst tussen getallen / na de getallen wordt niet verwijderd!

### 35. Hoe kan je een WAV-file inlezen in scilab?
```
[y, f, bit] = wavread('test.wav')
```

y = Variabele waar data in wordt geplaatst (sampels)
f = De sample rate van deze bepaalde .wav file
bit = Het aantal bits per sample van deze bepaalde .wav file

De variable zijn niet bedoeld om zaken in te stellen, maar om zaken te weten te komen van de .wav file! </br>
Als voorbeeld: hier kun je de sample rate niet instellen!

### 36. Hoe kan je interne variabelen opslaan en oproepen binnen SciLab?
Interne variabelen in SciLab kunnen bewaard worden in bestand! Dit is een binary file!

- Alle huidige variabelen kunnen opgeslagen worden met SAVE in het apart bestand
- Via LOAD kan je die variabele terug opgeroepen (uit het bestand halen).
- Via CLEAR kan je alle normale variabelen verwijderen! (geen effect op bestand)

Zo kun je je variable opslaan in het apart bestand en volgende sessie verder doen met die waarden!

```
save('sin_lowFreq', sin_10Hz)
clear
load('sin_lowFreq')
```

Uitleg: *sin10_Hz* is je lokale variable, je slaat deze op in de binary file onder de naam *sin_lowFreq*. Je verwijderd hierna alle lokale variabelen! Indien je nu sin_10Hz zou plotten zou dit niet gaan deze variable is verwijderd (undefined). Je kunt nu echter wel *sin_lowFreq* terug halen uit het bestand en dit plotten. Zo kun je dus verder werken!

### 37. Wat zijn de verschillen tussen offline signal processing en online signal processing?
##### Offline: 
- Alle data is reeds aanwezig en beschikbaar voor de dataprocessing
- Je hebt alle data beschikbaar vanaf het begin tot het einde van de meting.
- Tijdens het analyseren van de data heb je op ieder moment, vanaf het begin tot het einde van het proces, toegang tot alle samples die reeds in het verleden aan bod zijn geweest of nog moeten verwerkt worden.
- Met bovenstaande gegevens in acht genomen kan je dus heel verschillende algoritmen opbouwen dan bij online signal processing

##### Online:

* Een bepaalde sample wordt ingelezen en dan onmiddellijk verwerkt.
* De dataprocessing van een op dat moment beschikbare sample moet beëindigd zijn vooraleer de volgende sample wordt ingelezen.
* Het inlezen van de samples en de dataprocessing gebeurt cyclisch met constante intervallen.  Zulke intervalperioden zijn afhankelijk van het proces en kunnen seconden duren maar ook in de orde van ms of µs.
* Samples die reeds verwerkt zijn en de huidige sample zijn enkel beschikbaar voor signaalverwerking
* We kunnen niet in de toekomst kijken vermits we (nog) niet weten welke samplewaarde aanwezig zal zijn wanneer de volgende sample wordt verwerkt.

### 38. Hoe kan je een rijvector met een kolomvector samenstellen? Geef hierbij een voorbeeld.
We doen dit door gebruik te maken van de apostrof (afkappingsteken), Zie foto
![rijkolom](http://i.imgur.com/fi16lDy.png)

Uitleg: De ' vormt een rijvecotor om in een kolomvector en omgekeerd!

> rij' + kolum => resultaat = kolom </br>
> rij + kolum' => resultaat = rij

### 39. Gegeven: Een ADC levert een numerieke waarde in functie van het aantal bits. Stel een 10-bit ADC. Deze heeft een quantisatiebereik tussen 0 en 1024 (2^10 ) Stel dat de quantisatiegetallen tussen 0 en 1024 overeenstemmen met de fysieke spanning tussen 0 V en 10 V.
*Gevraagd:*</br>
Stel dat men de samples niet wil weergeven in hun DIGIT-vorm (tussen 0 en 1024), maar in de fysieke waarde van de spanning (0 V tot 10 V), hoe kan je dan deze schaling uitvoeren? Geef hiervoor voorbeeldcode.

![digToanalog](http://i.imgur.com/evnLJ5M.png)

TODO: Uitleg

### 40. Wat is de pricipiële werking van een moving average filter?
- Iedere sample in het uitgangssignaal is het resultaat van het bepalen van het gemiddelde van een aantal samples aan de ingang. 
Het neemt M samples van de input tegelijk en neemt het gemiddelde van die M-samples en levert een enkel uitgangspunt. Het 'venster' aan de ingang schijft steeds op hierdoor de term 'moving'
- Bij het bepalen van het gemiddelde (avarage) realtime (online) worden enkel de berekeningen in het verleden en de huidige berekening in rekening gebracht. Bij offline kunnen we ook de sampels in de toekomst gebruiken...

Voorbeeld:
- Venster grote van 3 sampels
- Ingang = 0 3 6 21 6 3 3 0
- Uitgang [0] = (0 + 0 + 0) / 3 = 0
- Uitgang [1] = (0 + 0 + 3) / 3 = 1
- Uitgang [2] = (0 + 3 + 6) / 3 = 3
- Uitgang [3] = (3 + 6 + 21) / 3 = 10
- Uitgang [4] = (6 + 21 + 6) / 3 = 11
- Uitgang [5] = (21 + 6 + 3) / 3 = 10
- Uitgang [6] = (6 + 3 + 3) / 3 = 12
- Uitgang [7] = (3 + 3 + 0) / 3 = 2

De filter is optimaal voor het verwijderen van rando m ruis terwijl een scherpe stap-response behouden blijft. 

Je kunt bij een moving average filter ook gebruik maken van factoren (gewichten). Hiermee kun je zeggen dat je meer prioriteit geeft aan een bepaalde sample. Zo kun je de vorige sampels meer laten doorwegen dan de huidige of andersom

> Uitgang [3] = (0.5 * 3 + 1 * 6 + 1.5 * 21) / 3 = 1.5 + 6 + 31.5 = 13

### 41. Voor de eerste berekening(en) van een moving average filter zijn er geen waarden uit het verleden. Hoe vermijd je best grote fouten tijdens het startmoment van de filter? Verklaar je antwoord.

Er zijn nog geen waarde in het verleden. We kunnen deze zien als 0 maar hierdoor zullen we een grote fout genereren. Beter is voor de onbekende dezelfde waarde te nemen als de eerste gekende waarde. Hoeveel is afhankelijk van je totale rekenkracht. Hoe meer hoe beter voor offline filters is dit geen probleem maar voor online moet dit in realtime gebeuren.

### 42. Hoe bouw je een 5-punts (gewichten/factoren) moving average filter op?

De optelling van alle gewichten moet gelijk zijn aan 5 (indien niet doen we aan versterking/verzwakking)

##### Alle gewichten gelijk:

> (X1 + X2 + X3 + X4 + X5) / 5

##### Verschillende factoren:

> (0.5 * X1 + 1 * X2 + 1 * X3 + 1 * X4 + 1.5 *  X5) / 5

### 43. Waarvoor kan je een moving average filter het best voor gebruiken? Geef ook aan waarom de moving average filter hiervoor een goede oplossing is.
Moving Average Filter is vooral geschikt voor onderdrukking van witte ruis terwijl de scherpste stapresponsie behouden blijft.

![Moving average filter](http://i.imgur.com/vShBMpr.png)

### 43.2 Wat is stapresponsie (Extra vraag door Brecht C)
TODO

### 44. Welke stappen (gebruik maken van functies) moet je doorlopen binnen scilab om een moving average filter te kunnen simuleren. Noem deze stappen/functies en verklaar beknopt hun doel.
TODO kan heel uitgebreid of heel kort

### 43. Vergelijk een FIR-filter met een IIR-filter. Wat zijn de voornaamste verschilpunten?
![firvsiir](http://i.imgur.com/0frYSwy.png)

### 44. Hoe wordt de transfertkarakteristiek van een digitaal systeem beschreven?
* Transferkarakteristiek van een analoog systeem wordt door differentiaalvergelijkingen beschreven.
* Transferkarakteristiek van een digitaal systeem wordt beschreven aan de hand van verschilvergelijkingen (difference-equations)

### 45. Geef een voorbeeld van een FIR-verschilvergelijking.
Een FIR verschilvergelijking bevat enkel de inputsignalen xk en de coëfficiënten bn.
![firvgl](http://i.imgur.com/YNcZ5mI.png)

### 46. Geef een voorbeeld van een IIR-verschilvergelijking
Een IIR verschilvergelijking bevat de inputsignalen xk en de outputsignalen yk en de coëfficiënten bn en an
![iirvgl](http://i.imgur.com/WhR5RwS.png)

### 47. Welk zijn de eigenschappen van een digitale filter en omschrijf deze beknopt.
* Afsnijfrequentie (cutoff frequency) : Frequentie waarbij de amplitude -3 dB
(0,707) lager ligt dan maximale amplitude in doorlaatband. Dit komt overeen
met nog de helft van het vermogen
* Doorlaatband (pass band) : zou vlak moeten zijn om zo weinig mogelijk
vervorming te bekomen (verschillende versterking van de frequenties die deel
uit maken van het signaal)
* Transition band (overgang tussen doorlaat en sper) Deze zou zo klein mogelijk
moeten zijn.
* Sperband (stop band) moet een zo hoog mogelijke dempings verhouding (hoge
–dB-waarden) bevatten en eveneens vlak.
* Filterorde : hoe hoger de orde, hoe beter het filter
* Filtertype : LD, HD, bandoorlaat, bandsper
* Filter design procedure : butterworth, bessel , chebycheff, …
![banden](http://i.imgur.com/ed7eE5Y.png)

### 48. Welke parameters heeft de wfir() – functie nodig om de filterparameters te kunnen bepalen van een digitale filter? Noem deze en omschrijf deze beknopt.
`[coefficients, amplitude, frequency] = wfir(filter-type, filter-order, [fg1 fg2], windowtype, [par1 par2])`
Wfir heeft volgende parameters nodig
* filter-type : lp,hp,bp,sb (laagdoorlaat hoogdoorlaat, banddoorlaat, bandsper)
* Filter-order : minimum 2 (2de orde)
* [fg1 fg2] : vector met twee afsnijfrequenties in het gebied tussen 0 en 0,5. Bij hoog- en laagdoorlaatfilter wordt enkel de eerste waarde fg1 gebruikt.
* window-type : re, tr, hm, kr, ch (rectangular, triangular, hamming, kaiser, chebyshev)
* [par1 par2] : vector met parameter voor het window-type

### 49. Wat zijn de return-waarden van de functie wfir()? Noem deze om omschrijf deze beknopt.
`[coefficients, amplitude, frequency] = wfir(filter-type, filter-order, [fg1 fg2], windowtype, [par1 par2])`
* coefficients : filtercoëfficiënten
* amplitude : vector met lengte 256 met de amplitudewaarden
* frequency : vector met lengte 256 met de frequenties in het gebied tussen 0 tot 0,5

### 50. Geef en verklaar de kenmerken (voor/nadelen) van een rectangular window om te gebruiken als window voor een window-sync digitale filter.
Rectangular window (ook boxcar of Dirichiet genoemd)
* Eenvoudigste window-equivalent om alle waarden op nul te plaatsen behalve de waarden n die doorgelaten worden.
* 𝑤 𝑛 = 1 met w(n) de windowfunctie
* Nadeel van deze filter zijn de plotselinge veranderingen tussen niet doorgelaten en wel doorgelaten waarden waardoor er ongewenste effecten in de discrete time Fourier transformatie (DTFT) ontstaan
![rectangle](http://i.imgur.com/xCSqeNE.png)

### 51. Geef en verklaar de kenmerken van een triangular window om te gebruiken als window voor een window-sync digitale filter.
Triangular Window
* Windowfunctie : ![triangle](http://i.imgur.com/42c0PlB.png?1)
* Hierin kan L gelijk zijn aan N, N+1 of N-1
* Kan gezien worden als de convolutie van twee N/2 brede rectangular windows.

![tri](http://i.imgur.com/1y386Qu.png)

### 52. Geef en verklaar de kenmerken van een hamming window om te gebruiken als window voor een window-sync digitale filter.
* Windowfunctie : ![hamming](http://i.imgur.com/oEgJPou.png?1)
* Met α = 0,54 en β= α -1 = 0,46
* Het window is zodanig geoptimaliseerd dat het maximum van de dicht bijzijnde zijlob een amplitude heeft van 1/5 van dat van het Hamming window

![hamming](http://i.imgur.com/n8kKdvV.png)

### 53. Geef en verklaar de kenmerken van een Dolph Chebyshev om te gebruiken als window voor een window-sync digitale filter.
* Minimaliseert de chebyshev norm van de zijlobes voor een gegeven hoofdlobebreedte.
* De window-functie is meestal gedefinieerd in termen van real-valued discrete Fouriertransformatie W0(k).

![dolbCheb](http://i.imgur.com/NhEOUQy.png)
