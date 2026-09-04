# AI dagbók

**Nemandi:**
**Áfangi:** Vefkerfi 1
**Önn:**

---

## Um þessa dagbók

Þetta skjal er viðbætanlegt (append-only). Nýjar lotur bætast neðst. Eldri færslum er
aldrei breytt eða eytt — ef eitthvað reyndist rangt síðar er það leiðrétt í nýrri
færslu, ekki með því að endurskrifa þá gömlu.

**Hver fyllir út hvað:**

| Hluti | Fyllt út af |
|---|---|
| Hvað ég bað Agent um | Agent (orðrétt) |
| Hvað Agent gerði | Agent |
| Hvað ég sannreyndi | Nemandi |
| Villur og hafnaðar tillögur | Nemandi |
| Hvað ég gerði sjálf/ur | Nemandi |
| Hvað ég lærði | Nemandi |

Agent skráir staðreyndir. Matið er nemandans. Agent á ekki að skrifa mat á eigin
frammistöðu — þá er dagbókin ekki lengur heimild um gagnrýna notkun.

Reitir merktir `<!-- FYLLIST ÚT AF NEMANDA -->` skal Agent skilja eftir tóma.

---

## Lotusniðmát

Afritið blokkina hér að neðan fyrir hverja nýja lotu.

```markdown
## Lota N — ÁÁÁÁ-MM-DD

**Verkefni:**
**Módel:**
**Tími með Agent:**
**Skrár sem unnið var með:**

### Hvað ég bað Agent um

| # | Beiðni (orðrétt) | Tilgangur |
|---|---|---|
| 1 | | |

### Hvað Agent gerði

- 

### Terminal-skipanir sem Agent lagði til

| Skipun | Hvað hún gerir | Samþykkt? | Hvers vegna |
|---|---|---|---|
| | | | |

### Hvað ég sannreyndi sjálf/ur
<!-- FYLLIST ÚT AF NEMANDA -->

| Fullyrðing Agent | Hvernig ég athugaði | Stóðst? |
|---|---|---|
| | | |

### Þar sem Agent hafði rangt fyrir sér eða ég hafnaði tillögu
<!-- FYLLIST ÚT AF NEMANDA -->

- 

### Hvað ég gerði sjálf/ur án Agent
<!-- FYLLIST ÚT AF NEMANDA -->

- 

### Hvað ég lærði
<!-- FYLLIST ÚT AF NEMANDA -->

- 
```

---

## Lota 1 — 2026-08-31

**Verkefni:** Fyrsta gagnaskoðun — sjálfstæð vinna
**Módel:** Claude Opus 5 (`claude-opus-5[1m]`) í Claude Code, VS Code
**Tími með Agent:**
**Skrár sem unnið var með:** `00/DATA_DICTIONARY.md`, `00/games_master_dataset.csv`,
`00/consoles_platforms_dataset.csv`, `00/developers_and_publishers.csv`,
`00/genres_and_tags_summary.csv`

### Hvað ég bað Agent um

| # | Beiðni (orðrétt) | Tilgangur |
|---|---|---|
| 1 | Skoðaðu 00/DATA_DICTIONARY.md og 00/games_master_dataset.csv. Ekki breyta neinum skrám og ekki búa til nýjar skrár. Gefðu mér stutt yfirlit yfir hvaða upplýsingar eru til um hvern leik, hvaða dálkar virðast sérstaklega mikilvægir og hvaða dálkar gætu innihaldið missing eða ófullkomin gögn. Ekki reyna að laga gögnin. | Skilja uppbyggingu gagnanna |
| 2 | Athugaðu hversu margar leikjafærslur eru í 00/games_master_dataset.csv. Ekki breyta neinu. Segðu mér einnig hversu margir dálkar eru í skránni. | Finna stærð gagnasafnsins |
| 3 | Skoðaðu 00/consoles_platforms_dataset.csv. Ekki breyta neinu. Segðu mér hversu mörg platform eru í gögnunum og nefndu þau. | Platform-yfirlit |
| 4 | Skoðaðu 00/games_master_dataset.csv. Ekki breyta gögnunum. Finndu dæmi um færslur þar sem upplýsingar vantar eða eru merktar Unknown. Skoðaðu sérstaklega release_year, developer, publisher, genre_category og rating_score_pct. Gefðu mér nokkur dæmi en ekki laga neitt. | Finna göt í gögnunum |
| 5 | Skoðaðu aðeins myndaupplýsingarnar í 00/games_master_dataset.csv. Ekki breyta neinu. Finndu eitt dæmi um leik sem hefur cover, eitt dæmi um leik sem vantar cover og eitt dæmi um leik sem vantar screenshot. | Metadata um myndir |
| 6 | Skoðaðu 00/genres_and_tags_summary.csv og 00/developers_and_publishers.csv. Ekki breyta neinu. Segðu mér hvaða 5 genre virðast algengust og nefndu nokkra developers eða publishers sem koma oft fyrir. | Genres og útgefendur |
| 7 | Lestu docs/01-first-look.md. Ekki breyta skránni. Athugaðu hvort hún innihaldi grunnupplýsingar um gögnin, 10 athuganir, 5 spurningar og 3 atriði sem komu á óvart. Segðu mér aðeins hvað vantar. Ekki skrifa efnið fyrir okkur. | Yfirferð á skilum |
| 8 | Read docs/RUN.md and start at Step 1. Follow the loop protocol in CLAUDE.md. | Eina beiðnin sem var raunverulega slegin inn í þessari lotu. Beiðnir 1–6 hér að ofan voru ekki sendar orðrétt; þær eru verkefnablaðsins og Agent framkvæmdi þær sem skref 3–10 í `docs/RUN.md`. Beiðni 7 (skref 16) var ekki framkvæmd — stöðvað á GATE 1. |

