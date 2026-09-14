# Hugmyndir að verkefni

> **Áfangi:** Vefkerfi 1 &nbsp;·&nbsp; **Verkefni 3** — Hugmyndavinna
> **Hópur:** _(nöfn)_
> **Gagnasafn:** 13.603 leikjafærslur &nbsp;·&nbsp; 38 dálkar &nbsp;·&nbsp; 14 leikjavélar

---

## Yfirlit

| # | Hugmynd | Í einni setningu | Staða |
|:-:|:--|:--|:--|
| 1 | **Leitarsíðan** | Leita, sía og skoða allar upplýsingar og myndir um leik | |
| 2 | **Tímalínan** | Velja vél og draga slider frá 1971 til 2026 | |
| 3 | **Letterboxd fyrir leiki** | Stofna reikning, gefa einkunn og skrifa umsagnir | **← VALIN** |
| 4 | **Guess the game** | Giska á leikinn út frá lýsingu eða myndum | |

---

## Fimm möguleikar sem gögnin skapa

_Skref 1 — allar tölur mældar úr `games_master_dataset.csv`._

1. **Umfangið sjálft.** 13.603 leikir á 14 vélum með 38 dálkum. Nógu margar víddir til að
   sía, raða og fletta á marga ólíka vegu.
2. **Spilarafjöldi er gallalaus.** `players_min` og `players_max` eru einu efnisdálkarnir
   með **núll** vantandi gildi af 13.603. Multiplayer-sía væri alltaf rétt.
3. **Textinn er til staðar.** 13.418 leikir hafa lýsingu og 12.478 þeirra eru 150 stafir
   eða lengri. Nóg fyrir leit, leikjasíður og textabyggða eiginleika.
4. **Tímaspönnin.** Útgáfuár ná frá 1971 til 2026 og 13.173 leikir hafa ártal. Tímalína
   eða söguleg framsetning er raunhæf.
5. **Gatið í einkunnunum.** 2.736 leiki (20,1%) vantar einkunn með öllu. Það er ekki bara
   galli — það er pláss sem notendur geta fyllt, og þar með ástæða fyrir kerfi sem safnar
   einkunnum frá fólki.

---

## Mögulegir notendur

_Skref 2 — „allir“ er ekki notandi._

| Notandi | Hvað hann vill |
|:--|:--|
| **Umsagnaskrifarinn** | Skrifa umsagnir og gefa einkunnir, sjá hvað aðrir skrifuðu |
| Retro-safnarinn | Halda utan um hvað hann á og hefur spilað |
| Sá sem veit ekki hvað hann á að spila | Velja næsta leik út frá því sem öðrum fannst |
| Vinir að leita að multiplayer-leik | Finna leik fyrir tiltekinn fjölda spilara |
| Sá sem vill skoða tölvuleikjasögu | Skoða þróun milli kynslóða og áratuga |

**Valinn aðalnotandi:** Umsagnaskrifarinn.

---

# Hugmynd 1 — Leitarsíðan

**Vefsíða sem þú getur leitað upp leikjum, séð myndir af leiknum og fanart, og allar upplýsingar um hann.**

### Notandi
Einhver sem vill leita að leikjum og upplýsingum um þá.

### Vandamál
Rosalega mikið af gögnum til að leita í gegnum.

### Gögn sem hugmyndin notar
Öll gögn sem við fáum.

### Aðalflæði
1. Þú opnar vefsíðuna.
2. Þú notar síu og search bar til að leita að leiknum sem þú vilt.
3. Þegar þú finnur hann kemur „thumbnail“ fyrir hann.
4. Þú opnar síðu um leikinn, lest allt um hann og sérð allar myndir.



### Af hverju gæti þetta verið gott verkefni?
> _Óútfyllt._

### Helstu áhættur
- Aðrir hópar hafa þessa hugmynd.

---

# Hugmynd 2 — Tímalínan

**Vefsíða þar sem þú velur hvaða console/platform þú vilt skoða og hefur slider sem þú getur fært til, til þess að sjá hvaða leikir voru á þessum tíma.**

### Notandi
Einhver sem vill leita að leikjum og upplýsingum frá því tímabili.

### Vandamál
Ónákvæmar dagsetningar

### Gögn sem hugmyndin notar
Allir leikir sem hafa upplýsingar um hvenær þeir voru gefnir út.

### Aðalflæði
1. Þú opnar vefsíðuna.
2. Þar er lína sem þú getur dregið milli 1971 og 2026.
3. Þú sérð leiki sem voru gefnir út á milli punktanna.



### Af hverju gæti þetta verið gott verkefni?
> _Óútfyllt._

### Helstu áhættur
- Röng dagsetning á leik.

---

# Hugmynd 3 — Letterboxd fyrir leiki

**Letterboxd, nema fyrir tölvuleiki.**

