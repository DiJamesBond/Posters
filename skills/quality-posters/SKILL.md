---
name: quality-posters
description: Genereer kwaliteitsposters in 31 uitgesproken visuele stijlen met GPT Image 2 (via Fal). Kiest automatisch de juiste stijl op basis van de briefing van de gebruiker, bouwt een prompt op basis van een template en rendert. Triggert op "quality posters", "make a poster", "poster style", "generate poster".
---

# Quality Posters

Een postergenerator met een samengestelde catalogus van 31 visuele stijlen. De agent kiest de stijl die bij de briefing past, bouwt de prompt en genereert met GPT Image 2.

Na het genereren wordt laag-scheiding buiten deze skill afgehandeld — open de PNG in Canva en gebruik Magic / Smart Layers om voorgrond/achtergrond/tekst te splitsen voor bewerking.

## Uitvoeren

`generate.js` staat in de root van het project (naast `styles.js`). Deze skillmap bevat alleen `SKILL.md`.

```bash
cd <repo-root>
node generate.js "<brief>"                         # auto-pick style
node generate.js "<brief>" --style=<style_id>      # force a style
node generate.js "<brief>" --n=3                   # 3 design variations
node generate.js "<brief>" --style=<style_id> --n=3
```

Het script leest `FAL_KEY` uit `.env` in de root van het project. Output-PNG's gaan naar `./out/`.

## Wanneer de gebruiker zegt: "Make a Poster"

1. Lees de briefing. Bepaal de mood (calm / vibrant / nostalgic / mystical / luxury / corporate / playful) en het onderwerp (event / product / album / movie / listing / retreat).
2. Kies de best passende stijl uit de catalogus met de **Style Picker**-regels hieronder. Als twee stijlen passen, kies dan de meest onderscheidende — de gebruiker kan om een andere stijl vragen als het niet raak is.
3. Vertel de gebruiker welke stijl je hebt gekozen en waarom (één zin).
4. Voer `generate.js` uit. Standaard is `--n=1`. Als ze "more designs" of "variations" zeggen, gebruik `--n=3` (of het aantal dat ze vragen).
5. Geef na het opslaan het bestandspad en herinner ze eraan: **open in Canva and use Magic Layers if they want to edit the text or swap the subject.**

## Style Picker (auto-match op briefing-intentie)

| Als de briefing gaat over... | Kies |
|---|---|
| moody crime / thriller / dark cinematic | `cinematic-neonoir` |
| travel / destination / vintage tourism | `vintage-travel` |
| design lecture / minimal swiss / typography | `swiss-minimal-typo` |
| tech conference / agentic web / dev event | `tech-conf-darkmode` |
| annual report / executive / finance | `corporate-report` |
| live music / DIY gig / underground band | `indie-gig-riso` |
| home listing / open house with photo | `luxury-real-estate` |
| luxury estate brochure / architectural retreat | `luxury-estate-cover` |
| art deco / Gatsby / 1920s glam | `art-deco` |
| Bauhaus / primary geometric / design school | `bauhaus-geometric` |
| Japanese woodblock / Edo / classical Japan | `ukiyo-e` |
| sixties rock / Fillmore / hippie concert | `psychedelic-60s` |
| synthwave / retro futurism / 80s sunset | `vaporwave-synth` |
| minimalist film / cut-paper / Hitchcock vibe | `saul-bass-minimal` |
| 80s postmodern / playful clashing patterns | `memphis-80s` |
| high fashion magazine / editorial cover | `editorial-fashion` |
| symmetric pastel / dollhouse / storybook film | `symmetric-storybook` |
| comic / Ben-Day dots / pop art | `pop-art-comic` |
| wellness / meditation / retreat / soft calm | `pastel-mindful` |
| zen / Japanese ink / monastic minimal | `sumi-e-zen` |
| Día de los Muertos / Mexican folk / festival | `loteria-folk` |
| surreal / Magritte / dreamlike | `surreal-dreamscape` |
| documentary / Magnum reportage / photo essay | `documentary-portrait` |
| stadium / race / athletic event campaign | `sports-action-hero` |
| album cover / vinyl / soul-funk debut | `album-cover-portrait` |
| post-apocalyptic action game key art | `post-apoc-sword` |
| melancholic sci-fi wanderer / cargo / Iceland | `lone-traveler-cargo` |
| cyberpunk / neon noir / dystopian megacity | `neon-noir-cyberpunk` |
| streetwear lookbook / drop / collection | `streetwear-lookbook` |
| tech product reveal / keynote / Apple-style | `minimal-tech-keynote` |
| absurd transit map / mood diagram | `absurd-transit-map` |

Als niets met voldoende zekerheid matcht, vraag de gebruiker om te kiezen uit een shortlist van 3 dichtstbijzijnde stijlen.

## Stijlencatalogus

Elke stijl is een zelfstandige prompttemplate die is gedefinieerd in `styles.js`. De velden die de agent invult verschillen per stijl (title, subtitle, date, location, enzovoort) — de template vertelt de gebruiker wat nodig is. Veelvoorkomende velden:

- `title` — hoofdheadline
- `subtitle` — ondersteunende regel of kicker
- `body` — klein tekstblok (line-up, quote, listingdetails)
- `footer` — datum / venue / billing block
- `subject` — het centrale geïllustreerde of gefotografeerde element

Als een veld niet is aangeleverd, laat het generiek (bijv. "EVENT NAME · CITY · DATE") in plaats van 5 vragen te stellen.

### Catalogus (31 stijlen)

