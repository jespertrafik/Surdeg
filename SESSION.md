# SESSION — 2026-07-26

## Aktiv uppgift

Klar. Temperaturstyrning återinförd i kalkylatorn och landad på rätt modell.
Inget påbörjat arbete ligger halvfärdigt.

## Vad som gjordes

1. Kodgenomgång av `index.html` — hittade att `mode` (Baguette/Bröd) var rent
   kosmetiskt, degdensiteten motsade en tidigare verifierad mätning, 4 MB
   bilder laddas direkt, inget sparas, och `state`/`lastVal` är dubblerade.
2. Grävde i git-historiken efter de försvunna temp-inställningarna. De dog i
   två steg: logiken i 35c5a76 (4 maj), det tomma UI-skalet i ebb2ec8 (29 maj).
   Slidrarna satt kvar och gjorde ingenting i tre och en halv vecka.
3. Återinförde temp-kortet, först med fel koppling (kyltemp → bulkmål), sedan
   rättat till kyltemp → surdeg när grundidén med kanten klarnade.
4. La till varning vid varm kyl med konkret stoppvolym i ml.
5. Uppdaterade README (beskrev fortfarande den gamla modellen) och skrev
   PROJECT-STATE.md.

Commits: d3342c4, a9dbbe7, 30f3c39, 6cede43, d7a8955. Allt pushat till
`origin/main`.

## Beslut och varför

Se PROJECT-STATE.md, avsnitten "Beslut och varför" samt "Förkastade ansatser".
Kärnan: kanten på kärlet är bulkstoppet, alltså är bulkmålet låst och all
temperaturkompensation måste gå via surdegen.

## Verifiering

Beräkningen kördes mot en DOM-stub i node, inte bara lästes:
`<scratchpad>/test.js`. Testet kontrollerar rate-neutralitet över 18–26°C,
att `factor` står still över hela kylspannet, att "jäs till" är identisk med
inmatade ml i ml-läge, och att degvikten är summan av de visade raderna.
Scratchpad-filer överlever inte sessionen — skriv om testet vid behov.

## Nästa steg

Inget planerat. Kalibrering väntar på verkliga bak — se öppna frågor i
PROJECT-STATE.md. De tre siffror som skulle skärpa modellen mest:

1. Hur lång tid bulken faktiskt tar vid en känd kökstemperatur (ger Q10).
2. Hur mycket degen stiger i kylen över natten (ger kant-antagandet).
3. Om bröd vid 70% bulkmål blir rätt jäst.

## Öppna frågor till Jesper

- Bulkas brödet i ett annat kärl än det jäser i?
- Ska något av de kända bristerna åtgärdas (bildstorlek, localStorage,
  degdensitetens 2.6%-avvikelse)? Ingen av dem påverkar bakningen utom den
  sista.
