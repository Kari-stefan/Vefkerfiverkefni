# Requirements

## Tilgangur

Vefkerfi þar sem notendur gefa retro-leikjum einkunn og skrifa umsagnir, svo hægt sé að
velja úr 13.603 leikjum eftir því hvað fólki fannst raunverulega um þá.

## Aðalnotandi

**Umsagnaskrifarinn.** Einhver sem spilar retro-leiki og vill skrifa umsagnir, gefa
einkunnir og sjá hvað aðrir skrifuðu. Hann kemur á síðuna til að leggja eitthvað til, ekki
bara til að fletta upp.

Þetta er ekki „allir“: sá sem vill aðeins fletta upp upplýsingum um leik er ekki
aðalnotandinn og kerfið þarf ekki að þjóna honum sérstaklega.

## Vandamál

**13.603 leikir og ekkert til að velja eftir.** Safnið er of stórt til að fletta í gegnum og
það er engin leið að vita hvað er þess virði að spila. Einkunnirnar sem fylgja gögnunum
leysa það ekki: 2.736 leiki (20,1%) vantar einkunn með öllu, og þær sem eru til koma úr
gagnasafninu án heimildar — enginn veit hver gaf þær eða af hverju. Umsagnir frá fólki eru
rekjanlegar og fylla götin.

## Out of scope

Það sem við ætlum viljandi ekki að byggja:

- **Mobile app** — vefur í vafra, ekkert app.
- **Recommendation AI** — engar sjálfvirkar tillögur reiknaðar af vél.
- **Comment undir umsögnum annarra** — umsagnir já, þræðir nei.
- **Follow, vinir og straumur** — ekkert félagsnet.

## Functional requirements

Merking: **[MVP]** verður að virka, **[SH]** should have, **[+]** utan product brief.

FR-01 **[MVP]**: Notandi getur leitað að leik með því að slá inn hluta úr titli og þrengt
niðurstöðurnar með síu á vél og flokk. Leitarstrengur og síur virka saman í einni fyrirspurn.

FR-02 **[MVP]**: Notandi getur gefið leik einkunn á heiltöluskalanum 1–5. Hver notandi á eina
einkunn á hvern leik; ný einkunn kemur í stað þeirrar fyrri.

FR-03 **[MVP]**: Leikjasíða sýnir titil, vél, útgáfuár, hönnuð, útgefanda, flokk, lýsingu og
kápumynd leiksins. Þegar reitur er tómur í gögnunum birtist skýr staðgengilstexti í stað
auðs svæðis.

FR-04 **[MVP]**: Notandi getur séð allar umsagnir og einkunnir sem aðrir notendur hafa skráð
á leikinn, á leikjasíðu hans.

FR-05 **[+]**: Notandi getur merkt leik sem spilaðan og séð lista yfir alla leiki sem hann
hefur merkt.

FR-06 **[+]**: Notandi getur valið tvo leiki og séð þá hlið við hlið í töflu sem sýnir
einkunn, útgáfuár, vél, flokk og hönnuð fyrir báða.

FR-07 **[SH]**: Notandi getur stofnað reikning með netfangi og lykilorði. Kerfið sendir
staðfestingarkóða í tölvupósti og reikningurinn verður virkur þegar réttur kóði er sleginn
inn.

FR-08 **[SH]**: Kerfið geymir lykilorð sem saltað hash, aldrei í hreinum texta, og flytur öll
gögn yfir HTTPS.

FR-09 **[MVP]**: Umsagnir, einkunnir og merkingar notanda varðveitast eftir að vafra er lokað
og eftir að kerfið er endurræst.

FR-10 **[+]**: Kerfið sýnir lista yfir þá leiki sem fengu flestar umsagnir síðustu 7 daga.
Leikur sem fékk enga umsögn á tímabilinu birtist ekki á listanum.

## Non-functional requirements

NFR-01 (Performance): Leitarniðurstöður úr 13.603 færslum birtast innan 2 sekúndna.

NFR-02 (Accessibility): Allur megintexti uppfyllir birtuskilahlutfall að minnsta kosti 4,5:1
(WCAG 2.1 AA), og notandi getur stækkað letur upp í 200% án þess að efni skerðist eða falli
út af skjánum.

NFR-03 (Responsive design): Vefsíðan virkar án lárétts skruns á skjábreiddum frá 360 px upp
í 1920 px.

## User stories

US-01: Sem Tölvuleikjaáhugamaður vil ég delete review takka svo að ég get eitt gömlum reviews sem ég er ekki lengur sammála

  AC-01.1 PASS/FAIL: Eyðingartakki birtist aðeins á umsögnum sem notandinn skrifaði sjálfur.
  AC-01.2 PASS/FAIL: Þegar umsögn er eydd hverfa bæði textinn og einkunnin af leikjasíðunni.
  AC-01.3 PASS/FAIL: Meðaleinkunn leiksins reiknast upp á nýtt án eyddu einkunnarinnar.
  AC-01.4 PASS/FAIL: Notandi getur ekki eytt umsögn annars notanda.