1. `cinematic-neonoir` — regenachtige steeg in Tokio, neon, verweerde serif title, fake billing block
2. `vintage-travel` — WPA / Roger Broders uit de jaren 50, flat color, cobalt + cream + red
3. `swiss-minimal-typo` — puur wit, één geometrische vorm, Helvetica-stack, royale ruimte
4. `tech-conf-darkmode` — houtskool, abstracte chromen sculptuur, geometrische sans-serif title, monospace footer
5. `corporate-report` — premium off-white, één redactionele foto, Didone serif, verfijnde witruimte
6. `indie-gig-riso` — tweekleuren-risograph, halftone collage, handgesneden zinegevoel
7. `luxury-real-estate` — listing flyer, foto bovenste 2/3, bosgroene serif, datablok met drie kolommen (gebruikt `--ref=<image>`)
8. `luxury-estate-cover` — full-bleed landgoedfoto bij schemering, Didone "QUIET MAJESTY", ingetogen magazinecover
9. `art-deco` — gouden geometrische zonnestraal op middernachtzwart, hoge condensed serif, gespiegeld ornament
10. `bauhaus-geometric` — primaire rode/blauwe/gele vormen, lowercase sans, hairline rules
11. `ukiyo-e` — Japanse houtsnede, grote golf of Fuji, verticale kanji-kolom + klein hanko-zegel
12. `psychedelic-60s` — handgetekende smeltende bubble lettering uit het Fillmore-tijdperk, hot magenta + lime + tangerine
13. `vaporwave-synth` — zonsondergangsgradient, chromen vloerrooster, Romeinse buste, gloeiende roze + cyaan title
14. `saul-bass-minimal` — één gescheurde-papiergraphic op cream, tweepassige screen print
15. `memphis-80s` — mintgroene achtergrond, botsende terrazzo- en zigzagpatronen, chunky multicolor type
16. `editorial-fashion` — full-bleed studioportrait, hoge serif masthead, verticale cover lines
17. `symmetric-storybook` — perfect symmetrisch pastel-diorama, mosterdgele Futura, decoratieve deco-rand
18. `pop-art-comic` — Ben-Day dots, dikke zwarte inktcontouren, comic speech burst
19. `pastel-mindful` — stoffig roze naar saliegroene gouache wash, keramische theekop, lichte serif-typografie
20. `sumi-e-zen` — rijstpapier, één bamboe-penseelstreek, enorme negatieve ruimte, vermiljoen hanko
21. `loteria-folk` — levendige Mexicaanse folk-art, papel-picado rand, marigold + sapphire + cactus green
22. `surreal-dreamscape` — heldere lucht waaruit appels regenen, zwevende deuropening, schilderachtige oil-on-canvas
23. `documentary-portrait` — Magnum-stijl zwart-wit reportageportret, hard natuurlijk licht, expositieposter
24. `sports-action-hero` — runner midden in de pas bij stadionnacht, lens flares, stencil sans + tangerine
25. `album-cover-portrait` — soul-funk vinylcover uit de jaren 70, mosterdband + serif title, warm tungsten light
26. `post-apoc-sword` — Koreaanse AAA action-RPG key art, vrouwelijke krijger + gloeiend zwaard, verwoeste megacity
27. `lone-traveler-cargo` — melancholische sci-fi reiziger met cargo stack, IJslandse asvlakte, gouden deeltjes
28. `neon-noir-cyberpunk` — regenachtige megacity, hologrambillboards, anamorphic flares, cinematic 70mm
29. `streetwear-lookbook` — betonnen studioachtergrond, oversized cargo + hoodie, Tokyo-NYC editorial
30. `minimal-tech-keynote` — zuiver zwart, één zwevende product hero, ultradunne lowercase sans, royale witruimte
31. `absurd-transit-map` — Vignelli metrodiagram, gekleurde routelijnen, stations genoemd naar emotionele toestanden

## Generatie-instellingen (locked)

```
endpoint:    https://fal.run/openai/gpt-image-2
image_size:  portrait_16_9
quality:     medium
num_images:  1 (per --n)
output:      png
```

Gebruik voor `luxury-real-estate` met een fotoreferentie `https://fal.run/openai/gpt-image-2/edit` en geef de geüploade image URL mee (elke S3/CDN URL werkt; het script handelt upload via Kie af als er een Kie key in `.env` staat).

## Regels

- **Anonimiseer** — zet geen echte persoonsnaam of bekend merk in de prompt tenzij de gebruiker daar expliciet om vraagt. Gebruik generieke stand-ins (bijv. "Acme Holdings" en geen echt bedrijf).
- **Overdrijf kalme stijlen niet** — bij `pastel-mindful` en `sumi-e-zen` is terughoudendheid juist het punt. Laad de prompt niet vol met extra elementen.
- **Footer billing line** staat altijd als laatste — datum · venue · prijs/credit.
- **Titelweergave** — GPT Image 2 is sterk in typografie, maar niet perfect. Als een titel langer is dan ongeveer 6 woorden, kun je typfouten verwachten. Stel voor om te verkorten.
- **Variaties** — varieer bij `--n=3` het onderwerp licht (ander kleuraccent, andere framing) in plaats van 3× dezelfde prompt te gebruiken.

## Buiten scope

- **Geen PSD-layering.** Verwijs de gebruiker naar **Canva → Magic / Smart Layers** voor scheiding en bewerking van voorgrond/achtergrond/tekst.
- **Geen upscaling.** Output is wat GPT Image 2 teruggeeft.
- **Geen animatie.** Dit is een still-image skill.
