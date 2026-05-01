---
name: fantastic-posters
description: Genereer fantastische posters in 33 uitgesproken visuele stijlen met GPT Image 2 (via Fal). Kiest automatisch de juiste stijl op basis van de briefing van de gebruiker, bouwt een prompt op basis van een template en rendert. Multi-ref + logo + brief + batch + template modes. Triggert op "fantastic posters", "quality posters", "make a poster", "poster style", "generate poster".
---

# Fantastic Posters

Een postergenerator met een samengestelde catalogus van 33 visuele stijlen. De agent kiest de stijl die bij de briefing past, bouwt de prompt en genereert met GPT Image 2 (via Fal). Multi-reference uploads, brand-book PDF's, gestructureerde briefings, batchgeneratie en template-replicatie zijn allemaal first-class functies.

Na het genereren wordt laag-scheiding buiten deze skill afgehandeld — open de PNG in Canva en gebruik Magic / Smart Layers om voorgrond/achtergrond/tekst te splitsen.

## Uitvoeren

`generate.js` staat in de root van het project (naast `styles.js`).

```bash
cd <repo-root>
node generate.js "<brief>"                                     # auto-pick, 1 image
node generate.js "<brief>" --style=<style_id>                  # force a style
node generate.js "<brief>" --n=3                               # 3 variations
node generate.js "<brief>" --refs=hero.jpg,brand.pdf,logo.png  # multi-ref edit
node generate.js "<brief>" --logo=<path>                       # logo-anchored edit
node generate.js --brief=path/to/brief.{md,yaml}               # structured brief
node generate.js --batch=path/to/listings.json                 # iterate many briefs
node generate.js --template=existing.png "<brief>"             # replicate-template
```

Flags: `--size=portrait|landscape|square|WxH`, `--quality=low|medium|high`, `--palette="#hex,..."`, `--yes`, `--include-experimental`.

Het script leest `FAL_KEY` (en optioneel `KIE_KEY`) uit `.env` in de root van het project. Output-PNG's worden opgeslagen in `./out/`.

## Wanneer de gebruiker zegt: "Make a Poster"

1. Lees de briefing. Bepaal de mood (calm / vibrant / nostalgic / mystical / luxury / corporate / playful) en het onderwerp (event / product / album / movie / listing / retreat).
2. Kies de best passende stijl uit de catalogus met de **Style Picker**-regels hieronder.
3. **Toon de relevante `examples/<style-id>.png` aan de gebruiker vóór het genereren** — genereer de catalogusshowcase nooit opnieuw. De referentierender is de baseline.
4. Vertel de gebruiker welke stijl je hebt gekozen en waarom (één zin).
5. Voer `generate.js` uit. Standaard is `--n=1`. Als ze "more designs" of "variations" zeggen, gebruik `--n=3`.
6. Het script print een geschatte kostprijs en vraagt om bevestiging. Bij >=5 afbeeldingen of `--quality=high` vraagt het altijd om bevestiging, ongeacht `--yes`.
7. Geef na het opslaan het bestandspad en herinner ze eraan: **open in Canva and use Magic Layers if they want to edit text or swap the subject.**

## Volgorde van referentiebeelden (voor `--refs`)

Multi-reference uploads volgen deze conventie:

1. **Afbeelding 1 — hero photo** (het hoofdonderwerp)
2. **Afbeelding 2 — brand book** (PDF rendert automatisch naar PNG-pagina 1 op 2x DPI)
3. **Afbeelding 3+ — logo's**

Voor `--template` mode is de volgorde: template (1e) → nieuwe hero photo (2e) → optionele logo's.

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
| brutalist / broadcast / jersey-number / HYROX-style | `brutalist-broadcast` |
| restaurant / wine bar / jazz lounge / brasserie / hospitality | `emerald-nocturne` |
| absurd transit map / mood diagram *(experimental)* | `absurd-transit-map` |

Als niets met voldoende zekerheid matcht, vraag de gebruiker om te kiezen uit een shortlist van 3 opties.

## Out-of-Left-Field Mode

Wanneer de gebruiker vraagt om "out of left field", "weird", "different", "surprise me" of "experimental" ideeën, zijn standaard cataloguskeuzes verboden. In plaats daarvan:

1. **Varieer palette en typografie weg van de catalogusdefaults.** Grijp niet naar de voor de hand liggende keuze.
2. **Haal inspiratie uit minder voor de hand liggende designreferenties via online research.** Web search/fetch vanuit: Polish theatre poster archives (Jan Lenica, Henryk Tomaszewski), Japanese book covers (Kohei Sugiura, Tadanori Yokoo), Czech New Wave film posters, AIGA poster annuals, Dribbble's experimental tag.
3. **Stel 5+ ideeën voor met elk een vibe van één regel VÓÓR het genereren.** Laat de gebruiker kiezen.
4. **Val bij dit soort verzoeken nooit terug op veilige cataloguskeuzes.**

