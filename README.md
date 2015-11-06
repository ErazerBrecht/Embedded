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

