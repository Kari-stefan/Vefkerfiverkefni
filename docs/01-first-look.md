# Fyrsta skoðun á gagnasafninu

## Hópur
Nöfn:

## Gagnaskrár sem við skoðuðum
- `00_GOGN_OG_SKILGREININGAR/DATA_DICTIONARY.md` — skilgreiningar á öllum dálkum og töflum
- `00_GOGN_OG_SKILGREININGAR/games_master_dataset.csv` — aðaltafla leikjanna
- `00_GOGN_OG_SKILGREININGAR/consoles_platforms_dataset.csv` — tafla yfir leikjavélarnar
- `00_GOGN_OG_SKILGREININGAR/genres_and_tags_summary.csv` — yfirlit yfir leikjaflokka
- `00_GOGN_OG_SKILGREININGAR/developers_and_publishers.csv` — yfirlit yfir hönnuði og útgefendur

Skrárnar `games_master_dataset.json` og `consoles_platforms_dataset.json` voru ekki
notaðar; þær innihalda sömu gögn og CSV-skrárnar.

## Stærð gagnasafnsins
Fjöldi leikjafærslna: 13.603
Fjöldi dálka: 38
Fjöldi platforma: 14

## 10 athuganir um gögnin

1. **Línufjöldi er ekki leikjafjöldi.** Skráin `games_master_dataset.csv` er 36.142 línur
   en inniheldur aðeins 13.603 leikjafærslur. Ástæðan er sú að 6.437 lýsingar innihalda
   línuskil inni í gæsalöppum, sem býr meðal annars til 9.908 auðar línur. Sá sem telur
   skrána með línuteljara fær 36.141 leik — næstum þrefalt of mikið.

2. **`game_id` er ekki einkvæmt.** Skilgreiningaskjalið kallar dálkinn „einstakt auðkenni
   leiks“, en 13.603 færslur innihalda aðeins 11.687 ólík auðkenni. 887 auðkenni koma oftar
   en einu sinni fyrir og 1.916 færslur eru umfram. Verst er
   `megadrive_nba-action-95-starring-david-robinson` sem kemur 26 sinnum fyrir.

3. **Einkunn vantar fyrir fimmtung safnsins.** `rating_score_pct` og `rating_stars_5` eru
   tóm fyrir 2.736 leiki, eða 20,1%. Þetta er langstærsta gatið í gögnunum og hefur bein
   áhrif á hvers kyns röðun eftir einkunn.

4. **Tvær ólíkar aðferðir eru notaðar til að merkja að gögn vanti.** Dagsetningar, einkunnir
   og lýsingar eru skildar eftir tómar, en hönnuður, útgefandi og flokkur fá strenginn
   `Unknown`. Enginn dálkur blandar aðferðunum saman, svo leit að tómum gildum einum saman
   finnur ekki nema hluta af götunum.

5. **Taflan er flöt og afnormalíseruð.** Upplýsingar um leikjavélina eru endurteknar á
   hverri einustu línu í fimm dálkum (`platform_key`, `platform_name`, `platform_short`,
   `platform_company`, `platform_generation`), þótt vélarnar séu aðeins 14 og til sé
   sérstök tafla um þær.

6. **Myndalýsigögnin eru fullkomlega samkvæm.** Í öllum sjö pörum af `has_*` og `*_path` er
   engin færsla þar sem gildið er `true` en slóðin tóm, og engin þar sem gildið er `false`
   en slóð er samt til staðar. Öll gildin eru nákvæmlega `true` eða `false`.

7. **`has_fanart` vísar hvergi.** Dálkurinn segir `true` fyrir 9.008 leiki, en það er
   enginn `fanart_path` dálkur í skránni. Þetta er eini myndadálkurinn án samsvarandi
   slóðar, og því eina myndaupplýsingin sem ekki er hægt að nota.

8. **Dreifing leikja milli véla er mjög ójöfn.** arcade hefur 3.052 leiki en wiiu aðeins 12
   — meira en 250-faldur munur. Fjórar efstu vélarnar (arcade, megadrive, nes, snes) geyma
   8.966 leiki, eða um tvo þriðju hluta safnsins.