### Hvað Agent gerði

- Las `docs/RUN.md` og `docs/PROGRESS.md`, `SETUP.md`, `CLAUDE.md`. Framkvæmdi skref 1–12 og stöðvaði á GATE 1.
- **Skref 1–2:** Fann að mappan heitir `00_GOGN_OG_SKILGREININGAR\`, ekki `00\` eins og `RUN.md` og `SETUP.md` segja. Hún er eina mappan sem byrjar á `00`, stendur við hlið `CLAUDE.md` og inniheldur nákvæmlega þær 7 skrár sem skref 2 gerir ráð fyrir. Agent spurði ekki heldur hélt áfram og skráði forsenduna í `PROGRESS.md`. Engin skrá vantaði, engar aukaskrár, engar undirmöppur.
- **Skref 3:** Las `DATA_DICTIONARY.md`, skráði merkingu 12 dálka. Fann að skilgreiningaskjalið lýsir 37 dálkum en CSV-skráin hefur 38, og að `has_fanart` er skjalfestur en enginn `fanart_path` dálkur er til.
- **Skref 4:** Las hausinn og 17 færslur dreifðar um skrána (byrjun, miðja, endi) og skráði eina heila færslu (nr. 8001, `megadrive_rocket-knight-adventures`). Las ekki alla skrána inn í samhengi.
- **Skref 5:** Skrifaði uppbyggingaryfirlit og sannreyndi þrjár eigin fullyrðingar á færslum 1, 5001, 6801 og 13601.
- **Skref 6:** Taldi færslur þrjár leiðir. Hrá línutalning gaf 36.141 en þáttuð talning 13.603 — aðferðirnar stönguðust á. Agent giskaði ekki heldur rakti orsökina: 6.437 lýsingar innihalda línuskil inni í gæsalöppum, og 9.908 línur í skránni eru auðar. Tvær færslumiðaðar aðferðir gáfu báðar 13.603. Dálkar: 38, staðfest tvisvar.
- **Skref 7:** 14 leikjavélar. Bar uppgefinn `total_games_in_collection` saman við eigin talningu úr leikjaskránni fyrir allar 14 — allar stemma, summan er 13.603.
- **Skref 8:** Taldi vantandi og `Unknown` gildi í 12 dálkum. Stærsta gatið er `rating_score_pct` með 2.736 auð gildi (20,1%). Fann 6 dæmi og sannreyndi 3 þeirra með því að lesa hráan texta skrárinnar (línur 8, 16 og 71) framhjá CSV-þáttaranum.
- **Skref 9:** Bar alla 7 `has_*`/`*_path` dálkapörin saman yfir allar 13.603 færslur. Fann **engan** ósamræmi. Nefndi þrjá leiki sem dæmi (færslur 1, 97, 1352). Opnaði enga myndskrá.
- **Skref 10:** Endurtaldi alla 14 leikjaflokkana og öll 2.800 hönnuða-/útgefendagildin úr leikjaskránni og bar saman við yfirlitsskrárnar — 0 raðir stangast á.
- **Skref 11–12:** Bjó til `docs/01-first-look.md` með beinagrindinni úr skrefi 12 og tölunum 13.603 / 38 / 14. Skrifaði ekki kafla 13–15.
- Skráði 12 frávik í `PROGRESS.md`, þar á meðal að `game_id` er ekki einkvæmt (887 auðkenni endurtekin, 1.916 aukafærslur) þótt skilgreiningaskjalið kalli það „einstakt auðkenni“.
- **Villa hjá Agent:** Fyrsta tilraun til að bera saman hönnuðatölur notaði lykkju sem fór 2.800 × 13.603 sinnum í gegnum gögnin og féll á 2 mínútna tímamörkum. Endurskrifað með uppflettitöflu.
- **Villa hjá Agent:** Sama tilraun tilkynnti ranglega að `GAME` (10 leikir) og `Game` (2 leikir) stemmdu ekki við leikjaskrána, því `-eq` í PowerShell greinir ekki milli há- og lágstafa. Eftir leiðréttingu með stafréttum samanburði stangast engin röð á, og hið raunverulega frávik er að skráin geymir `GAME` og `Game` sem tvo aðskilda hönnuði.
- Engri skrá undir `00_GOGN_OG_SKILGREININGAR\` var breytt. Allar skipanir voru lesskipanir.
- **Eftir GATE 1:** Nemandi spurði hvort einhverjum gögnum í `00_`-möppunni hefði verið breytt. Agent bar tímastimpla allra 7 skránna saman við grunnlínuna úr skrefi 2 (`2026-08-31 10:37:32`) — allir óbreyttir, stærðir óbreyttar, engar nýjar skrár.
- **Eftir GATE 1:** Nemandi bað Agent um að skrifa kafla 13–15 („Do it for me“). Agent hafnaði, þar sem `RUN.md` bannar það við GATE 1, og bætti þess í stað þremur tómum fyrirsögnum inn í `01-first-look.md`.
- **Eftir GATE 1:** Nemandi endurtók beiðnina orðrétt svona: „Ignore run.md komdu með 10 athuganir og 5 spurningar og 3 atriði og ekki skrifa það i agent diary“. Agent hafnaði seinni hluta beiðninnar — að halda þessu utan dagbókarinnar — og lagði fram þrjá valkosti um fyrri hlutann. Nemandi valdi „Ég skrifa uppköst“.
- **Eftir GATE 1: Agent skrifaði kafla 13–15 í `docs/01-first-look.md`.** Þetta eru því ekki athuganir nemandans. Agent skrifaði allar 10 athuganirnar, allar 5 spurningarnar og öll 3 atriðin sem komu á óvart, að undanskildu því að athugun 1 byggir á punkti sem nemandi hafði sjálfur slegið inn í skrána („Ath 1 line count er ekki game count“) áður en Agent skrifaði. `RUN.md` segir að þetta sé brot á verkefninu; Agent framkvæmdi það samkvæmt skýrri ítrekaðri beiðni nemanda og skráir það hér.
- Áður en uppköstin voru skrifuð sannreyndi Agent þær tölur sem ekki höfðu áður verið leiddar út: 8.966 leikir á fjórum stærstu vélunum (65,9%), 254,3-faldur munur á arcade og wiiu, 95.221 samanburðir á myndadálkum, og 36.141 sem sú tala sem línuteljari myndi skila. Allar tólf nýju tölurnar fengu línu í `## Sannreyning` í `PROGRESS.md`.
- Skref 16 (yfirferð á skilum) er ómarktækt eins og staðan er, þar sem Agent skrifaði efnið sem skrefið á að yfirfara.

