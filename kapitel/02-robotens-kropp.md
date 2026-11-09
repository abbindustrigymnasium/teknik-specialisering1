# Kapitel 2: Robotens kropp

> **I det här kapitlet lär du dig**
> - hur frihetsgrader beskriver vad en robot kan röra
> - hur motorer, servon och växlar skiljer sig åt
> - hur du räknar ut om ett servo orkar lyfta en arm
> - hur du beräknar var handen hamnar när du känner vinklarna
> - varför robotar med ben kan välta och hur man förhindrar det

Att gå upp för en trappa känns enkelt för dig. För en robot är det ett av de svåraste problemen som finns. Varje led måste ha rätt vinkel i rätt ögonblick, och tyngdpunkten får inte hamna fel. Innan du kan programmera en robot som rör sig behöver du förstå den mekaniska kroppen.

## Frihetsgrader

En **frihetsgrad** är ett oberoende sätt som något kan röra sig på. En dörr har en frihetsgrad, för den kan bara svänga kring gångjärnet. Din axel har tre, eftersom armen kan vridas på tre sätt.

| Robot | Typiskt rörelsesätt | Vad det kräver |
|---|---|---|
| Bil med styrservo | hjul, kan inte svänga på stället | motor och styrvinkel samordnas |
| Fyrbent robot | fyra ben med flera leder | samordning av många leder, balans |
| Humanoid | två ben, armar, ofta huvud | många leder och dynamisk balans |

Antalet frihetsgrader är ofta det som avgör hur svårt en robot är att styra. Varje extra led ger fler möjligheter men också fler saker som kan gå fel, mer vikt och större strömförbrukning.

## Ställdon

Ett **ställdon** är det som faktiskt får roboten att röra sig.

| Typ | Styrs med | Bra för | Svaghet |
|---|---|---|---|
| **DC-motor** | spänning och PWM | hjul, snabb rotation | vet inte sin egen position |
| **Servo** | pulsbredd, ger en vinkel | leder och styrning | begränsad rörelse, begränsat moment |
| **Stegmotor** | pulser i ordning | exakta steg i maskiner | kan tappa steg under last |

**PWM** (pulsbreddsmodulering) betyder att spänningen slås av och på snabbt. Andelen tid som spänningen är på avgör medeleffekten, och det används för att styra farten på en motor.

Ett hobbyservo styrs ofta med en puls som upprepas ungefär 50 gånger per sekund. Pulsens längd anger önskad vinkel. Exakt vilket intervall som gäller varierar mellan modeller, så kontrollera alltid databladet.

Anta att ett servo ger 0° vid 1000 µs och 180° vid 2000 µs. Vinkeln 120° motsvarar då:

$$
1000 + \frac{120}{180} \cdot 1000 \approx 1667\ \mu\text{s}
$$

### Växlar

En motor snurrar oftast snabbt med litet moment. En **växel** byter fart mot kraft. Med utväxlingen 48:1 blir utgående varvtal en fyrtioåttondel, medan momentet blir ungefär 48 gånger större, minus förluster. En motor med 6000 varv per minut och momentet 0,02 Nm ger med verkningsgraden 70 %:

| Storhet | Beräkning | Resultat |
|---|---|---|
| Varvtal ut | 6000 / 48 | 125 varv per minut |
| Moment ut | 0,02 · 48 · 0,70 | cirka 0,67 Nm |

## Genomgånget exempel: orkar servot lyfta armen?

En robotarm håller en last. Det **moment** som servot måste klara är kraften gånger hävarmen, M = F · r.

Lasten väger 0,25 kg och sitter 0,15 m från servots axel. Tyngdaccelerationen g är 9,82 m/s².

1. Kraften: F = 0,25 · 9,82 ≈ 2,46 N
2. Momentet: M = 2,46 · 0,15 ≈ 0,37 Nm
3. Datablad för servon anger ofta moment i kg·cm. 1 kg·cm motsvarar 0,0982 Nm, vilket ger 0,368 / 0,0982 ≈ 3,75 kg·cm.
4. Använd en **säkerhetsfaktor**, här 2, eftersom rörelser kräver extra moment när armen accelererar och eftersom servot inte ska jobba på gränsen. Du behöver ett servo på minst 7,5 kg·cm.

Glöm inte att armens egen vikt också ger moment. I ett riktigt projekt räknar du fram momentet från varje del och lägger ihop.

## Genomgånget exempel: var hamnar handen?