9. **Yfirlitsskráin yfir hönnuði og útgefendur sleppir `Unknown`.** Leikjafjöldinn í
   `developers_and_publishers.csv` leggst saman í 13.090 fyrir hönnuði og 13.417 fyrir
   útgefendur, ekki 13.603. Mismunurinn er nákvæmlega þeir 513 og 186 leikir sem merktir eru
   `Unknown` í aðaltöflunni.

10. **Texti er geymdur HTML-kóðaður.** 253 titlar og 328 lýsingar innihalda `&amp;` í stað
    `&`, til dæmis `Flip &amp; Flop`. Þessi texti birtist sem `&amp;` á vefsíðu nema hann sé
    afkóðaður fyrst.

## 5 spurningar sem við viljum rannsaka

1. **Eru tvítökin á `game_id` raunveruleg afrit eða ólíkar útgáfur sama leiks?**
   Mælanlegt með því að bera saman `rom_filename`, `release_date`, `rom_size_mb` og
   `rating_score_pct` innan hvers hóps sem deilir auðkenni. Ef reitirnir eru eins er um
   hrein afrit að ræða; ef þeir eru ólíkir vantar útgáfudálk í gagnalíkanið.

2. **Hvaða leikjavélar eru verst skjalfestar?**
   Mælanlegt með því að reikna hlutfall vantandi `rating_score_pct`, `developer` og
   `description` fyrir hvern `platform_key`. Það segir okkur hvaða hluta safnsins má treysta
   í vefkerfi og hvar þarf að gera ráð fyrir varaefni.

3. **Batnar skjölunin eftir því sem leikirnir eru yngri?**
   Mælanlegt með hlutfalli vantandi gilda á hvern `decade`. Ef elstu áratugirnir eru verst
   skjalfestir hefur það áhrif á hvernig tímalína eða „elstu leikirnir“ síða myndi líta út.

4. **Fylgjast myndir og lýsigögn að?**
   Mælanlegt með krosstöflu á `has_cover_image` á móti því hvort `rating_score_pct` sé tómt.
   Ef leikir án kápumyndar eru líka einkunnalausir er um heila hópa illa skjalfettra leikja
   að ræða frekar en tilviljanakennd göt.

5. **Hversu miklu ræður lítill hópur útgefenda?**
   Mælanlegt með uppsafnaðri hlutdeild útgefenda raðaðri eftir `game_count` í
   `developers_and_publishers.csv`. Við sjáum þegar að SEGA gefur út 1.550 leiki meðan 446
   útgefendur eiga aðeins einn; spurningin er hvar mörkin liggja fyrir nothæfa síu á vefsíðu.

## 3 atriði sem komu okkur á óvart

1. **Að `game_id` skyldi ekki vera einkvæmt.** Skilgreiningaskjalið segir berum orðum
   „einstakt auðkenni leiks“, og þetta er einmitt dálkurinn sem eðlilegast væri að nota sem
   lykil í vefkerfi. Að 1.916 færslur brjóti þá forsendu þýðir að ekki er hægt að treysta
   skjölun gagnanna í blindni — það þarf að prófa hana.

2. **Að myndagögnin væru gallalaus en textagögnin götótt.** Við bjuggumst við hinu
   gagnstæða, þar sem myndir eru yfirleitt það sem vantar. 95.221 samanburðir á `has_*` og
   `*_path` gáfu ekki eitt einasta ósamræmi, á meðan fimmtung leikjanna vantar einkunn og
   513 vantar hönnuð.

3. **Að yfirlitsskrárnar væru ekki sammála aðaltöflunni.** `developers_and_publishers.csv`
   sleppir `Unknown` þegjandi, svo summurnar stemma ekki við 13.603, og skráin geymir `GAME`
   (10 leikir) og `Game` (2 leikir) sem tvo aðskilda hönnuði. Yfirlitsskrá sem lítur út
   fyrir að vera einföld reyndist þurfa jafn mikla varúð og aðaltaflan sjálf.
