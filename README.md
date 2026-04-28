# Brödkalkylator

Surdegskalkylator: ange önskad jäsvolym → få ut exakt recept.

Live: https://surdeg.jespertrafik.com

## Logik
- Volym efter bulk är facit för "klar" — ingen poke-test, inga gissningar
- Sann hydrering: surdegens mjöl/vatten räknas in i totalen
- Faktor beräknas från bulktemp, kyltemp och degvikt (termisk tröghet i kyl)
- Fasta värden: surdeg 20%, salt 3%, surdeg hydrering 100%
- Två lägen: Baguette (rakt i kyl efter bulk) / Bröd (bulk → preshape → final proof)

## Stack
Statisk HTML, hostad på GitHub Pages, DNS via Cloudflare.
