# Product brief

> **Áfangi:** Vefkerfi 1 &nbsp;·&nbsp; **Verkefni 3** — Hugmyndavinna
> **Hópur:** _(nöfn)_
> **Valin hugmynd:** Hugmynd 3 — Letterboxd fyrir leiki

---

## Nafn verkefnis

**Vinnuheiti:** _(óútfyllt — ykkar að velja)_

---

## Tilgangur

Vefkerfi þar sem notendur gefa retro-leikjum einkunn og skrifa umsagnir, svo hægt sé
að velja úr 13.603 leikjum eftir því hvað fólki fannst raunverulega um þá.

---

## Notandi

**Umsagnaskrifarinn.**

Einhver sem spilar retro-leiki og vill skrifa umsagnir, gefa einkunnir og sjá hvað aðrir
skrifuðu. Hann kemur á síðuna til að leggja eitthvað til, ekki bara til að fletta upp.

Þetta er ekki „allir“: sá sem vill aðeins finna upplýsingar um leik er ekki aðalnotandinn,
og kerfið þarf ekki að þjóna honum sérstaklega.

---

## Vandamál

**13.603 leikir og ekkert til að velja eftir.**

Safnið er of stórt til að fletta í gegnum og það er engin leið að vita hvað er þess virði
að spila. Einkunnirnar sem fylgja gögnunum leysa það ekki:

| Staðreynd úr gögnunum | Tala |
|:--|--:|
| Leikir alls | 13.603 |
| Leikir **án einkunnar** | 2.736 (20,1%) |
| Leikir án lýsingar | 185 |
| Leikir með `genre_category = Unknown` | 298 |

Einkunnirnar sem eru til koma úr gagnasafninu án heimildar — enginn veit hver gaf þær eða
af hverju. Umsagnir frá fólki leysa bæði vandamálin: þær fylla götin og þær eru rekjanlegar.

---

## Gögn sem við ætlum að nota

### Úr `games_master_dataset.csv` (lesin, aldrei skrifuð)

| Dálkur | Til hvers | Þekjun |
|:--|:--|--:|
| `game_id` | Auðkenni leiks — **sjá áhættu 1** | 13.603 |
| `title` | Titill, leit og fyrirsögn | 13.603 |
| `platform_key`, `platform_name` | Sía og leikjasíða | 13.603 |
| `release_year` | Sía, röðun, leikjasíða | 13.173 |
| `developer`, `publisher` | Leikjasíða | 13.090 / 13.417 |
| `genre_category` | Sía og leikjasíða | 13.305 |
| `description` | Leikjasíða | 13.418 |
| `rating_score_pct` | Sýnt við hlið einkunna notenda | 10.867 |
| `cover_image_path` | Thumbnail — **sjá áhættu 3** | 13.269 |

### Úr `consoles_platforms_dataset.csv`

`platform_key`, `platform_name`, `company`, `generation` — fyrir síuna og heiti véla.

### Töflur sem við búum til sjálf

`reviews` (einkunn + texti + hvaða leikur), og síðar `users`.

---

## Aðalflæði notanda

1. Notandi opnar síðuna og leitar að leik, eða síar eftir vél og flokki.
2. Hann finnur leikinn í niðurstöðum og opnar leikjasíðuna.
3. Á leikjasíðunni sér hann upplýsingar úr gögnunum og umsagnir sem þegar eru til.
4. Hann gefur einkunn 1–5 og skrifar stutta umsögn, sem birtist strax á síðunni.

---

## MVP

Fjögur atriði sem **verða** að virka:

1. **Leita að leik** og sía eftir vél og flokki.
2. **Leikjasíða** með upplýsingum úr gögnunum.
3. **Gefa einkunn** 1–5.
4. **Skrifa umsögn** í texta sem birtist á leikjasíðunni.

Engir notendareikningar í MVP — einn harðkóðaður notandi. Auðkenning er heil vika sem
færi ekki í kjarnann.

---

## Should have

- Notendareikningar og innskráning.
- Umsagnir tengdar notanda, með nafni hans.
- Síða með „mínar umsagnir“.
- Röðun leikja eftir meðaleinkunn notenda.

---

## Could have

- Comment undir umsögnum annarra.
- Follow, vinir og straumur.
- Tímalína eftir útgáfuári (hugmynd 2).
- „Guess the game“ sem aukaleikur (hugmynd 4).
- Listar og wishlist.

---

## Out of scope

Það sem við ætlum **viljandi ekki** að byggja:

- **Mobile app** — vefur í vafra, ekkert app.
- **Recommendation AI** — engar sjálfvirkar tillögur reiknaðar af vél.

> **Athugið:** comment-kerfi, follow/feed og fanart voru **ekki** sett út af borðinu.
> Þau eru því í Could have og geta þanist út. Ef þið viljið verja ykkur gegn því þurfa
> þau að færast hingað niður.

---

## Helstu áhættur

### 1. `game_id` er ekki einkvæmt — blokkar umsagnir

Gagnasafnið hefur 13.603 færslur en aðeins **11.687 ólík `game_id`**. 887 auðkenni
endurtaka sig og 1.916 færslur eru umfram. `Ultracore` kemur fjórum sinnum fyrir,
`megadrive_nba-action-95-starring-david-robinson` tuttugu og sex sinnum.

Umsögn verður að festast við eina tiltekna færslu. Þetta þarf að leysa **áður en** fyrsta
umsögnin er skrifuð í gagnagrunn — annars veit enginn við hvað hún á.

### 2. Cold start — engar umsagnir nema okkar eigin

Umsagnasíða án notenda hefur engar umsagnir. Í skólaverkefni verða þær eingöngu okkar og
kannski hins hópsins. Síðan verður að vera nothæf og líta heil út með **núll umsögnum**.

### 3. Myndskrárnar fylgja ekki gagnapakkanum

`cover_image_path` vísar á `covers/…` en gagnamappan inniheldur sjö skrár og enga
myndamöppu. 13.269 leikir eiga skráða kápuslóð sem bendir á skrá sem við höfum ekki.
Þetta þarf að staðfesta strax; annars þarf placeholder-myndir frá byrjun.

---

## Verkefnasetning

> Við erum að búa til **umsagnavef fyrir retro-leiki** fyrir **fólk sem spilar gamla leiki
> og vill skrifa um þá**, sem hjálpar því að **finna hvað er þess virði að spila úr 13.603
> leikjum**, með því að **leyfa því að gefa einkunn og skrifa umsagnir sem aðrir sjá**.

---

## Feedback frá Agent

### Tillaga sem við samþykktum

Að taka notendareikninga **út úr MVP** og hafa einn harðkóðaðan notanda á meðan kjarninn
er byggður. Agent benti á að auðkenning væri klassísk ástæða þess að verkefni klárast ekki,
og að leit, leikjasíða, einkunn og umsögn væru það sem gerir kerfið að því sem við lýsum.

**Af hverju við samþykktum hana:**
> _(óútfyllt — ykkar rök)_

### Tillaga sem við höfnuðum

Að setja **fanart og full myndasöfn út af borðinu** (out of scope). Agent lagði það til af
því að `has_fanart` hefur enga slóð í gögnunum og myndskrárnar fylgja ekki gagnapakkanum.
Við völdum að hafa myndir áfram inni.

**Af hverju við höfnuðum henni:**
> _(óútfyllt — ykkar rök)_
