# Embedded 5

Deze README bevat mijn antwoorden op de vragenlijst van de te kennen leerstof voorhet examen van Embedded.
De 1ste 15 vragen zijn voor na de herfstvakantie, de rest volgt later.

#### 0. Standaardbegrippen (Extra vraaag by Brecht C)
TODO: Standaarddeviatie, gemiddelde, gauss curve en SNR / CV

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

### 3.1 Wat is het verschil tussen een signaal en het onderliggend proces? (Extra vraag by Brecht C)
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

Nu is de afwijing niet altijd  maximaal en volgens de statistiek is de variatie (standaard deviatie) dat er naar boven of naar beneden afgerond wordt dezelfde als een random getal tussen 0 en 1. In dat geval is de standaarddeviatie 0,29 (Hoe die cursus aan dat getal komt, ik heb godverdomme geen idee).

Dus de ruis wordt dan **0.29 LSB** (leert er met leven, geen vragen stellen Brecht, Let It Goooooo) => 0,29 . 1,22mV = 0,3538 mV.

Owkay we zijn er bijna, nu hebben we ook nog die extra ruis op het signaal van 2mV piek. 
Ik ga er even van uit gaan dat hij 2mV RMS bedoeld :) </br>
LSB was 1,22mV => 2mV = 1,64 LSB. Om dit nu te bereken gebruik je Pythagoras (oorspronkelijke ruis => 0,29LSB en nieuwe ruis uitgedrukt in LSB), dus √(0,29² + 1,64²) = 1,665 LSB * 1,22mV = **2,0313mV**

### 4.1 Dither (Extra vraaag by Brecht C)
TODO

### 4.2 Sampling theorema + Aliasing (Extra vraaag by Brecht C)
Indien je een signaal correct wilt sampelen, mag de frequentie van het signaal niet groter worden dan de helft van de sampel frequentie! Dit noemt het **Nyquist theorema**.

Indien je dit wel doet zullen de sample waardes niet het juiste signaal voorstellen. Het signaal die je dan verkrijgd noemen we een **alias** (vermomming). 

![Nyquist theorema](http://i.imgur.com/Z8T7yiv.png)

Het signaal op de linkse ADC heeft een frequentie van 0,31 keer de sample frequentie, volgens het Nyquist theorema moet deze correct gesampeld worden (0,31 < 0,50). Je kunt zien dat er maar één bepaalde sinus is die voldoet aan de sample punten, je kunt hier dus het correct signaal uithalen.

Het rechtste signaal heeft een frequentie van 0,95 keer de sample frequentie, deze zal volgens het Nyquist theorema niet correct gesampeld worde (0.95 !< 0,50). Je kunt uit de sample punten de sinus halen van het ingangssignaal. Maar je ziet duidelijk dat er nog een andere sinus ook voldoet aan de sample punten (een alias). Dit signaal was niet aanwezig in ons ingangsignaal en is daarom onwenselijk!

### 5. Hoeveel ruisvermogen levert een 8-bit ADC toe aan het signaal (na kwantisatie) als deze een bereik heeft tussen -5V en + 5V?
8 bit => 0 tot 255 </br>
0 = -5V</br>
255 = 5V</br>
Delta = 10V</br>

10V / 255 => 0,0392V => 39,216mV        (Kleinste stap die mogelijk is!!!) </br>
Maximum ruis => 0,5 LSB => 39,216 * 0,5 = 19,608 mV
 
Ruis => 0,29 LSB => 11,3725mV </br>
**Ruisvermogen => σ² => 0,29² => 0,0841W**

####Ben niet zeker, ik heb een mail gestuurd naar Patje


### 6. Leg het principe (algoritme) uit hoe je een digitale ruisgeneratie kan bekomen die een Gauscurve benadert. Geef ook aan voor welke toepassing(en) je deze digitale ruisgeneratie kan gebruiken
Digtitale ruisgeneratie kan gegenereerd door een **random number generator**.

Indien je een random getal laat genereren door je computer tussen nul en één is de standaarddeviatie 0,29 (1/√12), en het gemiddelde 0.5.
Indien je dit getal nu optelt met een nieuw random getal tussen nul en één, krijg je een standaarddeviatie van √(0.29² + 0.29²) = 0.41 (1/√6) en een gemiddelde van 1 </br>
Indien je dit in het totaal 12 keer doet zul je dus een standard deviatie van 1 hebben en een gemiddelde van 6.
Een gauscurve heeft een standard deviatie van 1 (zie vraag 0). 

TODO: Foto C# programma

We moeten random ruis kunnen genereren om zo verschillende aspecten van ons algoritme te kunnen testen in het bijzijn van ruis. Bijvoorbeeld: Ruis heeft effect op hoever een radio kan communiceren, hoeveel radiatie er nodig is om een X-ray foto te kunnen maken.

### 7. Oefening convolutie
Gegeven: volgend ingangssignaal: **x[0] = 4, x[1]=-2,x[2] = 8, x[3] = -1, x[4] = -1, x[5] = -2.** </br>
Het functiesignaal h[n] bestaat uit de samples: **h[0] = -1, h[1] = 1, h[2] = -0,5, h[3]= +0,5.** </br>
Gevraagd: bepaal **y[n]** </br>

TODO: Probleem voor morgen

### 8. Verklaar met eigen woorden beknopt het begrip discrete afgeleide bij digitaal signaal verwerking. Leg ook uit hoe deze kan worden berekend (formule). Pas discrete afgeleide toe op een voorbeeld.
TODO

### 9. Verklaar met eigen woorden beknopt het begrip discrete integratie bij digitaal signaal verwerking. Leg ook uit hoe deze kan worden berekend (formule). Pas discrete integratie toe op onderstaand voorbeeld:
TODO

### 10.1 Wat is een linear systeem, geef enkele voorbeelden en eigenschappen (Extra vraag by Brecht)
TODO

### 10.2 Verklaar wat decompositie en synthese is, waarom is het belangrijk? (Extra vraag by Brecht)
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

**Voorbeeld met grafieken:**

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