## Logo Handling Protocol

- Geef logo's mee als base64 data URI's (automatisch afgehandeld wanneer je `--logo=<path>` meegeeft), nooit als lokale padstring.
- Gebruik de `gpt-image-2/edit` endpoint wanneer een logo wordt meegegeven (automatisch afgehandeld).
- Voeg de "do NOT redraw, recolour, or modify proportions"-clausule toe aan de prompt (automatisch afgehandeld).
- Wanneer een briefing "mark on black panel" of "mark on white panel" specificeert (HYROX-affiliate werk), dwing dit af in de prompt.
- Specificeer bij **dual-wordmark** layouts (klant + partner) gelijke visuele zwaarte, gescheiden door een hairline rule, nooit gecombineerd tot één lockup.
- Betrouwbaarheid van logoplaatsing is niet perfect, zelfs met de edit endpoint — controleer het gerenderde logo zorgvuldig vóór oplevering.

## Vertrouw de referentie

Bij gebruik van `--template` of een multi-ref edit mode werkt **de kortste prompt die ALLEEN benoemt wat verandert beter dan uitgebreide specificaties.** Vertrouw erop dat de referentieafbeelding layout, typografie, palette en logo draagt. Herhaal niet wat de referentie al toont. Uitgebreide specificaties laten het model afdwalen.

## Wat referenties wel en niet kunnen doen

- Referenties sturen stijl/content, geen pixel-perfect templatekopieën.
- Exacte fontreproductie is onbetrouwbaar voorbij ongeveer 6 woorden. Verkort titels.
- Logo's kunnen subtiel opnieuw getekend worden — geef ze mee als aparte ref voor sterkere verankering (`--logo=`) of componeer ze achteraf in Canva.
- De outputverhouding volgt `image_size` (de `--size` flag), niet de referentieafbeelding. Stel bij `--template` mode handmatig `--size` in zodat deze overeenkomt met de aspect ratio van de template.

## Generatie-instellingen (kostentabel)

| Kwaliteit | $/afbeelding | Tijd | Wanneer gebruiken |
|---|---|---|---|
| `low` | ~$0.011 | 10-15s | concepten, richtingen verkennen |
| `medium` | ~$0.04 | 25-40s | klantreview |
| `high` | ~$0.17 | 60-90s | definitieve oplevering (daarna extern upscalen) |

Standaard is `--size=portrait` 1024x1536 (max 1536/zijde). Voor A2-print op 300 DPI schaal je extern op met Topaz Photo AI of Real-ESRGAN.

Bij >=5 afbeeldingen of `--quality=high` vraagt de CLI altijd om bevestiging, ongeacht `--yes`.

## Subagent Fan-Out (canoniek bulkpatroon)

Gebruik voor 10+ briefings Claude's Agent tool om uit te waaieren — één subagent per briefing, waarbij elke subagent deze skill onafhankelijk uitvoert. Voorbeeld:

```
Spawn N subagents. Each runs:
  fantastic-posters --brief=briefs/{client}.md --refs=hero.jpg,brand.pdf,logo.png
```

Subagents zijn hoe deze skill snel batches voor echte klanten produceert.

## Regels

- **Gebruik echte merknamen wanneer ze worden aangeleverd.** Werk voor echte klanten is de primaire use case. Anonimiseer alleen bij generieke demo's.
- **Overdrijf kalme stijlen niet** — bij `pastel-mindful` en `sumi-e-zen` is terughoudendheid juist het punt.
- **Footer billing line** staat altijd als laatste — datum · locatie · prijs/credit.
- **Titelweergave** — GPT Image 2 is sterk in typografie, maar niet perfect. Als een titel langer is dan ongeveer 6 woorden, kun je typfouten verwachten.
- **Variaties** — varieer bij `--n=3` het onderwerp licht in plaats van 3x dezelfde prompt te gebruiken.
- **Show, don't regenerate.** Toon bij het automatisch kiezen van een stijl eerst `examples/<style-id>.png` — genereer de catalogus nooit opnieuw.

## Buiten scope

- **Geen PSD-layering in deze skill.** Verwijs de gebruiker naar **Canva → Magic / Smart Layers**. PSD-layering is beschikbaar via de naastgelegen `poster-to-layers` pipeline als Photoshop de voorkeur heeft.
- **Geen upscaling in deze skill.** Verwijs gebruikers naar Topaz Photo AI of Real-ESRGAN voor output op printresolutie.
- **Geen animatie.** Alleen stilstaande beelden.