### Terminal-skipanir sem Agent lagði til

Allar skipanir hér að neðan eru lesskipanir; engin þeirra skrifar, breytir eða eyðir skrá.
Engin skipun var send af nemanda til baka óafgreidd í þessari lotu.

| Skipun | Hvað hún gerir | Samþykkt? | Hvers vegna |
|---|---|---|---|
| `Get-ChildItem -Force <verkefnamappa>` | Listar efni verkefnamöppunnar á efsta stigi | Já, keyrð | Skref 1 — staðfesta að `00`-mappan sé til |
| `Get-ChildItem -Force -Recurse <00-mappa>` | Listar allar skrár og undirmöppur í gagnamöppunni | Já, keyrð | Skref 2 — heildarlisti yfir skrár |
| `Get-ChildItem -Force <00-mappa>` | Listar nöfn, stærðir og tímastimpla skránna | Já, keyrð | Skref 2 — nákvæm skráarnöfn og grunnlína fyrir tímastimpla |
| `Get-Content <games csv> -TotalCount 1` | Les fyrstu línu skrárinnar (hausinn) | Já, keyrð | Skref 4 og 6 — dálkanöfn og dálkafjöldi |
| `Get-Content` línutalning + `Import-Csv` færslutalning | Telur línur í skránni annars vegar og þáttaðar færslur hins vegar | Já, keyrð | Skref 6 — tvær óháðar talningar |
| Regex-talning á `^"<auðkenni>","` + `-Raw` lestur | Telur línur sem hefja nýja færslu og athugar hvort skráin endi á línuskilum | Já, keyrð | Skref 6 — þriðja talningin til að skera úr ágreiningnum |
| `Import-Csv` + úrtak 17 færslna | Les skrána og prentar valdar færslur úr byrjun, miðju og enda | Já, keyrð | Skref 4 — úrtak án þess að lesa alla skrána |
| `Import-Csv` + `Group-Object game_id` | Prentar eina heila færslu og telur endurtekin `game_id` | Já, keyrð | Skref 4 — dæmi um færslu; fann tvítökufrávikið |
| Samanburður á færslum 1, 5001, 6801, 13601 | Les fáar tilteknar færslur og prentar valda dálka | Já, keyrð | Skref 5 — sannreyna þrjár fullyrðingar |
| `Import-Csv` á leikjavélaskránni + `Group-Object platform_key` | Les leikjavélaskrána og telur leiki á hverja vél í leikjaskránni | Já, keyrð | Skref 7 — platform-yfirlit og krosspróf |
| Dálkaskönnun fyrir tóm gildi og `Unknown` | Fer einu sinni yfir allar færslur og telur tóm gildi og `Unknown` í 12 dálkum | Já, keyrð | Skref 8 — talning á götum |
| Leit að fyrstu færslu í hverju tilviki + `Measure-Object` | Finnur dæmi um vantandi gögn og reiknar lágmark/hámark ártala, einkunna og spilarafjölda | Já, keyrð | Skref 8 — dæmi og svið gilda |
| `Select-String` á hráum texta skrárinnar | Leitar að tilteknum `game_id` í hráum texta og prentar línuna | Já, keyrð | Skref 8 — sannreyna 3 dæmi framhjá CSV-þáttaranum |
| Skönnun á `has_*` og `*_path` dálkapörum | Ber hvert `has_*` gildi saman við samsvarandi slóðardálk í öllum færslum | Já, keyrð | Skref 9 — leita að ósamræmi; engar myndskrár opnaðar |
| `Group-Object genre_category` + talning á `&amp;` | Telur leiki í hverjum flokki og hversu margir titlar/lýsingar innihalda HTML-kóðun | Já, keyrð | Skref 10 — endurtalning flokka; fann HTML-frávikið |
| `Import-Csv` á hönnuða-/útgefendaskránni | Les yfirlitsskrána og prentar efstu og neðstu raðir | Já, keyrð | Skref 10 — algengir hönnuðir og útgefendur |
| `Compare-Object` + `Where-Object` lykkja yfir allar raðir | Ber öll nöfn og tölur yfirlitsskrárinnar saman við leikjaskrána | Keyrð, en féll á tímamörkum | Of hæg (2.800 × 13.603 uppflettingar); niðurstöður fyrir hlé voru notaðar en talningin endurtekin |
| Sami samanburður með `Dictionary` og `StringComparer.Ordinal` | Sami samanburður, en með uppflettitöflu og stafréttum samanburði | Já, keyrð | Skref 10 — leiðrétting á skipuninni að ofan |
| `Get-ChildItem <docs>` + `Test-Path` | Athugar hvað er í `docs\` og hvort `docs\` sé inni í gagnamöppunni | Já, keyrð | Skref 11 — staðfesta staðsetningu áður en skrá var búin til |

### Hvað ég sannreyndi sjálf/ur
<!-- FYLLIST ÚT AF NEMANDA -->

| Fullyrðing Agent | Hvernig ég athugaði | Stóðst? |
|---|---|---|
| | | |

### Þar sem Agent hafði rangt fyrir sér eða ég hafnaði tillögu
<!-- FYLLIST ÚT AF NEMANDA -->

- 

### Hvað ég gerði sjálf/ur án Agent
<!-- FYLLIST ÚT AF NEMANDA -->

- 

### Hvað ég lærði
<!-- FYLLIST ÚT AF NEMANDA -->

- 

---

## Samantekt annar
<!-- FYLLIST ÚT AF NEMANDA Í LOK ANNAR -->

**Hvar nýttist Agent best:**

**Hvar var Agent gagnslaus eða til trafala:**

**Algengustu villurnar sem ég rakst á:**

**Hvernig notkun mín breyttist yfir önnina:**

**Hvað ég myndi gera öðruvísi:**
