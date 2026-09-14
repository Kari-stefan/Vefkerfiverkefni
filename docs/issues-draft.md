# Issues — uppkast fyrir GitHub Project

> **Athugið:** þessi issues eru komin á GitHub sem **#3–#15**. Númerin hér að neðan (#1–#13)
> eru uppkastsnúmer og eiga **ekki** við GitHub. Rétt vörpun:
> #1→#3, #2→#4, #3→#5, #4→#6, #5→#7, #6→#8, #7→#10, #8→#11, #9→#13, #10→#15, #11→#14,
> #12→#12, #13→#9. Traceability-taflan í `requirements.md` notar GitHub-númerin.

Þrettán issues leidd af `requirements.md`. Hvert er nógu lítið til að hægt sé að klára það,
yfirfara og prófa sérstaklega. Owner er skilinn eftir auður — Skref 12 segir að báðir
hópmeðlimir eigi að eiga sýnilega vinnu.

Uppástunga um Ready (aðeins þrjú, Skref 13): **#1, #2, #3**.

---

## #1 — Ákveða lykil fyrir leik og leysa tvítök á game_id
**Requirement:** Open questions · blokkar US-01, US-04
**Size:** M · **Status:** Ready

Gagnasafnið hefur 13.603 færslur en aðeins 11.687 ólík `game_id`. 887 auðkenni endurtaka sig
og 1.916 færslur eru umfram. Ákveða hvort tvítökin séu afrit sem á að sameina, eða ólíkar
útgáfur sem eiga að lifa hver í sínu lagi, og skilgreina þann lykil sem umsagnir festast við.

**Acceptance criteria**
- [ ] PASS/FAIL: Skjalfest ákvörðun liggur fyrir um hvað telst einn leikur.
- [ ] PASS/FAIL: Hver leikur í gagnagrunninum hefur lykil sem er einkvæmur.
- [ ] PASS/FAIL: `Ultracore` skilar þeim fjölda færslna sem ákvörðunin segir til um, hvorki fleiri né færri.

---

## #2 — Flytja CSV inn í gagnagrunn
**Requirement:** FR-01, FR-03 · **Size:** M · **Status:** Ready

Lesa `games_master_dataset.csv` og `consoles_platforms_dataset.csv` inn í gagnagrunn. Athuga
að 6.437 lýsingar innihalda línuskil inni í gæsalöppum — línubundinn lestur skilar röngum
fjölda.

**Acceptance criteria**
    - [ ] PASS/FAIL: Gagnagrunnurinn inniheldur þann fjölda leikja sem ákvörðun #1 segir til um.
    - [ ] PASS/FAIL: Allar 14 leikjavélarnar eru til í gagnagrunninum.
    - [ ] PASS/FAIL: Leikur með fjölmálsgreina lýsingu skilar sér heill, ekki afskorinn.

---

## #3 — Leita að leik eftir titli
**Requirement:** FR-01, US-02 · **Size:** S · **Status:** Ready

Leitarreitur sem skilar leikjum þar sem titill inniheldur innslegna strenginn.

**Acceptance criteria**
- [ ] PASS/FAIL: Hluti úr titli skilar öllum leikjum sem innihalda þann streng (AC-02.1).
- [ ] PASS/FAIL: Hver niðurstaða sýnir titil, vél og útgáfuár (AC-02.3).
- [ ] PASS/FAIL: Leit er ónæm fyrir há- og lágstöfum.

---

## #4 — Sía leiki eftir vél
**Requirement:** FR-01, US-02 · **Size:** M

Fellilisti með þeim 14 vélum sem til eru, sóttur úr `consoles_platforms_dataset.csv`.

**Acceptance criteria**
- [ ] PASS/FAIL: Notandi getur valið eina vél.
- [ ] PASS/FAIL: Niðurstöður sýna aðeins leiki fyrir valda vél.
- [ ] PASS/FAIL: Val á `wiiu` skilar 12 leikjum, val á `arcade` skilar 3.052.

---

## #5 — Sía leiki eftir flokki
**Requirement:** FR-01, US-02 · **Size:** S

Fellilisti með þeim 14 flokkum sem til eru.

**Acceptance criteria**
- [ ] PASS/FAIL: Notandi getur valið einn flokk.
- [ ] PASS/FAIL: Niðurstöður sýna aðeins leiki í völdum flokki.
- [ ] PASS/FAIL: `Unknown` birtist ekki sem valkostur, eða er merktur sem „óflokkað“.

---

