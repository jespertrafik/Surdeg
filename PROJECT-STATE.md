# PROJECT-STATE — Brödkalkylator

Senast uppdaterad: 2026-07-26

## Vision

En surdegskalkylator där man aldrig behöver tänka på tid. Du väljer kärl och
läge, appen räknar fram degmängden, och när degen står i kant med hinken är
bulken klar. Ingen klocka, inget streck på hinken, ingen poke-test.

## Grundprincip — allt annat följer av den

**Kanten på kärlet ÄR bulkstoppet.** Det betyder att bulkmålet är en fysisk
konstant som appen inte får röra. Temperaturkompensation måste därför gå via
surdegsmängden, aldrig via målvolymen.

Detta togs fel två gånger under juli-sessionen innan det landade — se
"Förkastade ansatser" nedan.

## Nuvarande modell

| Insignal | Verkar på | Formel |
|---|---|---|
| Bulktemp 18–26°C | surdeg% | `20 × 2.5^(-(T-22)/10)` — rate-neutral |
| Kyltemp 2–8°C | surdeg% | `-(T-4) × 0.5` procentenheter |
| Degvikt | surdeg% | `sizeAdjustment`, 0/0.5/1.0/1.5 pp i steg |
| Läge | bulkmål | Baguette 100%, Bröd 70% |
| Bulkmål-slider | bulkmål | 50–100%, manuell override |

Surdeg clampas till 12–28%. Salt 3% och surdeg hydrering 100% är fasta.

Över 5°C kyltemp visas en varning; över 6°C räknar appen ut en konkret
stoppvolym i ml eftersom kanten då är fel signal.

## Tech stack

Statisk HTML i en enda fil (`index.html`), GitHub Pages, DNS via Cloudflare.
Ingen bygg, inga beroenden, inget ramverk. Repot flyttat till
`github.com/jespertrafik/Surdeg`.

## Beslut och varför

- **Rate-neutral surdeg, multiplikativt.** v2:s linjära 0.5 pp/°C kompenserade
  bara ~1/3 av hastighetsökningen — vid 26°C jäste degen ändå 30% fortare än
  vid 22°C. Multiplikativt ger 0.94–1.00 relativ hastighet över hela spannet.
- **Kyltemp medvetet försiktig.** Appen förutsätter en fungerande kyl och
  försöker inte rädda en som ligger på 8°C. Beslut av Jesper: "har de hög temp
  i kylen, fuck it, de får lära sig själva att kompensera".
- **Ingen tid visas någonstans.** Rate-neutraliteten gör att bulken tar ungefär
  lika lång tid varje gång — det är så tiden blir onödig att tänka på, inte
  genom att visa den.
- **Läget sätter bulkmålet igen.** Kopplingen dog i 9893bbf när bulkmål-slidern
  infördes; mode var kosmetiskt i flera månader. Återinfört 2026-07-26.

## Förkastade ansatser

- **Temp → tidsestimat.** Motsäger hela poängen med appen. Avvisat av Jesper.
- **Kyltemp → bulkmål** (byggt i d3342c4 och a9dbbe7, rivet i 30f3c39).
  Fungerar inte: kanten är bulkstoppet, alltså är målet låst av kärlet.
- **Kyltemp bara i baguette-läge.** Fel antagande — allt kalljäser, både för
  smakens skull och för att det ska bli lättare dagen efter.
- **Sats-storlek härledd ur brödtyp.** Frallor delas inte förrän efter kylen,
  så en fralldeg är en av de större odelade klumparna. Ett bröd på 300 g mjöl
  är det lilla fallet.

## Öppna frågor

- Q10 = 2.5 är ett litteraturvärde, inte mätt på Jespers kultur.
- Kyltempens 0.5 pp/°C är satt på känsla.
- Antagandet att kanten fungerar vid 4°C vilar på Jespers erfarenhet, inte på
  en mätning av hur mycket degen faktiskt stiger över natten.
- Bröd 70% bulkmål — inte verifierat mot verkligt bak.
- Bulkas brödet i samma kärl som det jäser i? Påverkar om brödläget också
  behöver kant-logiken.

## Kända brister (från kodgenomgång 2026-07-26, ej åtgärdade)

- `baguette.png` 2.4 MB + `boule.png` 1.8 MB laddas direkt. 4 MB på en app man
  öppnar i köket på mobilen.
- Inget sparas — hydrering, dinkel och volym nollställs vid varje besök.
- `state` och `lastVal` är samma sak, uppdateras identiskt på tre ställen.
- Degdensitetsformeln ger 1.217 g/ml vid H=80 medan mätningen i a46fac9 gav
  1.25 (440 g deg i 352 ml). ~2.6% avvikelse.
