# Fantastic Posters

Een Claude Code-skill die fantastische posters genereert in **33 uitgesproken visuele stijlen** met OpenAI's GPT Image 2 (via Fal). De skill kiest automatisch de juiste stijl op basis van je briefing, bouwt een prompt op basis van een template en rendert de afbeelding. Nu met multi-reference uploads, brand-book PDF's, gestructureerde briefings, batchgeneratie en template-replicatie.

Na het genereren kun je de PNG in **Canva → Magic / Smart Layers** plaatsen om voorgrond, achtergrond en tekst te splitsen voor bewerking.

## Snel starten

```bash
git clone <vullink>
cd fantastic-posters
echo "FAL_KEY=your_fal_key_here" > .env
node generate.js --list
node generate.js "annual report cover for a green-energy holding company"
```

Zeg daarna in Claude Code, in deze map, simpelweg: **"make a poster for [brief]"**.

## Nieuw in v0.2

- **`--refs=hero.jpg,brand.pdf,logo.png`** — multi-reference uploads met automatisch gerenderde PDF-pagina 1
- **`--logo=<path>`** — logo-insluiting als base64 data URI, stijl-onafhankelijk, met "do not redraw"-clausule
- **`--brief=brief.{md,yaml}`** — gestructureerde klantbriefing als input
- **`--batch=listings.json`** — veel briefings itereren met gedeelde refs
- **`--template=existing.png`** — replicate-template mode (kopieer layout, vervang foto + tekst)
- **`--size=portrait|landscape|square|WxH`** + **`--quality=low|medium|high`**
- **`--palette="#hex,#hex,#hex"`** — strikte palette override
- Kosteninschatting + **`--yes`**-bevestiging. >=5 afbeeldingen of `--quality=high` vraagt altijd om bevestiging.
- 2 nieuwe stijlen (**brutalist-broadcast**, **emerald-nocturne**) en `absurd-transit-map` gemarkeerd als experimenteel.

## Wat is inbegrepen

```
fantastic-posters/
├── .claude/skills/fantastic-posters/SKILL.md   # De skill — keuzeregels + stijlopmerkingen
├── styles.js                                   # 33 prompttemplates + auto-picker
├── generate.js                                 # CLI: node generate.js "<brief>"
├── examples/                                   # Eén voorbeeldrender per stijl (PNG)
├── CLAUDE.md
└── README.md
```

## De 33 stijlen

Elke stijl wordt geleverd met een referentierender in `examples/<style-id>.png`. Toon deze aan de gebruiker vóór het genereren — genereer de catalogus nooit opnieuw.

| ID | Vibe | Voorbeeld |
|---|---|---|
| `cinematic-neonoir` | Regenachtig Tokio, neon, verweerde serif | `examples/cinematic-neonoir.png` |
| `vintage-travel` | WPA flat-color uit de jaren 50 | `examples/vintage-travel.png` |
| `swiss-minimal-typo` | Eén geometrische vorm, Helvetica-stack | `examples/swiss-minimal-typo.png` |
| `tech-conf-darkmode` | Houtskool, chromen sculptuur, monospace footer | `examples/tech-conf-darkmode.png` |
| `corporate-report` | Redactionele foto, Didone serif, royale witruimte | `examples/corporate-report.png` |
| `indie-gig-riso` | Tweekleuren-risograph, handgesneden zine | `examples/indie-gig-riso.png` |
| `luxury-real-estate` | Foto bovenste 2/3, bosgroene serif (gebruikt `--ref`) | `examples/luxury-real-estate.png` |
| `luxury-estate-cover` | Full-bleed landgoed bij schemering, ingetogen magazinegevoel | `examples/luxury-estate-cover.png` |
| `art-deco` | Gouden zonnestraal op nachtblauw, gespiegeld ornament | `examples/art-deco.png` |
| `bauhaus-geometric` | Primaire vormen, lowercase sans, hairline rules | `examples/bauhaus-geometric.png` |
| `ukiyo-e` | Japanse houtsnede, verticale kanji, hanko-zegel | `examples/ukiyo-e.png` |
| `psychedelic-60s` | Smeltende bubble lettering uit het Fillmore-tijdperk | `examples/psychedelic-60s.png` |
| `vaporwave-synth` | Zonsondergangsgradient, chromen vloerrooster, Romeinse buste | `examples/vaporwave-synth.png` |
| `saul-bass-minimal` | Knip-papiergraphic, tweepassige screen print | `examples/saul-bass-minimal.png` |
| `memphis-80s` | Mintgroene achtergrond, terrazzo- en zigzagpatronen | `examples/memphis-80s.png` |
| `editorial-fashion` | Full-bleed portret, hoge serif masthead | `examples/editorial-fashion.png` |
| `symmetric-storybook` | Pastelkleurig diorama, mosterdgele Futura, deco-rand | `examples/symmetric-storybook.png` |
| `pop-art-comic` | Ben-Day dots, comic speech burst | `examples/pop-art-comic.png` |
| `pastel-mindful` | Stoffig roze naar saliegroene gouache, keramische theekop | `examples/pastel-mindful.png` |
| `sumi-e-zen` | Rijstpapier, één bamboe-penseelstreek | `examples/sumi-e-zen.png` |
| `loteria-folk` | Mexicaanse folk-art, papel-picado rand | `examples/loteria-folk.png` |
| `surreal-dreamscape` | Schilderachtige olie, regende appels, zwevende deuropening | `examples/surreal-dreamscape.png` |
| `documentary-portrait` | Magnum-stijl zwart-wit reportage | `examples/documentary-portrait.png` |
| `sports-action-hero` | Runner bij stadionnacht, lens flares, stencil sans | `examples/sports-action-hero.png` |
| `album-cover-portrait` | Soul-funk vinyl uit de jaren 70, mosterdband + serif | `examples/album-cover-portrait.png` |
| `post-apoc-sword` | Koreaanse action-RPG key art, vrouwelijke krijger | `examples/post-apoc-sword.png` |
| `lone-traveler-cargo` | Melancholische reiziger + cargo, IJslandse asvlakte | `examples/lone-traveler-cargo.png` |
| `neon-noir-cyberpunk` | Regenachtige megacity, hologrambillboards | `examples/neon-noir-cyberpunk.png` |
| `streetwear-lookbook` | Betonnen studio, oversized cargo + hoodie | `examples/streetwear-lookbook.png` |
| `minimal-tech-keynote` | Zuiver zwart, één zwevend product | `examples/minimal-tech-keynote.png` |
| `brutalist-broadcast` | Modulair grid, jersey-number digit, duotone band | `examples/brutalist-broadcast.png` |
| `emerald-nocturne` | Fluwelen jewel-tone, messing + champagne, gegraveerde caps | `examples/emerald-nocturne.png` |
| `absurd-transit-map` *(experimenteel)* | Vignelli metrodiagram met mood-station names | `examples/absurd-transit-map.png` |

