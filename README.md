# Brödkalkylator

Surdegskalkylator: ange önskad jäsvolym → få ut exakt recept.

Live: https://surdeg.jespertrafik.com

## Grundidén

Kanten på kärlet är stoppsignalen. Du gör så mycket deg att den står i kant med
hinken när bulken är klar — då behövs ingen klocka, inget streck och ingen
poke-test. Appens uppgift är att räkna fram den degmängden.

Allt annat i modellen följer av det: bulkmålet är låst av kärlet och kan inte
justeras av appen, så all temperaturkompensation måste ske via surdegsmängden.

## Logik

- **Volym efter bulk är facit för "klar"** — ingen tid visas någonstans i appen
- **Bulktemp → surdeg%**, rate-neutralt: `20 × 2.5^(-(T-22)/10)`, clamp 12–28%.
  Varmt kök ger mindre surdeg så bulken tar ungefär lika lång tid varje gång.
  Multiplikativt eftersom jäsningen är exponentiell i temperaturen (Q10 ≈ 2.5)
- **Kyltemp → surdeg%**, mild linjär justering 0.5 procentenheter/°C från 4°C.
  Medvetet försiktig: appen förutsätter en fungerande kyl
- **Degvikt → surdeg%**, `sizeAdjustment`: stor degmassa värmer sig själv av
  jäsningen och kyls långsammare, så den får mindre surdeg
- **Bulkmål** sätts av läget (Baguette 100%, Bröd 70%) och kan justeras med
  slidern. Temperatur rör det aldrig — kanten sitter där den sitter
- **Varning över 5°C kyl**: surdegsjusteringen räcker inte längre, appen räknar
  då ut en konkret stoppvolym i ml att stanna på i stället för kanten
- **Sann hydrering**: surdegens mjöl/vatten räknas in i totalen
- **Degdensitet** räknas ur ingrediensformeln, H-beroende (~1.22 g/ml)
- Fasta värden: salt 3%, surdeg hydrering 100%
- Två lägen: Baguette (rakt i kyl efter bulk) / Bröd (bulk → preshape → final proof)

## Stack

Statisk HTML i en fil, hostad på GitHub Pages, DNS via Cloudflare.
Ingen bygg, inga beroenden.
