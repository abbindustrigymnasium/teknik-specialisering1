# Kapitel 4: Säkerhet i uppkopplade system

> **I det här kapitlet lär du dig**
> - varför du bara får testa system som du har tillstånd att testa
> - vad en CTF är och varför man tränar säkerhet på det sättet
> - hur du tänker kring hot mot ett uppkopplat system
> - skillnaden mellan kodning, kryptering och hashning
> - hur du skyddar lösenord och data i dina egna program

Det finns en robot på en verkstad som tar emot kommandon över nätet. Vem som helst som kan skicka meddelanden till den kan också få den att röra sig. Ingen skulle skriva ett sådant program med flit, men det händer hela tiden, eftersom säkerhet ofta tänks på sist. I det här kapitlet lär du dig tänka på det först.

## Regeln som gäller före allt annat

> **Du testar bara system som du har uttryckligt tillstånd att testa.**
> Det gäller även om systemet är dåligt skyddat, även om du bara vill titta, och även om du tänker berätta om felet efteråt.

Att olovligen skaffa sig tillgång till en dators uppgifter kan vara **dataintrång** enligt 4 kap. 9 c § [brottsbalken](https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/brottsbalk-1962700_sfs-1962-700/), och kan ge böter eller fängelse. Det gäller skolans nät, kompisars konton, webbsidor och uppkopplade prylar. Att du är nyfiken är inget försvar.

