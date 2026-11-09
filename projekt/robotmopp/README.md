# Projekt: Robotmopp

## Syfte

Du bygger en robotmopp, en vidareutveckling av robotbilen som kallas Driverbot 2.0. Bilen har en ESP8266 och styrs från ROS2 på datorn via en brygga över MQTT. Med en whiteboardpenna under sig ska den täcka en arena på 2 × 2 m. Arenan delas in i 8 × 8 rutor på 25 cm, så att du kan mäta hur stor del av ytan bilen har städat.

Projektet samlar kursens innehåll: du planerar, konstruerar, simulerar, testar på riktig hårdvara och utvärderar med mätdata.

## Koppling till kursen

**Kriterier i vanliga ord**

| Kriterium | Vad det betyder här |
|---|---|
| Begrepp, teorier, metoder och verktyg | du använder ROS2 och verktygen på ett sätt som visar att du förstår dem |
| Utveckla tekniska lösningar | du arbetar systematiskt från plan till CAD och färdig bil, och testar stegvis |
| Dokumentera, presentera och utvärdera | du visar resultat med mätvärden och drar slutsatser |

**Centralt innehåll:** begrepp och teorier, metoder och verktyg för att skapa, simulera och testa, strategier för problemlösning, kvalitetssäkring samt dokumentation, presentation och utvärdering.

**Kapitel som förbereder**

| Kapitel | Används till |
|---|---|
| [Kapitel 1](../../kapitel/01-smarta-system.md) | val av strategi och källkritik vid komponentval |
| [Kapitel 3](../../kapitel/03-sensorer.md) | ultraljud, ljussensor, filter och kalibrering |
| [Kapitel 5](../../kapitel/05-ros2.md) och [kapitel 6](../../kapitel/06-simulering-och-tillstand.md) | ROS2, tillståndsmaskin och täckningsmätning |
| [Kapitel 7](../../kapitel/07-uppkopplade-system.md) | MQTT, bryggan och dödmansgreppet |
| [Kapitel 8](../../kapitel/08-reglering.md) | mjuk inbromsning |
| [Kapitel 9](../../kapitel/09-kvalitet-och-dokumentation.md) | mätprotokoll, krav, dokumentation och presentation |

## Vad du ska göra

- Gör en projektplan med komponentlista, bland annat ultraljudssensor HC-SR04 och ljussensor.
- Cadda ett chassi med fästen för penna och sensorer, och sätt ihop bilen.
- Testa städprogrammet i simulering ([labb 8](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-8-robotmopp-simulering.md)), koppla bilen till ROS2 ([labb 9](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-9-koppla-bilen.md)) och kör autonomt ([labb 10](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-10-autonom-bil.md)).

Du använder den MQTT-broker som läraren anger, även om labbarna nämner andra adresser som exempel. Eventuella inloggningar får du av läraren, och de ska aldrig finnas i kod som du lägger ut.

### Nivåer

| Nivå | Mål |
|---|---|
| **Nivå 1** | Styr bilen manuellt från ROS2 och berör alla 64 rutor. |
| **Nivå 2** | Hinderstopp och autonom slumpstädning med mätt täckning. |
| **Nivå 3** | Spiral eller "undvik ritat" med ljussensorn, jämfört med slump. |

## Krav och leverabler

- Projektplan med komponentlista.
- CAD-modell av chassit, med fästen för penna och sensorer.
- Färdig, sammansatt bil.
- Städprogram testat i simulering och på bilen.
- Mätresultat för täckningen enligt protokollet i labb 10.
- Dokumentation av lösning och resultat, samt demo på robotbanan.

Exakt vad som lämnas in anger labbarna och läraren.

## Arbetsgång

| Lektion | Innehåll |
|---|---|
| 1 | Projektstart: genomgång av Driverbot 2.0 och robotmoppen (arena, rutnät, nivå 1–3). Bli klar med turtlesim-labbarna till och med labb 7. |
| 2 | Projektplan: gör komponentlista (ESP8266, motor, servo, HC-SR04, ljussensor, penna) och skissa chassit. |
| 3 | CAD: börja modellera chassit med plats för motor, servo och ESP8266. |
| 4 | CAD fortsättning: fästen för whiteboardpenna, ultraljudssensor och ljussensor. Kontrollera måtten mot komponentlistan. |
| 5 | Labb 8 del 1–2: täckningsmätaren (8 × 8 rutnät) och manuell städning med teleop i turtlesim. |
| 6 | Labb 8 del 3–4: autonom städare med tillståndsmaskinen och launch-fil. Jämför manuell och autonom täckning. |
| 7 | Labb 9 del 1–2: ladda upp ny firmware på ESP8266 och testa bilen via MQTT utan ROS2. |
| 8 | Labb 9 del 3–4: bryggnoden mellan ROS2 och MQTT. Kör bilen med teleop och läs av ultraljudssensorn i ROS2. |
| 9 | Sätt ihop robotmoppen: montera chassit, pennan och sensorerna på bilen och testkör. |
| 10 | Nivå 1 på arenan: kalibrera bilen, styr manuellt från ROS2 och försök beröra alla 64 rutor. Börja på hinderstoppet i labb 10. |
| 11 | Labb 10: hinderstopp, hinder-undvikare och autonom slumpstädning på arenan. Mät täckningen i rutnätet (nivå 2). |
| 12 | Nivå 3: spiral eller "undvik ritat" med ljussensorn, jämför med slump. Trimma och dokumentera resultaten inför demon. |
| 13 | Läraren anger innehållet. |

Projektet avslutas med demo på robotbanan.

## Bedömning

Du bedöms på

- hur väl du använder ROS2 och verktygen,
- hur systematiskt du utvecklar och testar lösningen,
- hur du följer konstruktionsprocessen från plan till CAD och färdig bil,
- hur du dokumenterar, presenterar och utvärderar resultatet.

## Material

- [ROS2-repot med alla labbar](https://github.com/abbjetmus/ROS2), där också presentationen om Driverbot 2.0 ligger
- [Labb 8: Robotmoppen i simulering](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-8-robotmopp-simulering.md)
- [Labb 9: Koppla bilen](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-9-koppla-bilen.md)
- [Labb 10: Autonom bil](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-10-autonom-bil.md)

## Tips

- Bli klar med labb 1–7 först. Allt i projektet bygger på dem.
- Ha hjulen i luften första gången ett nytt kommando testas.
- Ändra en sak i taget och skriv upp varje mätning. Då går resultaten att jämföra.
- Börja med låg fart. Stoppavståndet behöver räknas på din bils verkliga fart och fördröjning.
- Se till att bilen stannar när styrsignalen försvinner, och testa det medvetet.
- Kontrollera spänningsnivåerna mellan HC-SR04 och ESP8266 med hjälp av datablad och läraren innan du kopplar in sensorn.

[Innehåll](../../README.md)
