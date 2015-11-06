# Embedded 5

Deze README bevat mijn antwoorden op de vragenlijst van de te kennen leerstof voorhet examen van Embedded.
De 1ste 15 vragen zijn voor na de herfstvakantie, de rest volgt later.

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

Waarom is hier een verschil in: 
Een pdf van 0.03 bij een amplitude van 120,5 betekent niet dat een spanning van 120,5mV gedurende 3% van de tijd voorkomt in het signaal. Integendeel de kans dat het signaal exact 120,500000000000mV is, is bijzonder klein.
In werkelijkheid: Aangezien er oneindig mogelijke waarden (en schommelingen) zijn. Dit zorgt ervoor dat bij 120.5 mV ook 120,4997, 120,4998, 120,4999, …. Vallen

Hoe kun je dan den eigenlijke kans berekenen?
Stel dus dat bij 121 een kansdichtheid hoort van 0.03... </br>
Dan is de kans dat het signaal tussen 120 en 121 ligt => (121 - 120) * 0.03 = 0.03 => 3%
De kans dat het signaal tussen 120.5 en 120.4 ligt => (120.5 - 120.4) * 0.03 = 0.003 = 0.3%
NOOT: Dit is het geval indien de kansdichtheid constant blijft tussen de twee getallen, indien dit niet het geval is moet je de oppervlakte onder de grafiek berekenen (aka Integraal, maar dit ligt buiten de scope van dit vak!)