I kursen arbetar du därför bara i miljöer som läraren har gett dig, eller på träningsplattformar som är byggda för att övas på och vars regler tillåter det. Om du är osäker, fråga först. Om du hittar ett fel i ett riktigt system ska du inte testa vidare. Berätta för läraren eller för systemets ägare. Det kallas **ansvarsfullt rapporterande**. Nationella [CERT-SE](https://www.cert.se/) tar också emot rapporter om säkerhetsincidenter.

## Vad är en CTF?

**CTF** står för *Capture the Flag*. Det är en tävlingsform där du löser säkerhetsuppgifter i en avsiktligt sårbar och lovlig miljö för att hitta en kort textsträng, en **flagga**, som ger poäng. Syftet är att du ska förstå hur fel uppstår, så att du kan undvika dem i dina egna system.

| Kategori | Vad det handlar om |
|---|---|
| Kryptografi | förstå varför svaga chiffer går att knäcka |
| Webb | förstå hur fel i webbapplikationer uppstår |
| Forensik | undersöka filer, loggar och nätverkstrafik |
| Reverse engineering | förstå hur ett färdigt program fungerar |
| Blandat | logik och problemlösning |

Vill du träna utanför de miljöer som läraren ger dig kan du använda plattformar som [picoCTF](https://picoctf.org/), som är gjord för elever. Läs och följ alltid plattformens regler. Flaggorna finns just där, inte i andras system.

## Säkerhetens grund: CIA

Säkerhet brukar beskrivas med tre mål, ofta kallade **CIA** efter de engelska orden:

| Mål | Fråga | Exempel i en robotbil |
|---|---|---|
| **Konfidentialitet** | Vem får se informationen? | Vem kan läsa sensordata? |
| **Integritet** | Kan någon ändra informationen? | Kan någon annan skicka körkommandon? |
| **Tillgänglighet** | Fungerar det när det behövs? | Vad händer om nätet försvinner? |

## Genomgånget exempel: hotmodell för en robotbil

En **hotmodell** är ett strukturerat sätt att fråga: vad vill jag skydda, vad kan gå fel och vad gör jag åt det? Här är ett exempel för en bil som styrs via en MQTT-broker, ett system som du lär dig mer om i [kapitel 7](07-uppkopplade-system.md).

| Tillgång | Hot | CIA | Skydd |
|---|---|---|---|
| Körkommandon | någon annan skickar kommandon till din bil | Integritet | inloggning och behörighet per ämne, unik klient-id |
| Wifi-lösenord | råkar hamna i publik kod | Konfidentialitet | håll hemligheter utanför Git, använd en konfigurationsfil som inte checkas in |
| Sensordata | kan läsas av andra | Konfidentialitet | krypterad anslutning (TLS) där det är möjligt |
| Bilens rörelse | nätet försvinner mitt i körning | Tillgänglighet | dödmansgrepp som stoppar motorn |

Sista raden är viktig. Säkerhet handlar inte bara om angripare, utan också om att systemet ska hamna i ett säkert läge när något går fel. Den sista raden är ett exempel på principen **fail safe**.

Några allmänna principer för försvar:

- **Minsta möjliga behörighet:** ge varje användare och program bara det de behöver.
- **Försvar i flera lager:** räkna med att ett skydd kan falla, och ha ett till.
- **Uppdatera:** kända fel åtgärdas i uppdateringar.
- **Logga:** du kan inte åtgärda det du inte vet om.

## Kodning, kryptering och hashning

Tre saker blandas ofta ihop.

| Teknik | Går att vända? | Kräver nyckel? | Används till |
|---|---|---|---|
| **Kodning**, t.ex. Base64 | ja, av vem som helst | nej | skicka data i ett visst format |
| **Kryptering** | ja, med rätt nyckel | ja | skydda hemligheter |
| **Hashning** | nej | nej | kontrollera att data inte ändrats, lagra lösenord |

```python
import base64
import hashlib

print(base64.b64encode(b"hej").decode())   # aGVq
print(hashlib.sha256(b"hej").hexdigest()[:16])
print(hashlib.sha256(b"Hej").hexdigest()[:16])
```

Kodningen av `hej` blir `aGVq`, och vem som helst kan vända tillbaka den. Hashvärdena för `hej` och `Hej` börjar med helt olika tecken (`9c478bf63e9500cb` respektive `20ac4eea95e143bb`). En liten ändring i indata ger ett helt annat resultat. Det gör hashning användbart för att upptäcka manipulation.

**Caesarchiffret** är ett gammalt chiffer där varje bokstav flyttas ett visst antal steg i alfabetet. Med steget 3 blir ordet `ROBOT` till `URERW`. Det finns bara 25 möjliga steg i det engelska alfabetet, så chiffret går att knäcka genom att prova alla. Därför är det en bra övning men inget skydd. I riktiga system används beprövade algoritmer, som du aldrig skriver själv.

## Lösenord

Långa lösenord är starka, eftersom antalet möjligheter växer väldigt snabbt med längden. Antag att en dator hinner testa en miljard gissningar per sekund, och att systemet tillåter det, vilket ett väl skyddat system inte gör.

| Lösenord | Antal möjligheter | Tid att prova alla |
|---|---|---|
| 8 små bokstäver | 26⁸ ≈ 2,1 · 10¹¹ | ungefär 209 sekunder |
| 12 tecken, gemener, versaler, siffror | 62¹² ≈ 3,2 · 10²¹ | ungefär 102 000 år |

Program ska aldrig spara lösenord i klartext. De ska sparas med en långsam hashfunktion med **salt**, ett slumpvärde per användare. OWASP:s råd finns i länken under Läs mer.

## Skriv säker kod: släpp aldrig in data i kommandon

Ett klassiskt fel uppstår när text från en användare klistras ihop med en databasfråga. Då kan texten tolkas som en del av kommandot. Försvaret är att skicka datan separat från kommandot, med en **parametriserad fråga**.

```python
import sqlite3

db = sqlite3.connect(":memory:")
db.execute("CREATE TABLE robotar (namn TEXT, agare TEXT)")
db.execute("INSERT INTO robotar VALUES (?, ?)", ("Mopp", "Grupp 1"))

agare = "Grupp 1"   # text som kommer från en användare
rad = db.execute("SELECT namn FROM robotar WHERE agare = ?", (agare,)).fetchone()
print(rad[0])   # Mopp
```

Frågetecknet talar om för databasen att värdet alltid är data, aldrig kommando. Samma tänk gäller allt som kommer utifrån: kontrollera format, längd och rimlighet. I din robot kan det vara att begränsa styrvinkeln till tillåtna värden även om någon skickar något annat.

## Begrepp

- **Dataintrång:** att olovligen skaffa sig tillgång till uppgifter i ett datasystem.
- **CTF:** tävlingsform där säkerhet övas i lovliga miljöer.
- **Hotmodell:** strukturerad genomgång av vad som ska skyddas och hur.
- **CIA:** konfidentialitet, integritet och tillgänglighet.
- **Minsta möjliga behörighet:** ingen får mer rättighet än som behövs.
- **Hashning:** enkelriktad beräkning som ger ett fingeravtryck av data.
- **Salt:** slumpvärde som läggs till innan ett lösenord hashas.
- **Parametriserad fråga:** databasfråga där data skickas skilt från kommandot.

## Koppling till projektet

Kapitlet är underlaget för projektet [CTF](../projekt/ctf/README.md), där läraren fastställer upplägget. Hotmodellen kan du också använda på bilen i [Robotmopp](../projekt/robotmopp/README.md).

## Läs mer

- [OWASP: Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP: Query Parameterization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Query_Parameterization_Cheat_Sheet.html)
- [OWASP Internet of Things Project](https://owasp.org/www-project-internet-of-things/)
- [picoCTF](https://picoctf.org/)
- [CERT-SE](https://www.cert.se/)
- [Brottsbalken på riksdagens webbplats](https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/brottsbalk-1962700_sfs-1962-700/)

[← Föregående](03-sensorer.md) | [Innehåll](../README.md) | [Nästa →](05-ros2.md)