### Notandi
Tölvuleikjafan eða einhver sem hefur mikinn áhuga á leikjum.

### Vandamál
Þarf bakenda — fólk þarf að geta stofnað reikning, gefið einkunn og skrifað comment.

### Gögn sem hugmyndin notar
Allir leikir sem hafa:

| Dálkur | Hvað það er |
|:--|:--|
| `title` | Titill leiksins |
| `cover_image_path` | Mynd |
| `developer` | Hann sem bjó til leikinn |
| `release_date` | Dagsetning |
| `description` | Lýsing |
| `genre_category` | Hvernig leikur þetta er |

### Aðalflæði
1. Þú ferð inn á síðuna.
2. Þú leitar að leik sem þú hefur spilað og vilt skrifa um.
3. Þú opnar síðu leiksins.
4. Þú gefur einkunn og skrifar comment eða umsögn.



### Af hverju gæti þetta verið gott verkefni?
> _Óútfyllt._

### Helstu áhættur
- Ekki nóg af upplýsingum um leikinn.
- Ekki nóg af feedback frá notendum.

---

# Hugmynd 4 — Guess the game

**Leikur þar sem þú giskar á hvaða leik er verið að lýsa eða sýna.**

### Notandi
Hver sem er sem vill spila þennan leik, t.d. með vinum.

### Vandamál
Hver leikur sem er í honum þarf að hafa mynd, lýsingu, dagsetningu sem hann var gefinn út, útgefanda og genre. Þú gætir rage-quittað.

### Gögn sem hugmyndin notar
Allir leikir sem hafa þessi gögn sem við lýstum fyrir ofan.

### Aðalflæði
1. Þú ferð inn í leikinn.
2. Þú færð annaðhvort stutta lýsingu af leiknum eða einhverjar myndir úr honum.
3. Þú átt að giska hvaða leikur þetta er.

-

### Af hverju gæti þetta verið gott verkefni?
> _Óútfyllt._

### Helstu áhættur
- Finnur ekki leikinn.
- Léleg lýsing á leiknum.
- Vondar myndir.

---

# Samanburður hugmynda

_Skref 8 — gefið hverri hugmynd einkunn frá 1 til 5._

| Atriði | Hugmynd 1 | Hugmynd 2 | Hugmynd 3 | Hugmynd 4 |
|:--|:-:|:-:|:-:|:-:|
| Gagnleg fyrir notanda | 3 | 3 | **5** | 2 |
| Nýtir gögnin vel | 4 | 4 | **4** | 3 |
| Áhugaverð | 2 | 5 | **4** | 5 |
| Raunhæf | 5 | 3 | **3** | 4 |
| Svigrúm til að þróa áfram | 2 | 3 | **5** | 2 |
| **Samtals** | **16** | **18** | **21** | **16** |

---

# Feedback frá öðrum hópi

_Skref 9._

**Hvaða hugmynd myndir þú helst vilja prófa sem notandi?**
> _Óútfyllt._

**Hvaða hugmynd er skýrust?**
> _Óútfyllt._

**Hvaða vandamál eða áhættu sérðu sem höfundarnir hafa mögulega ekki séð?**
> _Óútfyllt._

---

# Valin hugmynd

## Við veljum

### Hugmynd 3 — Letterboxd fyrir leiki

## Ástæða

Það hljómar skemmtilegt og er eina raunverulega vefkerfið.

---

# Staða á lokatékklista

| | Atriði | Athugasemd |
|:-:|:--|:--|
| [~] | Við skoðuðum niðurstöður úr verkefnum 1 og 2 | `02-data-profile.md` er ekki til — Verkefni 2 óunnið |
| [x] | Við skráðum 5 möguleika sem gögnin skapa | Sjá að ofan |
| [x] | Við skilgreindum nokkra mögulega notendur | 5 skráðir, einn valinn |
| [x] | Við bjuggum til þrjár ólíkar hugmyndir | Við erum með fjórar |
| [ ] | Hver hugmynd hefur notanda, vandamál, gögn, flæði og áhættu | „Af hverju gott verkefni“ vantar í allar fjórar |
| [x] | Við bárum hugmyndirnar saman | Hugmynd 3 hæst með 21 af 25 |
| [ ] | Við fengum feedback frá öðrum hópi | Krefst annars hóps |
| [x] | Við völdum eina hugmynd og rökstuddum valið | Rökin mega vera sterkari — sjá Skref 10 |
| [x] | Við skrifuðum skýra verkefnasetningu | Í `product-brief.md` |
| [x] | `product-brief.md` hefur MVP, Should have, Could have og Out of scope | |
| [ ] | Við skráðum eina Agent tillögu sem við samþykktum og eina sem við höfnuðum | Tillögurnar eru skráðar, rökin ykkar vantar |
| [ ] | Báðir hópmeðlimir geta útskýrt verkefnið með eigin orðum | Ykkar að staðfesta |