## #6 — Sameina leitarstreng og síur í eina fyrirspurn
**Requirement:** FR-01 · **Size:** M

Leit og báðar síurnar virka saman, ekki hvor í sínu lagi.

**Acceptance criteria**
- [ ] PASS/FAIL: Titilleit ásamt vél og flokki skilar aðeins leikjum sem uppfylla öll þrjú skilyrðin (AC-02.2).
- [ ] PASS/FAIL: Að hreinsa eina síu breytir niðurstöðum án þess að hinar tapist.

---

## #7 — Skilaboð þegar engin niðurstaða finnst
**Requirement:** US-02, AC-02.4 · **Size:** S

**Acceptance criteria**
- [ ] PASS/FAIL: Leit sem passar við ekkert sýnir skýr skilaboð, ekki auðan lista.
- [ ] PASS/FAIL: Skilaboðin nefna hvað var leitað að.

---

## #8 — Leikjasíða með reitum úr gögnunum
**Requirement:** FR-03, US-03 · **Size:** M

Síða fyrir einn leik með titli, vél, útgáfuári, hönnuði, útgefanda, flokki og lýsingu.

**Acceptance criteria**
        - [ ] PASS/FAIL: Allir sjö reitirnir birtast fyrir leik sem hefur þá alla (AC-03.1).
        - [ ] PASS/FAIL: Hægt er að opna leikjasíðu beint úr leitarniðurstöðum.

---

## #9 — Staðgengilstexti fyrir tóma reiti
**Requirement:** FR-03, AC-03.4 · **Size:** S

185 leiki vantar lýsingu, 430 vantar ártal, 513 hafa `Unknown` hönnuð.

**Acceptance criteria**
- [ ] PASS/FAIL: Leikur án lýsingar sýnir skýran texta í stað auðs svæðis.
- [ ] PASS/FAIL: Reitur með gildinu `Unknown` birtist ekki sem orðið „Unknown“ fyrir notanda.

---

## #10 — Kápumynd með staðgengli
**Requirement:** FR-03 · **Size:** S

334 leiki vantar kápumynd, og myndskrárnar fylgja ekki gagnapakkanum.

**Acceptance criteria**
- [ ] PASS/FAIL: Leikur með kápumynd sýnir hana.
- [ ] PASS/FAIL: Leikur án kápumyndar sýnir staðgengilsmynd, ekki brotið myndtákn.
- [ ] PASS/FAIL: Slóð sem bendir á skrá sem er ekki til fellur líka á staðgengilinn.

---

## #11 — Gefa leik einkunn 1–5
**Requirement:** FR-02, US-04 · **Size:** M

**Acceptance criteria**
- [ ] PASS/FAIL: Notandi getur valið heiltölu 1–5 og hún vistast (AC-04.1).
- [ ] PASS/FAIL: Ný einkunn frá sama notanda kemur í stað þeirrar fyrri, býr ekki til aðra.
- [ ] PASS/FAIL: Einkunnin lifir af endurræsingu kerfisins (FR-09).

---

## #12 — Skrifa umsögn og birta hana
**Requirement:** FR-02, FR-04, US-03, US-04 · **Size:** M

**Acceptance criteria**
- [ ] PASS/FAIL: Vistuð umsögn birtist á leikjasíðunni strax (AC-04.2).
- [ ] PASS/FAIL: Umsögn undir lágmarkslengd er ekki vistuð og notandi fær villuskilaboð (AC-04.3).
- [ ] PASS/FAIL: Allar umsagnir um leikinn birtast á síðu hans (AC-03.2).
- [ ] PASS/FAIL: Leikur án umsagna sýnir skilaboð um það, ekki auðan reit (AC-03.3).

---

## #13 — Eyða eigin umsögn
**Requirement:** US-01 · **Size:** M

**Acceptance criteria**
- [ ] PASS/FAIL: Eyðingartakki birtist aðeins á umsögnum notandans sjálfs (AC-01.1).
- [ ] PASS/FAIL: Bæði texti og einkunn hverfa við eyðingu (AC-01.2).
- [ ] PASS/FAIL: Meðaleinkunn leiksins reiknast upp á nýtt (AC-01.3).
- [ ] PASS/FAIL: Notandi getur ekki eytt umsögn annars notanda (AC-01.4).

---

## Traceability

| User story | Issues |
|------------|--------|
| US-01 | #13 |
| US-02 | #3, #4, #5, #6, #7 |
| US-03 | #8, #9, #10, #12 |
| US-04 | #11, #12 |

Issues #1 og #2 eru undirstaða og tilheyra engri einni user story.
