# Vefforritun 2, 2022. Verkefni 3: Viðburðakerfis vefþjónustur

[Kynning á verkefni í tíma](https://youtu.be/2eikxqJ80pM).

Verkefnið er framhald af [verkefni 2](https://github.com/vefforritun/vef2-2022-v2/).

Bæta skal við vefþjónustum við viðburðakerfið með notendaskráningu ásamt því að bæta við fleiri aðgerðum.

Eftirfarandi eru markmið verkefnisins:

* Búa til og hanna vefþjónustur
* Framkvæma _CRUD_ aðgerðir gegnum vefþjónustlag með staðfestingu og hreinsun gagna
* Setja upp auðkenningu með JWT tokens, innskráningu og nýskráningu
* Skrifa „integration test“ fyrir vefþjónustu
* Prófanir og debug á vefþjónustum

## Notendaumsjón

Útfæra skal innskráningarkerfi með `passport`. Nota skal töflu í postgres grunni til að geyma notendanafn og lykilorð. Lykilorð skal vista með `bcrypt`.

Geyma skal gögn fyrir notanda í töflu í gagnagrunni:

* `id` primary key fyrir töflu
* `name`, nafn notanda, krafist, hámark 64 stafir
* `username` einkvæmt og krafist, hámark 64 stafir
* `password` krafist, hámark 256 stafir

Útbúa skal a.m.k. einn notanda með gefið notendanafn lykilorð í `readme` í skilum.

Innskráning fer fram gegnum `/login`, nýskráning fer fram í gegnum `/register`.

Huga þarf að öryggi, sér í lagi að aðeins sé hægt að framkvæma aðgerðir ef viðkomandi er innskráður og að lykilorð sé geymt á öruggan máta.

## Vefþjónustur

Þegar þau eru send á bakenda skal framkvæma _validation_  og _sanitization_ á gögnum áður en þau eru vistuð í gagnagrunn.

Ef villur eru í gögnum skal birta það á framenda ásamt þeim gildum sem komu fram.

Huga þarf að öryggi, sér í lagi að engar _xss_ holur séu til staðar. Leyfilegt er að taka við HTML og birta í lýsingu á viðburði, ef það er öruggt.

### Notendaumsjón, vefþjónustur

* `/users/`
  * `GET` skilar síðu af notendum, aðeins ef notandi sem framkvæmir er stjórnandi
* `/users/:id`
  * `GET` skilar notanda, aðeins ef notandi sem framkvæmir er stjórnandi
* `/users/register`
  * `POST` staðfestir og býr til notanda. Skilar auðkenni og nafn. Notandi sem búinn er til skal aldrei vera stjórnandi
* `/users/login`
  * `POST` með notandanafni og lykilorði skilar token ef gögn rétt
* `/users/me`
  * `GET` skilar upplýsingum um notanda sem á token, auðkenni og nafn, aðeins ef notandi innskráður

Aldrei skal skila eða sýna hash fyrir lykilorð.

### Viðburðir

* `/events/`
  * `GET` skilar síðu af viðburðum
  * `POST` býr til vibðurð, aðeins ef innskráður notandi
* `/events/:id`
  * `GET` skilar viðburð
  * `PATCH` uppfærir viðburð, a.m.k. eitt gildi, aðeins ef notandi bjó til viðburð eða er stjórnandi
  * `DELETE` eyðir viðburð, aðeins ef notandi bjó til viðburð eða er stjórnandi

### Skráningar

* `/events/:id/register`
  * `POST` skráir notanda á viðburð, aðeins ef innskráður notandi
  * `DELETE` afskráir notanda af viðburði, aðeins ef innskráður notandi og til skráning

## Postgres grunnur

Vista skal gögn í postgres gagnagrunni, hér er skilgreining á grunni frá því í verkefni 2:

Fyrir viðburði:

* `id` primary key fyrir töflu
* `name` krafist, nafn á viðburði hámark 64 stafir, t.d. `Hönnuðahittingur í mars`
* `slug` útbúið útfrá nafni hámark 64 stafir, krafist, útbúið útfrá `name` þ.a. aðeins `ASCII` stafir séu notaðir og `-` í staðinn fyrir bil, t.d. `honnudahittingur-i-mars`
* `description`, texti, valkvæmt
* `created`, dagstími þegar færsla var búin til
* `updated`, dagstími þegar færslu var breytt

Fyrir skráningar:

* `id` primary key fyrir töflu
* `name` krafist, nafn á þeim sem skráir sig, hámark 64 stafir
* `comment` valkvæmt, athugasemd við skráningu
* `event` tala, krafist, vísun í `id` í viðburðatöflu
* `created`, dagstími þegar færsla var búin til

Bæta þarf við:

* `name` við notendatöflu
* Tengingu viðburða við notanda sem bjó hann til í stað þess að skráning hafi `name`. Skráningartaflan verður _tengitafla_ milli notenda og viðburða sem á sér `comment` við þá skráningu
* Virkni til þess að eyða viðburðum m.t.t. þess að tenging er milli skráninga og viðburðs og hvað gera eigi við þær skráningar

Huga þarf að öryggi, sér í lagi að engar _injection_ holur séu til staðar.

Útbúa skal a.m.k. þrjá test viðburði og eina til þrjár test skráningar á hvern þeirra.

## Tæki, tól og test

Uppsett er `eslint` fyrir JavaScript. Engar villur skulu koma fram ef `npm run lint` er keyrt.

`jest` er uppsett með dæmi um hvernig keyra megi test á _sér_ test gagnagrunn.

Skrifa skal test fyrir a.m.k. eina aðgerð á hvern endapunkt.

Setja skal upp vefinn á Heroku tengt við GitHub með Heroku postgres settu upp.

Skrá skal og setja dæmi um það hvernig kalla eigi á vefþjónustur með cURL.

## Mat

* 50% Vefþjónustur, útfærsla og tenging við gagnagrunn
* 30% Notendausmjón með nýskráningu og innskráningu með JWT
* 20% Tæki, tól og test, verkefni sett upp á Heroku, dæmi um köll

## Sett fyrir

Verkefni sett fyrir í fyrirlestri miðvikudaginn 16. febrúar 2022.

## Skil

Skila skal í Canvas í seinasta lagi fyrir lok dags föstudaginn 4. mars 2022.

Skil skulu innihalda:

* Slóð á verkefni keyrandi á Heroku
* Slóð á GitHub repo fyrir verkefni. Dæmatímakennurum skal hafa verið boðið í repo. Notendanöfn þeirra eru:
  * `MarzukIngi`
  * `WhackingCheese`

---

> Útgáfa 0.2

| Útgáfa | Breyting                                     |
|--------|----------------------------------------------|
| 0.1    | Fyrsta útgáfa                                |
| 0.2    | Fjarlægja `stylelint`, laga lýsingu á testum, fjarlægja netfang úr lýsingu |