US-02: Sem umsagnaskrifari vil ég leita að leik eftir titli og þrengja niðurstöðurnar eftir vél og flokki, svo að ég finni tiltekinn leik án þess að fletta í gegnum 13.603 færslur.

  AC-02.1 PASS/FAIL: Notandi getur slegið inn hluta úr titli og fær lista yfir leiki sem innihalda þann streng.
  AC-02.2 PASS/FAIL: Notandi getur valið vél og flokk samtímis, og allar niðurstöður uppfylla bæði skilyrðin.
  AC-02.3 PASS/FAIL: Hver niðurstaða sýnir titil, vél og útgáfuár.
  AC-02.4 PASS/FAIL: Ef ekkert passar birtast skýr skilaboð í stað tóms lista.

US-03: Sem tölvuleikjaáhugamaður vil ég sjá einkunnir og umsagnir annarra á leikjasíðunni, svo að ég geti metið hvort leikurinn sé þess virði áður en ég set kvöld í hann.

  AC-03.1 PASS/FAIL: Leikjasíðan sýnir titil, vél, útgáfuár, hönnuð, útgefanda, flokk og lýsingu.
  AC-03.2 PASS/FAIL: Allar umsagnir sem skráðar eru um leikinn birtast á síðu hans.
  AC-03.3 PASS/FAIL: Leikur sem hefur enga umsögn sýnir skilaboð um það, ekki auðan reit.
  AC-03.4 PASS/FAIL: Þegar reit vantar í gögnin (t.d. lýsingu, sem vantar hjá 185 leikjum) birtist skýr texti í staðinn fyrir tómt svæði.

US-04: Sem umsagnaskrifari vil ég gefa leik einkunn og skrifa umsögn um hann, svo að mín skoðun bætist í safnið og næsti maður hafi eitthvað til að fara eftir.

  AC-04.1 PASS/FAIL: Notandi getur valið einkunn frá 1 til 5 og hún vistast.
  AC-04.2 PASS/FAIL: Vistuð umsögn birtist á leikjasíðu leiksins strax að lokinni vistun.
  AC-04.3 PASS/FAIL: Umsögn styttri en 20 stafir er ekki vistuð og notandi fær villuskilaboð.
  AC-04.4 PASS/FAIL: Umsögnin festist við þann eina leik sem valinn var og birtist ekki á öðrum leik sem ber sama titil.

## Open questions

- **Tvítökin á `game_id`.** `Ultracore` kemur fjórum sinnum fyrir í gögnunum og 887 auðkenni
  endurtaka sig alls. Er það einn leikur á síðunni okkar eða fjórir? Þetta blokkar
  gagnalíkanið og þarf að leysast áður en fyrsta umsögnin er vistuð.
- **Hámarkslengd umsagnar.** Lágmarkið er sett á 20 stafi í AC-04.3; efri mörk eru óákveðin.
- **Má breyta umsögn eftir á**, eða bara eyða henni og skrifa nýja?
- **Hvað verður um umsagnir harðkóðaða notandans** í MVP þegar reikningar bætast við í
  Should have? Fá þær eiganda, eða falla þær út?
- **Fylgja myndskrárnar með?** `cover_image_path` vísar á `covers/…` sem er ekki í
  gagnapakkanum. Ef þær koma ekki, hvaða staðgengilsmynd notum við?
- **Hvaða póstþjónusta sendir staðfestingarkóðann** í FR-07? Hún er ekki nefnd í product
  brief og krefst uppsetningar.
- **Eiga FR-05, FR-06 og FR-10 heima í þessu verkefni?** Þær eru merktar [+] því þær eru
  hvergi í product brief — hvorki í MVP, Should have né Could have.

## Traceability

Issue-númer vísa í GitHub: `Kari-stefan/Vefkerfiverkefni`.

| User story | Issues |
|------------|--------|
| US-01 | #9 |
| US-02 | #5, #6, #7, #8, #10 |
| US-03 | #11, #12, #13, #15 |
| US-04 | #12, #14 |

Issues #3 (lykill fyrir leik) og #4 (CSV-innflutningur) eru undirstaða og tilheyra engri
einni user story — allar fjórar hanga á þeim.

## Review notes

_Skref 16 — afstaða hópsins til Agent-feedback._

**Tillaga sem við samþykktum:**
Að taka notendareikninga út úr MVP og nota einn harðkóðaðan notanda á meðan kjarninn er
byggður. Agent hélt því fram að auðkenning væri klassísk ástæða þess að verkefni klárast
ekki. Við völdum MVP með fjórum atriðum — leit, leikjasíðu, einkunn og umsögn — og færðum
reikninga í Should have (FR-07).

**Tillaga sem við höfnuðum:**
Að setja myndir og fanart út af borðinu. Agent lagði það til vegna þess að `has_fanart`
hefur enga slóð í gögnunum og myndskrárnar fylgja ekki gagnapakkanum. Við höfnuðum því og
héldum myndum inni — sjá FR-03 og issue #15, þar sem staðgengilsmynd er notuð í staðinn.

Við höfnuðum einnig ábendingu Agent um að staðfestingarkóði í tölvupósti væri of stór
biti fyrir verkefnið, og héldum honum í FR-07.

**Af hverju:**
> _(óútfyllt — þetta er ykkar mat og Agent skrifar það ekki)_
