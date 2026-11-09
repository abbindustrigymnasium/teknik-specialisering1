# Kapitel 3: Sensorer och mätning

> **I det här kapitlet lär du dig**
> - hur en ultraljudssensor mäter avstånd
> - vad upplösning, noggrannhet och brus är
> - hur du filtrerar bort felaktiga mätvärden
> - hur du kalibrerar en sensor
> - varför sensorns mättakt påverkar hur fort roboten får köra

En robot vet ingenting om världen utom det som dess sensorer berättar. Och sensorer ljuger, inte för att de är dåliga utan för att fysiken är rörig. En mjuk gardin sväljer ultraljud. Solljus stör en ljussensor. Lärdomen i det här kapitlet är enkel: lita aldrig blint på ett enskilt mätvärde.

## Från fysik till tal

En sensor gör om en fysisk storhet till en elektrisk signal, och den signalen blir ett tal i programmet. En **analog** signal kan anta alla värden inom ett intervall, medan en **digital** signal bara har bestämda nivåer. För att en mikrokontroller ska kunna läsa en analog signal används en **AD-omvandlare**, som gör om spänningen till ett heltal.

En AD-omvandlare på 10 bitar ger 1024 olika värden. Med referensspänningen 3,3 V är det minsta steget 3,3 / 1024 ≈ 3,2 mV. Mindre skillnader än så kan omvandlaren inte se. Det kallas **upplösning**.

Tre begrepp blandas ofta ihop:

| Begrepp | Betydelse |
|---|---|
| **Upplösning** | minsta skillnad sensorn kan visa |
| **Noggrannhet** | hur nära det sanna värdet mätningen ligger |
| **Repeterbarhet** | hur lika mätningarna blir när du mäter samma sak flera gånger |

En sensor kan visa avstånd med en millimeters upplösning och ändå ha fel med en centimeter.

## Ultraljud: att mäta med tid

Sensorn HC-SR04 skickar ut en kort ljudpuls och mäter hur lång tid det tar för ekot att komma tillbaka. Ljudet färdas dit och tillbaka, så avståndet blir hälften av sträckan:

$$
d = \frac{v \cdot t}{2}
$$

### Genomgånget exempel

Ekot kommer tillbaka efter 2,0 ms. Ljudets fart i luft vid 20 °C är ungefär 343 m/s.

d = 343 · 0,0020 / 2 = 0,343 m

Ljudets fart beror på temperaturen. En vanlig närmeformel är v ≈ 331,3 + 0,606 · T, där T är temperaturen i °C. Vid 10 °C är farten ungefär 337 m/s, och samma tid motsvarar då 0,337 m. Skillnaden är bara 6 mm, så i ett klassrum spelar det sällan roll, men i ett kallt garage eller med höga krav på noggrannhet gör det det.

Labbarna i kursens ROS2-material räknar med `tid * 0.034 / 2` med tiden i mikrosekunder, alltså med den avrundade farten 340 m/s. Vid 1750 µs ger det 29,75 cm, medan 343 m/s skulle ge 30,01 cm. För en robot som ska stanna 30 cm från väggen är det utan betydelse. Det är ett exempel på att en förenkling kan vara helt rätt, så länge du vet att den finns.

> **Kontrollera spänningsnivåerna.** Enligt databladet arbetar HC-SR04 med 5 V, och ESP8266 är en 3,3 V-krets. Kontrollera därför i databladen för just dina komponenter, och fråga läraren, innan du kopplar sensorns Echo-utgång direkt till en ingång. En vanlig lösning är en spänningsdelare, till exempel 1 kΩ och 2 kΩ, som sänker 5 V till 5 · 2 / 3 ≈ 3,3 V.

Ultraljud har också svagheter. Mjuka och sneda ytor kan ge missade ekon, och sensorn ser i en kon och inte i en tunn linje.

## Brus och felaktiga mätvärden

Verkliga mätvärden skakar alltid lite. Det kallas **brus**. Ibland kommer också enstaka helt felaktiga värden, så kallade **spikar**. Säg att du mäter avståndet till en vägg som är 31 cm bort och får talen 31, 32, 30, 250 och 31.

| Metod | Beräkning | Resultat |
|---|---|---|
| Medelvärde | (31 + 32 + 30 + 250 + 31) / 5 | 74,8 cm |
| Median (mittenvärdet i storleksordning) | 30, 31, **31**, 32, 250 | 31 cm |

Medelvärdet förstörs av spiken. Medianen påverkas inte alls. Därför är medianfilter vanligt på sensorer med spikar.

Ett annat vanligt filter är det **exponentiella medelvärdet**, där nästa värde blir y = α · x + (1 − α) · y_förra. Med α = 0,3 och startvärdet 31 ger mätningarna 32, 250 och 31 resultaten 31,3, 96,9 och 77,1. Filtret jämnar ut brus men dras med av spikar, och det kommer alltid lite efter den sanna signalen.

