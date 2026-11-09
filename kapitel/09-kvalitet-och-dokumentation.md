# Kapitel 9: Kvalitetssäkring, dokumentation och presentation

> **I det här kapitlet lär du dig**
> - hur du skriver krav som går att testa
> - hur du planerar tester på olika nivåer
> - hur du jämför resultat så att slutsatsen håller
> - hur du hittar risker innan de blir problem
> - hur du dokumenterar och presenterar en teknisk lösning

Föreställ dig att din robot klarade demonstrationen på fredagen, men ingen, inte ens du, kan förklara varför den fungerade den gången och inte dagen innan. Då är det ett lyckokast och inte en teknisk lösning. Det här kapitlet handlar om hur du vet att något fungerar och hur du visar det för andra.

## Krav som går att testa

Ett **krav** ska beskriva vad en lösning ska klara och gå att kontrollera.

| Dåligt krav | Bättre krav |
|---|---|
| Roboten ska vara snabb. | Roboten ska täcka minst 80 % av arenan på 3 minuter. |
| Roboten ska vara säker. | Motorn ska stanna inom 1 sekund om styrsignalen försvinner. |
| Koden ska vara bra. | Alla inställningar ska kunna ändras som parametrar. |

Två ord är bra att skilja på. **Verifiering** svarar på frågan: byggde vi det vi sa? **Validering** svarar på frågan: är det vi byggde det som behövdes?

## Test på olika nivåer

| Nivå | Vad som testas | Exempel |
|---|---|---|
| **Enhetstest** | en funktion för sig | att `ruta(10.9)` blir 7 |
| **Integrationstest** | delar tillsammans | att ett `Twist` blir rätt MQTT-meddelande |
| **Systemtest** | hela lösningen | att bilen klarar en körning på arenan |
| **Regressionstest** | gamla funktioner efter en ändring | att hinderstoppet fortfarande fungerar |

Börja nedifrån. Ett fel som hittas i ett enhetstest tar en minut att rätta. Samma fel, hittat på golvet, kan ta en lektion. Ett **testprotokoll** bör innehålla vad som testades, hur, med vilken version av koden, vad du väntade dig och vad som hände.

## Genomgånget exempel: jämföra två versioner

Du har mätt tre körningar av en städrobot enligt samma protokoll, där täckningen är antal rutor av 64.

| Körning | Rutor | Täckning |
|---|---|---|
| 1 | 41 | 64,1 % |
| 2 | 47 | 73,4 % |
| 3 | 44 | 68,8 % |

- Medelvärdet är (41 + 47 + 44) / 3 = 44 rutor, vilket är 44 / 64 = 68,75 %.
- Variationsbredden är 47 − 41 = 6 rutor.
- Standardavvikelsen, ett mått på spridningen, är 3 rutor.

Sammanfattningen av tre körningar säger mycket mer än den bästa körningen. Om nästa version får 46 rutor i medel är skillnaden mindre än spridningen, och då kan du inte säga att versionen är bättre. Tre slutsatser att ta med sig:

1. Ändra **en sak i taget**, annars vet du inte vad som orsakade förändringen.
2. Gör **flera körningar** och redovisa medelvärde och spridning.
3. **Samma protokoll** varje gång: samma start, samma tidsgräns, samma räkning.

## Riskanalys med FMEA

**FMEA** (feleffektanalys) är en metod från industrin där du går igenom vad som kan gå fel innan det har hänt. För varje felsätt bedömer du allvarlighet (A), sannolikhet (S) och hur svårt felet är att upptäcka (U), oftast på en skala från 1 till 10. Produkten kallas riskprioritetstal.

| Felsätt | Effekt | A | S | U | Risktal |
|---|---|---|---|---|---|
| WiFi tappas under körning | bilen kör vidare | 4 | 3 | 5 | 60 |
| Sensor ger spikvärden | bilen backar i onödan | 3 | 4 | 3 | 36 |
| Motor överhettas | motor skadas | 8 | 2 | 2 | 32 |

Risktalet 60 blev högst, och därför läggs åtgärder där först, i det här fallet dödmansgreppet. Skalorna är bedömningar, så diskutera dem i gruppen. Inom industrin används ofta standarder för kvalitetsledning som ISO 9001, och den grundläggande idén är densamma: definiera vad som ska uppnås, gör, kontrollera och förbättra.

## Dokumentation

Dokumentation är inte något du gör sist. Den är ditt minne under projektet och det som gör att andra kan använda och bygga vidare på din lösning.

| Del | Innehåll |
|---|---|
| **README** | vad projektet är, hur det startas, hur det används |
| **Arkitekturdiagram** | vilka delar som finns och hur de kommunicerar |
| **Komponentlista** | delar, antal, datablad, källa |
| **Kod och kommentarer** | varför något är gjort, inte bara vad |
| **Testprotokoll** | vad du testade och resultatet |
| **Logg** | beslut, problem och lösningar |

**Versionshantering** med Git gör att du kan gå tillbaka om något går sönder och se vad som ändrades. Skriv korta, tydliga commit-meddelanden och lägg aldrig in lösenord eller andra hemligheter.

## Presentera

En teknisk presentation ska svara på några få frågor: vad var problemet, vad byggde du, hur testade du och vad blev resultatet?

1. **Börja med problemet**, inte med tekniken.
2. **Visa, berätta inte bara.** En kort demo eller ett diagram säger mer än text.
3. **Visa siffror** från tester, inte bara känslor.
4. **Var ärlig.** Berätta vad som inte fungerade och vad du skulle ändra.
5. **Öva med tidtagning**, och ha en inspelad film om det är risk att demon krånglar.

Demonstrationer går ibland fel. Säg vad som händer, vad du tror att det beror på och fortsätt. Det är också teknisk kompetens.

## Utvärdera

En **utvärdering** jämför resultatet med kraven. Gå igenom kraven ett och ett och skriv om de uppfylldes, och vad dina mätningar visar. Skriv sedan vad du skulle göra annorlunda nästa gång. Goda utvärderingar är konkreta: "stoppavståndet var för kort vid högsta farten eftersom fördröjningen var större än vi antagit" är bättre än "det gick ganska bra".

## Begrepp

- **Krav:** beskrivning av vad en lösning ska klara, utformad så att den går att kontrollera.
- **Verifiering:** kontroll att lösningen är byggd enligt kraven.
- **Validering:** kontroll att lösningen löser det verkliga behovet.
- **Testprotokoll:** dokument som beskriver vad som testats och resultatet.
- **Regressionstest:** test som visar att gammalt fortfarande fungerar efter en ändring.
- **Standardavvikelse:** mått på hur mycket mätvärden sprider sig.
- **FMEA:** systematisk genomgång av hur något kan gå fel och vad det får för följder.
- **Utvärdering:** jämförelse mellan resultat och krav.

## Koppling till projektet

Metoderna i kapitlet används i [Robotmopp](../projekt/robotmopp/README.md), där du följer ett mätprotokoll och redovisar resultat. Dokumentation och presentation gäller alla projekt, även dem där läraren fastställer upplägget senare.

## Läs mer

- [Pro Git på svenska](https://git-scm.com/book/sv/v2)

[← Föregående](08-reglering.md) | [Innehåll](../README.md) | [Nästa →](10-hallbarhet-och-livscykel.md)