## Gebruiksvoorbeelden

```bash
# Automatisch kiezen op basis van keywords in de briefing
node generate.js "Día de los Muertos community festival"

# Een stijl forceren + 3 variaties
node generate.js "narrative game launch" --style=lone-traveler-cargo --n=3

# Multi-reference edit: hero photo + brand book PDF + logo
node generate.js "summer launch poster" \
  --refs=./hero.jpg,./brand-book.pdf,./logo.png \
  --quality=high

# Logo-anchored generation (elke stijl)
node generate.js "open day, restaurant" \
  --style=emerald-nocturne \
  --logo=./client-logo.png

# Gestructureerde briefing
node generate.js --brief=./briefs/forge-strength.yaml

# Bulk batch — array met verschillende briefings die gedeelde refs gebruiken
node generate.js --batch=./listings.json

# Replicate-template mode — kopieer de layout van een bestaande afgeronde poster
node generate.js --template=./template.png \
  --refs=./new-photo.jpg \
  "headline=New listing, subtitle=23 Oak Avenue"

# Strikte palette override
node generate.js "executive briefing" \
  --style=corporate-report \
  --palette="#0E3B2E,#C9A96E,#FAFAF6"
```

## Volgorde van referentiebeelden (voor `--refs`)

Volgens conventie volgen multi-reference uploads deze volgorde:

1. **Afbeelding 1 — hero photo** (het hoofdonderwerp)
2. **Afbeelding 2 — brand book** (PDF rendert automatisch naar PNG-pagina 1 op 2x DPI)
3. **Afbeelding 3+ — logo's** (meegegeven als extra refs of via `--logo`)

Voor `--template` mode is de volgorde: template (1e) → nieuwe hero photo (2e) → optionele logo's.

## Subagent fan-out (canoniek bulkpatroon)

Voor 10+ briefings kun je uitwaaieren via Claude's Agent tool — één subagent per briefing, waarbij elke subagent deze skill onafhankelijk uitvoert:

```
Spawn N subagents. Each runs:
  fantastic-posters --brief=briefs/{client}.md --refs=hero.jpg,brand.pdf,logo.png
```

Subagents zijn hoe deze skill snel batches voor echte klanten produceert.

## Instellingen

| Kwaliteit | $/afbeelding | Tijd | Wanneer gebruiken |
|---|---|---|---|
| `low` | ~$0.011 | 10-15s | concepten, richtingen verkennen |
| `medium` | ~$0.04 | 25-40s | klantreview |
| `high` | ~$0.17 | 60-90s | definitieve oplevering (daarna extern upscalen) |

De standaardgrootte is `portrait` (1024x1536). GPT Image 2 gaat maximaal tot 1536 aan één zijde. Voor A2-print op 300 DPI wil je extern upscalen (Topaz Photo AI, Real-ESRGAN).

GPT Image 2 is momenteel het sterkste model voor tekstweergave — titels, billing blocks en masthead lockups blijven goed overeind. Als een titel langer is dan ongeveer 6 woorden, kun je typfouten verwachten; verkort de titel en draai opnieuw.

## Na het genereren

1. Open de PNG in [Canva](https://canva.com).
2. Klik met rechts → **Magic / Smart Layers** om voorgrond, achtergrond en tekst te splitsen.
3. Bewerk tekst of vervang het onderwerp zonder opnieuw te renderen.

PSD-layering is beschikbaar via de naastgelegen `poster-to-layers` pipeline als je Photoshop-bewerkbare output wilt in plaats van Canva.

## Je eigen stijl toevoegen

1. Open `styles.js`.
2. Voeg een entry toe aan het `styles` object.
3. Voeg een rij toe aan de `PICK_RULES` array zodat de auto-picker de stijl kan vinden.
4. Plaats een referentierender in `examples/<your-style-id>.png`.
5. Werk de catalogustabel in `SKILL.md` en deze README bij.

## Licentie

MIT.