```python
from collections import deque
from statistics import median

class MedianFilter:
    def __init__(self, storlek=3):
        self.varden = deque(maxlen=storlek)

    def uppdatera(self, varde):
        self.varden.append(varde)
        return median(self.varden)

f = MedianFilter(3)
for matning in [31, 32, 250, 31, 30]:
    print(matning, "->", f.uppdatera(matning))
```

Vad händer i körningen? När värdet 250 kommer in är medianen av 31, 32 och 250 lika med 32, alltså bortfiltrerat. Priset är att filtret reagerar något senare på verkliga förändringar. Ju större filter, desto mer fördröjning. Allt filtrerande är en avvägning.

## Kalibrering

Att **kalibrera** en sensor är att jämföra den med något känt och justera. Ett enkelt exempel är en ljussensor som ska skilja ett vitt papper från ett pennstreck.

1. Mät värdet över rent papper, till exempel 780.
2. Mät värdet över pennstrecket, till exempel 410.
3. Sätt tröskeln mitt emellan: (780 + 410) / 2 = 595.

Värdena beror på ljuset i rummet och på hur högt sensorn sitter. Därför måste du kalibrera på samma plats och med samma montering som du tänker köra. Och du ska aldrig gissa en tröskel som går att mäta.

## Mättakt, fart och säkerhetsmarginal

En sensor mäter inte kontinuerligt. Om den mäter 10 gånger per sekund (10 Hz) går det 100 ms mellan mätningarna. En bil som kör 0,36 m/s hinner då 0,36 · 0,1 = 3,6 cm mellan varje mätning. Lägger du till fördröjningar i nätverket växer sträckan ytterligare, vilket du räknar på i [kapitel 7](07-uppkopplade-system.md).

Sambandet är viktigt att komma ihåg: ju snabbare roboten kör, desto oftare behöver den mäta och desto längre stoppavstånd behövs.

## Begrepp

- **AD-omvandlare:** krets som gör om en analog spänning till ett heltal.
- **Upplösning:** minsta skillnad en sensor kan visa.
- **Noggrannhet:** hur nära det sanna värdet en mätning ligger.
- **Repeterbarhet:** hur lika upprepade mätningar blir.
- **Brus:** slumpmässig variation i mätvärden.
- **Spik:** enstaka kraftigt felaktigt mätvärde.
- **Medianfilter:** filter som använder mittenvärdet och tar bort spikar.
- **Kalibrering:** jämförelse med något känt och justering utifrån det.
- **Mättakt:** hur ofta sensorn mäter, anges i hertz.

## Testa dig själv

1. Varför delas den uppmätta sträckan med två i formeln för ultraljud?
2. Vad är skillnaden mellan noggrannhet och upplösning?
3. Varför klarar medianen en spik bättre än medelvärdet?
4. Varför kommer ett större medianfilter att reagera långsammare?
5. Varför ska en ljussensor kalibreras på samma plats som den används?
6. Nämn två situationer där en ultraljudssensor kan ge fel eller inget eko alls.

## Övningar

**Grund.** Ekot från en HC-SR04 kommer tillbaka efter 5800 µs. Beräkna avståndet med ljudfarten 343 m/s.

<details><summary>Svar</summary>

d = 343 · 0,0058 / 2 ≈ 0,995 m.

</details>

**Vidare.** Tre mätningar blir 18, 19 och 300 cm. Beräkna median och medelvärde, och skriv vilket värde du skulle låta roboten använda.

<details><summary>Svar</summary>

Medianen är 19 cm. Medelvärdet är (18 + 19 + 300) / 3 ≈ 112,3 cm. Medianen är det rimliga värdet, eftersom 300 är en spik.

</details>

**Utmaning.** En bil kör 0,30 m/s och sensorn mäter 10 gånger per sekund. Kommandon försenas 200 ms innan de når bilen. Hur långt hinner bilen köra från att ett hinder dyker upp tills stoppkommandot kommit fram, i det sämsta fallet? Bromssträckan räknas inte.

<details><summary>Svar</summary>

Högst 0,10 s mellan mätningarna ger 0,30 · 0,10 = 3 cm. Fördröjningen ger 0,30 · 0,20 = 6 cm. Totalt ungefär 9 cm.

</details>

## Koppling till projektet

I [Robotmopp](../projekt/robotmopp/README.md) använder du ultraljudssensorn för hinderstopp och en ljussensor för nivå 3, och där behövs både filter och kalibrering. Sensorförståelsen är också grund för [Robot: Humanoiden](../projekt/humanoiden/README.md) och [Robotik Hunden](../projekt/hunden/README.md).

## Läs mer

- [Datablad för HC-SR04 (PDF)](https://cdn.sparkfun.com/datasheets/Sensors/Proximity/HCSR04.pdf)
- [Datablad för ESP8266 (PDF)](https://www.espressif.com/sites/default/files/documentation/0a-esp8266ex_datasheet_en.pdf)

[← Föregående](02-robotens-kropp.md) | [Innehåll](../README.md) | [Nästa →](04-sakerhet-och-ctf.md)
