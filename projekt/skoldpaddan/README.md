> **Under planering.** Läraren fastställer upplägg och krav för det här projektet.

# Projekt: Robotik: Sköldpaddan

## Syfte

Projektet använder simulatorn turtlesim i ROS2, där en sköldpadda styrs på skärmen. Enligt planeringen av projektet [Robotmopp](../robotmopp/README.md) ska du vara klar med turtlesim-labbarna till och med labb 7 där, och robotmoppen använder simuleringen från det här projektet. Upplägget i övrigt fastställer läraren.

## Koppling till kursen

Läraren fastställer vilka kriterier projektet bedöms mot. Kursens kriterier står i [kursplanen](../../kursplan.md). Projektet delas med en annan kurs. Här ligger tyngdpunkten på smarta system: begrepp och verktyg i ROS2, och att skapa och simulera lösningar.

**Kapitel som förbereder**

| Kapitel | Innehåll |
|---|---|
| [Kapitel 5](../../kapitel/05-ros2.md) | noder, topics och egna ROS2-noder |
| [Kapitel 6](../../kapitel/06-simulering-och-tillstand.md) | simulering och tillståndsmaskiner |

## Vad du ska göra

Läraren fastställer uppgiften. Materialet som finns är labbarna nedan.

| Labb | Innehåll |
|---|---|
| [Labb 1](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-1-turtlesim.md) | Starta turtlesim och undersök systemet |
| [Labb 2](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-2-topics.md) | Topics och meddelanden |
| [Labb 3](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-3-publicera.md) | Publicera egna kommandon och services |
| [Labb 4](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-4-workspace.md) | Workspace och package |
| [Labb 5](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-5-publisher.md) | Publisher-nod i Python |
| [Labb 6](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-6-subscriber.md) | Subscriber-nod i Python |
| [Labb 7](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-7-vagg-undvikare.md) | Autonom vägg-undvikare |

## Krav och leverabler

Läraren fastställer krav och leverabler. Labbarna har egna inlämningsdelar, och läraren avgör vilka som gäller.

## Arbetsgång

Projektet är planerat till cirka 15 timmar fördelade på 4 lektioner.

| Lektion | Innehåll |
|---|---|
| 1–4 | Innehållet fastställs av läraren. |

## Bedömning

Läraren fastställer bedömningen utifrån [kursplanens kriterier](../../kursplan.md).

## Material

- [ROS2-repot med installationsguide och labbar](https://github.com/abbjetmus/ROS2)
- [Kapitel 5](../../kapitel/05-ros2.md) och [kapitel 6](../../kapitel/06-simulering-och-tillstand.md)

## Tips

- Installationen tar tid. Gör klart den enligt repots instruktioner innan labb 1.
- När något inte fungerar, börja med `ros2 node list` och `ros2 topic echo`.
- Spara dina noder i versionshantering. De återanvänds i Robotmopp.

[Innehåll](../../README.md)