Att räkna ut en lems position från ledernas vinklar kallas **framåtkinematik**. Tänk dig en arm med två leder i ett plan. Överarmen är 0,10 m och underarmen 0,08 m. Axelvinkeln θ₁ är 30° och armbågsvinkeln θ₂ är 45°, räknad från överarmens förlängning.

```text
x = l1·cos(θ1) + l2·cos(θ1 + θ2)
y = l1·sin(θ1) + l2·sin(θ1 + θ2)
```

```python
import math

def hand(l1, l2, theta1_grader, theta2_grader):
    t1 = math.radians(theta1_grader)
    t2 = math.radians(theta2_grader)
    x = l1 * math.cos(t1) + l2 * math.cos(t1 + t2)
    y = l1 * math.sin(t1) + l2 * math.sin(t1 + t2)
    return round(x, 4), round(y, 4)

print(hand(0.10, 0.08, 30, 45))   # (0.1073, 0.1273)
```

Handen hamnar alltså ungefär 10,7 cm framåt och 12,7 cm upp från axeln. Det omvända problemet, att hitta vinklarna när du vet var handen ska vara, kallas **inverskinematik**. Det är svårare, eftersom det ofta finns flera lösningar eller ingen alls.

## Balans och tyngdpunkt

En robot står stilla utan att välta så länge **tyngdpunkten** rakt ovanifrån ligger inom **stödytan**, det område som begränsas av kontaktpunkterna mot golvet. Det kallas **statisk balans**.

En humanoid har liten stödyta, särskilt när den står på en fot. En fyrbent robot kan alltid ha tre fötter i golvet och ändå ha en stödyta att stå på, vilket gör den lättare att hålla stabil vid långsam gång. När en robot rör sig snabbt räcker inte statisk balans. Då måste den ständigt fånga upp sig själv, ungefär som du gör när du springer. Det kallas **dynamisk balans**.

**Räkneexempel.** En robots tyngdpunkt sitter 0,40 m över golvet och foten sträcker sig 0,04 m åt sidan från tyngdpunktens lodlinje. Om roboten lutar 5° flyttas tyngdpunkten i sidled ungefär 0,40 · tan 5° ≈ 0,035 m, vilket är innanför kanten. Roboten börjar välta först vid den vinkel där tan α = 0,04 / 0,40, alltså ungefär 5,7°. Marginalen är liten, och därför behöver robotar med ben snabb återkoppling.

## Säkerhet runt robotar som rör sig

Rörliga robotar kan klämma fingrar och slå i föremål. Prova rörelser med låg fart och med roboten upplyft när det går. Ha alltid ett sätt att stoppa roboten direkt, och stäng av strömmen när du arbetar med mekaniken. Batterier ska hanteras enligt tillverkarens anvisningar.

## Begrepp

- **Frihetsgrad:** oberoende sätt som något kan röra sig på.
- **PWM:** snabb av- och påslagning av spänning som styr medeleffekten.
- **Servo:** ställdon med inbyggd återkoppling som ställer in en vinkel.
- **Moment:** kraft gånger hävarm, det som vrider en led.
- **Utväxling:** förhållande som byter varvtal mot moment.
- **Säkerhetsfaktor:** marginal mellan beräknad belastning och det komponenten klarar.
- **Framåtkinematik:** beräkning av position utifrån ledvinklar.
- **Inverskinematik:** beräkning av ledvinklar utifrån önskad position.
- **Stödyta:** området mellan robotens kontaktpunkter mot golvet.
- **Statisk och dynamisk balans:** stabil när den står stilla, respektive stabil genom ständig rörelse och korrigering.

## Testa dig själv

1. Hur många frihetsgrader har en vanlig bil med styrning på ett plant golv? Motivera.
2. Varför behöver en DC-motor extra hjälp för att veta var den är?
3. Vad händer med momentet och farten när utväxlingen ökar?
4. Varför används en säkerhetsfaktor vid val av servo?
5. Vad är skillnaden mellan framåtkinematik och inverskinematik?
6. Varför är statisk balans lättare med fyra ben än med två?

## Koppling till projektet

Kapitlet förbereder [Robot: Humanoiden](../projekt/humanoiden/README.md) och [Robotik Hunden](../projekt/hunden/README.md), där mekanik, balans och val av ställdon spelar stor roll. Läraren fastställer upplägget i de projekten. Beräkningarna av moment och kraft kommer också till nytta när du bygger chassit i [Robotmopp](../projekt/robotmopp/README.md).

## Läs mer

- [Arduino: biblioteket Servo](https://www.arduino.cc/reference/en/libraries/servo/)

[← Föregående](01-smarta-system.md) | [Innehåll](../README.md) | [Nästa →](03-sensorer.md)
