# Fantastic Posters

Dit project geeft Claude Code een postergenerator met 33 handmatig afgestemde visuele stijlen. De generator leest een briefing, kiest de bijpassende stijl, vult de prompttemplate in en rendert via OpenAI's GPT Image 2 (via Fal).

De skill zelf staat in `.claude/skills/fantastic-posters/SKILL.md`. De generator is `generate.js` in de root van het project.

## Setup

1. Plaats je Fal API key in `.env` in de root van het project:

   ```
   FAL_KEY=your_fal_key_here
   ```

   Optioneel: `KIE_KEY=...` schakelt gehoste reference uploads in. Zonder deze key worden refs inline meegegeven als base64 data URI's.

2. Installeer Node.js 18+ als je dat nog niet hebt.

3. Optionele dependencies voor geavanceerde functies:
   ```bash
   npm install pdfjs-dist canvas js-yaml
   ```
   - `pdfjs-dist` + `canvas` maken automatische rendering van PDF brand-books mogelijk.
   - `js-yaml` maakt gestructureerde YAML-briefings mogelijk.

4. Vraag vanuit een Claude Code-sessie in deze map: **"make a poster for [your brief]"**.

## Handmatig gebruik

```bash
node generate.js "boutique wellness retreat in the redwoods"
node generate.js "underground techno gig at the warehouse" --style=indie-gig-riso
node generate.js --brief=./briefs/launch.yaml
node generate.js --batch=./listings.json
node generate.js --template=./reference.png --refs=./new-photo.jpg "headline=NEW, subtitle=YORK"
node generate.js --list
```

## Na het genereren

Om tekst, voorgrond en achtergrond te splitsen voor bewerking, open je de PNG in **Canva** en gebruik je **Magic Layers / Smart Layers**. PSD-layering is beschikbaar via de naastgelegen `poster-to-layers` pipeline als je Photoshop-bewerkbare output wilt.

## Stijlencatalogus

33 stijlen, elk met een voorbeeldrender in `examples/`. Zie `.claude/skills/fantastic-posters/SKILL.md` voor volledige keuzeregels en opmerkingen per stijl.
