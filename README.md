# Embedded 5

Deze README bevat mijn antwoorden op de vragenlijst van de te kennen leerstof voorhet examen van Embedded.
De 1ste 15 vragen zijn voor na de herfstvakantie, de rest volgt later.

#### 0. Standaardbegrippen (Extra vraaag van Brecht C)
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

Het gemiddelde hiervan is 2052. Het verschil tussen 2050 n 2052 noemen we dus de statische ruis. Indien je nu de standaardeviatie berekent en je gebruikt als gemiddelde 2052 (het gemiddelde van het signaal) is dit eigenlijk fout van het feitelijke gemiddelde is 2050.Dit is ook de reden dat we door N - 1 delen om de standarddeviatie te berekenen i.p.v. door N.
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
TODO

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

Ben ik nog niet 100% zeker, suggesties mogen altijd.

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

