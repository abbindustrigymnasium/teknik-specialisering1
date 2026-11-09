# Teknik specialisering nivå 1: Smarta system

Kursbok för TEKK1000X på Hitachigymnasiet Västerås.

I kursen fördjupar du dig i ett teknikområde, och här är området **smarta system**: robotik, sensorer, reglering, ROS2, uppkopplade system och säkerhet. Du lär dig hur robotar känner av världen, fattar beslut och kommunicerar. Du lär dig också att testa dina lösningar, redovisa dem och fundera över deras påverkan på människor och miljö.

## Så använder du boken

- Läs kapitlet innan du börjar på projektet som det förbereder. Tabellen längre ned visar vilket kapitel som hör till vilket projekt.
- Gör övningarna som är märkta **Grund** först, och gå sedan vidare till **Vidare** och **Utmaning**. Svaren på räkneuppgifterna döljer sig bakom *Svar*.
- Skriv av kodexemplen, ändra i dem och se vad som händer. Exemplen med ROS2 kräver att ROS2 är installerat, de andra kör du med vanlig Python.
- Bedömningsgrunderna finns i [kursplanen](kursplan.md).

## Kapitel

1. [Vad är ett smart system?](kapitel/01-smarta-system.md)
2. [Robotens kropp](kapitel/02-robotens-kropp.md)
3. [Sensorer och mätning](kapitel/03-sensorer.md)
4. [Säkerhet i uppkopplade system](kapitel/04-sakerhet-och-ctf.md)
5. [ROS2 och robotens nervsystem](kapitel/05-ros2.md)
6. [Simulering och tillståndsmaskiner](kapitel/06-simulering-och-tillstand.md)
7. [Uppkopplade system och MQTT](kapitel/07-uppkopplade-system.md)
8. [Reglering](kapitel/08-reglering.md)
9. [Kvalitetssäkring, dokumentation och presentation](kapitel/09-kvalitet-och-dokumentation.md)
10. [Hållbarhet, regler och teknikens livscykel](kapitel/10-hallbarhet-och-livscykel.md)

## Projekt

Projekt som saknar beskrivning är markerade med *Under planering* i sin projektmapp. Där fastställer läraren upplägget.

1. [Robot: Humanoiden](projekt/humanoiden/README.md) (under planering)
2. [CTF](projekt/ctf/README.md) (under planering)
3. [Robotik: Sköldpaddan](projekt/skoldpaddan/README.md) (under planering)
4. [Robotmopp](projekt/robotmopp/README.md)
5. [Spetsteknikens livscykel](projekt/spetsteknikens-livscykel/README.md) (under planering)
6. [Robotik Hunden](projekt/hunden/README.md) (under planering)

## Vilket kapitel förbereder vilket projekt?

| Kapitel | Förbereder |
|---|---|
| 1 Smarta system | Alla projekt, särskilt Humanoiden och Robotmopp |
| 2 Robotens kropp | Humanoiden, Hunden, Robotmopp |
| 3 Sensorer och mätning | Humanoiden, Robotmopp, Hunden |
| 4 Säkerhet | CTF |
| 5 ROS2 | Sköldpaddan, Robotmopp |
| 6 Simulering och tillståndsmaskiner | Sköldpaddan, Robotmopp |
| 7 Uppkopplade system | Robotmopp, CTF |
| 8 Reglering | Robotmopp, Hunden, Humanoiden |
| 9 Kvalitet och dokumentation | Alla projekt, särskilt Robotmopp |
| 10 Hållbarhet och livscykel | Spetsteknikens livscykel, Robotmopp |

## Kursplan och material

- [Kursplan och täckningsmatris](kursplan.md)
- [ROS2-repot med labbserien](https://github.com/abbjetmus/ROS2): installation och tio labbar, från turtlesim till en autonom bil
- [Labb 8: Robotmoppen i simulering](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-8-robotmopp-simulering.md)
- [Labb 9: Koppla bilen](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-9-koppla-bilen.md)
- [Labb 10: Autonom bil](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-10-autonom-bil.md)
