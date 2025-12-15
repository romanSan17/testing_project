# Testiplaan – Ülesanne 2: Projekti seadistamine ja API ühendamine

**Projekti nimetus**: API töökindluse kontroll  
**Koostaja**: Martin Sild, 26.11.2025

## 1. Ülevaade
**Siht** – Kontrollida FastAPI serveri ühendust JSONPlaceholder ja Rick & Morty API-ga ning monitoorida andmete ühtlustamist.  
**Fookus:** Veenduda, et `/api/koond` tagastab oodatava struktuuriga JSONi ning käsitleb vigaseid välisühendusi veakoodiga HTTP 502.

## 2. Testimise ring
**Testitavad osad:**
- FastAPI backend (`backend/main.py`)
- Avalikud andmeallikad: JSONPlaceholder, Rick & Morty
- Lõpp-punktid: `/status`, `/api/koond`
- Logifailid ja veatöötlus

## 3. Nõuded ja kontrollkriteeriumid

| Nõue               | Kontrollmeetod      | Tulemuse ootus                                              |
|--------------------|---------------------|-------------------------------------------------------------|
| `/status` 200 kood | GET /status         | {"olek": "aktiivne", "päritolu": "API töökindlus"}         |
| `/api/koond` ühendab välisandmed | GET /api/koond | JSONis on väljad `postitus`, `tegelane`, `allikad`, `püütudAeg` |
| Logikirjed         | Vaata logifaili     | Logi sisaldab välispäringute ja kombineerimise infot        |
| Veaolukord välis-API-s | Muuda andmeallika URL | HTTP 502 ja detailobjekt (sõnum, allikas, põhjus)           |

## 4. Riskide analüüs

| Risk                   | Mõju                     | Tõenäosus     | Leevendus              |
|------------------------|-------------------------|---------------|------------------------|
| Välis-API pole saadaval | Andmekoond puudub       | Keskmine      | 502 vigade käsitlemine |
| Vale JSON struktuur     | Testid annavad valeresultati | Madal      | Kodeeritud väljatestid |

## 5. Töövõtted ja rakendused
- Ühikutestid FastAPI-le
- Integratsioonitestid `/api/koond` kaudu
- Automattestid: pytest, käsurea HTTP päringud

## 6. Testkeskkonnad
- Python 3.13
- Testandmeteks avalikud API-d
- Vigade simuleerimiseks mock-serverid

## 7. Tööjaotus ja ajakava

| Faas                        | Vastutaja   | Kestus     |
|-----------------------------|-------------|------------|
| Virtuaalkeskkonna seadistus | Martin Sild | 5 min      |
| Serveri ja `/status` testid | Martin Sild | 2 min      |
| Koondandmete/kombineerimise test | Martin Sild | 2 min      |
| Veaolukordade katsetus      | Martin Sild | 10 min     |

## 8. Tulemite dokumenteerimine
- Tulemid: `docs/results/task2.md`, `backend/README.md`
- Olulised testid sooritatud, veatöötlus toimib


---

# Testiplaan – Ülesanne 3: GA4 sündmuste kontroll

**Projekt:** Töökindluse analüütika  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Sissejuhatus

**Eesmärk:** Kontrollida, kas Google Analytics 4 (GA4) sündmused (`variant_vaade`, `variant_vahetus`) liiguvad eri olukordades õigesti.

## 2. Testide ulatus
**Osa:**  
- Frontend (`index.html`, `app.js`, `ab.js`)
- Backend (`GET /api/koond`)
- GA4 ja DebugView testimine

## 3. Nõuded

| Kontrollpunkt      | Kriteerium                                                        |
|--------------------|-------------------------------------------------------------------|
| GA4 script olemas  | `index.html` sisaldab GA4 scripti viidet                         |
| `variant_vaade`    | Sündmus sisaldab `variant`, `sessioonId`, `staadium`; päring saadetud|
| `variant_vahetus`  | Sündmus salvestatakse nuppu klikates                              |
| Ekraanipildid      | Võrgupäringute ning logide screenshote on säilitatud             |

## 4. Riskide hinnang

| Risk                         | Mõju          | Tõenäosus | Leevendus         |
|------------------------------|--------------|-----------|-------------------|
| GA4 päringud takerduvad      | Testid ebaõnnestuvad | Keskmine  | debug_mode + mockid |
| Backend-i info puudub        | Variantide info puudu | Kõrge     | Backend'i töö kontroll enne |
| CORS/cookie piirangud        | Sündmus ei salvestu  | Keskmine  | Testida localhost’is, vajadusel CORS lubamine |

## 5. Kasutatavad tööriistad
- DevTools (võrgu/logivaade)
- Python HTTP server
- GitHub Actions CI

## 6. Testkeskkonna kirjeldus
- Python 3.10+
- Node.js kui vaja
- Chrome / Firefox

## 7. Testimise ajakava

| Töö                | Vastutaja   | Aeg    |
|--------------------|-------------|--------|
| Sündmuste kontroll | Martin Sild | 10 min |
| Võrgupaneeli ülevaatus | Martin Sild | 5 min  |
| Ekraanipiltide talletus | Martin Sild | 5 min  |

## 8. Tulemuste dokumenteerimine
- Raport: `docs/results/analytics/`
- Kriteeriumid: sündmus liigub, pildid olemas, logid korrektsed

---

# Testiplaan – Ülesanne 4: Pytest

**Projekt:** API ühenduse stabiilsuse testimine  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Sissejuhatus
**Siht:** Tagada FastAPI backendile reguleeritud testimine Pytesti vahendusel.

## 2. Testimise objektiiv

- Backend (`backend/main.py`), sh `/status`, `/api/koond`, `hanki_andmed()`
- Testid kataloogis `tests-python/`
- Logifailid: `docs/results/pytest/pytest.log`

## 3. Nõuded ja kontrollpunktid

| Kontrollpunkt     | Kirjeldus                        |
|-------------------|----------------------------------|
| TestCli töötab    | GET päringud tagastavad 200 või 502|
| Error handling    | Vea olukord genereerib 502       |
| Skeemi test       | Väljad `pealkiri`, `nimi`, `allikad` olemas|
| Logi olemas       | pytest.log on salvestatud        |

## 4. Riskid

| Probleem              | Mõju         | Tõenäosus | Lahendus                |
|-----------------------|-------------|-----------|-------------------------|
| Välis-API mitteaktiivne| Test kukub  | Kõrge     | monkeypatchi kasutus    |
| Vale skeem            | Väline allikas muutub | Keskmine | Kodeeritud validatsioon |
| Moodulite import error| Pytest ei käivitu | Keskmine | Kontroll PYTHONPATH     |
| Logi puudub           | Puudulik testiaruanne | Madal     | Pytest väljund suunata logifaili |

## 5. Kasutatavad tööriistad

- Pytest
- FastAPI TestClient
- monkeypatch/Responses

## 6. Keskkonna tingimused

- Python 3.10+
- pytest käsk `pytest tests-python -v`
- logifail: `pytest tests-python -v --capture=no > docs/results/pytest/pytest.log 2>&1`

## 7. Ajakava

| Töö              | Vastutaja   | Aeg       |
|------------------|-------------|-----------|
| Testide loomine  | Martin Sild | 10-15 min |
| Testkäivitus/logi| Martin Sild | 5 min     |

## 8. Raport

- Tulemid logifailis
- Kõik testid rohelised
- Logifail salvestatud

---

# Testiplaan – Ülesanne 5: JavaScript testid (Jest)

**Projekt:** Frontendi funktsioonide verifitseerimine  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Eesmärk

Testida ja kinnitada `frontend/ab.js` funktsioonide korrektsust Node.js ja Jesti abil.

## 2. Testimise ulatus

- Testitavad: valiVariant, vahetaVariant, looLayoutHTML, looSyndmuseKeha
- localStorage mockimine testides
- Testid kataloogis `tests-js/`, logi `docs/results/jest/jest.log`

## 3. Kontrollpunktid

| Nõue                 | Kontroll                 |
|----------------------|-------------------------|
| valiVariant töötab   | Tagastab olemasoleva    |
| vahetaVariant töötab | Varieerib valikut      |
| looSyndmuseKeha lisab metaandmed | Skeemi kontroll olemas |
| localStorage toimib  | Mock/fallback tagab salvestuse/tagasitoomise |
| Logifail              | Testi väljund salvestatud |

## 4. Riskid ja lahendus

| Risk                | Mõju        | Tõenäosus | Lahendus             |
|---------------------|-------------|-----------|----------------------|
| Funktsiooni muutus  | Test ebaõnnestub| Keskmine | Testide uuendamine   |
| localStorage error  | Salvestus katkeb | Madal   | Mock/fallback        |
| Jest käivitamise probleem | Testid ei jookse | Keskmine | Node/npm kontroll   |
| Logi salvestus | Testrapport puudub | Madal   | Väljund suunamine logifaili |

## 5. Tööriistad
- Jest
- Node.js/mockimine
- npm test
- Logimise väljund `docs/results/jest/jest.log`

## 6. Keskkonnad

- Node 18+, npm 9+
- Käivituskäsk: `cd tests-js`, `npm install`, `npm test > ..\docs\results\jest\jest.log 2>&1`

## 7. Ajastamine

| Töö                | Vastutaja   | Kestvus  |
|--------------------|-------------|----------|
| Testide kirjutamine| Martin Sild | 10-15 min|
| Testkäivitus/logi  | Martin Sild | 5 min    |

## 8. Tulemite salvestus

- Logifail on loodud ja kõigis testides PASS
- Tulemused salvestatakse `docs/results/jest/jest.log`

---

# Testiplaan – Ülesanne 6: A/B testimise kontroll

**Projekt:** Frontendi valiku, salvestuse ja sündmuse loogika  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Siht

Verifitseerida, et variandi valik, salvestus, sündmuste logimine ja visuaalne identifitseerimine toimivad.

## 2. Ulatus

- Testitud moodulid `frontend/ab.js`, sündmused nuppudega `frontend/app.js`
- localStorage salvestus
- Logid kataloogis `docs/results/analytics/`

## 3. Kontrollpunktid ja nõuded

| Kontroll             | Kirjeldus                                   |
|----------------------|---------------------------------------------|
| Variandi identifitseeritus | Klassid või ID-d selgelt eristavad     |
| Funktsioonid töötavad | valiVariant, vahetaVariant, looLayoutHTML   |
| localStorage         | Valiku säilitamine ja taastamine             |
| Sündmuste vallandumine| `variant_vaade` ja `variant_vahetus` salvestatud |
| Logifail             | Logid olemas failides                        |

## 4. Riskide analüüs

| Probleem            | Mõju     | Tõenäosus | Lahendus            |
|---------------------|----------|-----------|---------------------|
| Variandi eristus puudulik | Vale sündmuslogi | Keskmine  | Klasside/ID kontroll |
| localStorage error  | Salvestamine ebaõnnestub | Madal   | fallback/mocking     |
| Sündmus ei vallandu | Analüütika puudub | Keskmine  | Testimine/logimine   |
| Logide salvestus | Testiaruannet pole | Madal   | Automaat/logide talletus |

## 5. Rakendused
- GA4 debug-vaatlus
- Mock localStorage

## 6. Keskkonnad
- Chrome / Firefox
- Käivitamine index.html kaudu
- Logi kataloog: `docs/results/analytics/`

## 7. Ajakava

| Faas                 | Vastutaja   | Aeg    |
|----------------------|-------------|--------|
| Variandi ja HTML test| Martin Sild | 5-10 min |
| Nupu/sündmuse kontroll| Martin Sild | 5 min    |
| Logide talletus      | Martin Sild | 5 min    |

## 8. Tulemite salvestus

- Logid ja sündmuste registreerimine, GA andmetes mõlemad variandid olemas

---

# Testiplaan – Ülesanne 7: Integratsioonitestid

**Projekt:** FastAPI backend ja välisallikate integratsioon  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Sissejuhatus

**Eesmärk:** Kontrollida backend’i, välis-API-de ja logi integratsiooni ning tagada, et testid ei sõltu pärisandmetest.

## 2. Katsetamise ulatus

- FastAPI backend: `/api/koond` endpoint
- Testid: `tests-integration/`
- Logid: `docs/results/integration/integration.log`

## 3. Kontrollikriteeriumid

| Kontrollpunkt    | Kirjeldus                                 |
|------------------|-------------------------------------------|
| Edukas API       | 200 vastus, JSON kerge                    |
| Veaolukord       | 502 kood ja detailiga vastus              |
| Logifail         | integratsiooni logikirjed failis           |

## 4. Riskid ja leevendus

| Risk                   | Mõju            | Tõenäosus | Lahendus            |
|------------------------|-----------------|-----------|---------------------|
| Välis-teenus mitteaktiivne| Testid kukuvad | Madal     | Mockimine responses |
| Logifail puudub        | Testiaruanne pole| Madal   | Failitee kontroll   |
| Endpoint muutub        | Katsetulemused valed | Keskmine | Testide hooldus     |

## 5. Töövahendid

- TestClient FastAPI jaoks
- pytest
- responses välis-API-de jaoks
- Logide failisarvestus

## 6. Keskkonnatingimused

- Python 3.12+
- Virtuaalkeskkond
- Käsk: `pytest tests-integration -v`

## 7. Ajakava

| Töö         | Vastutaja    | Kestus     |
|-------------|--------------|------------|
| Testide loomine | Martin Sild | 10–15 min  |
| Testdev ja logi | Martin Sild | 5 min      |

## 8. Tulemite salvestus

- Logifailid ja testide PASS
- Edukad/nõrgad API päringud on logis

---

# Testiplaan – Ülesanne 8: GitHub Actions CI

**Projekt:** CI automaattestide süsteem  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Eesmärk

Kontrollida automaatset Python ja JS testide käivitust iga push/pull_request korral ning logide arhiveerimist.

## 2. Katsetatavad komponendid

- CI workflow fail `.github/workflows/tests.yml`
- Python-testid: `tests-python/`, `tests-integration/`
- JavaScript-testid: `tests-js/`
- Artefaktide salvestamine: `docs/results/pytest/**`, `docs/results/jest/**`
- CI dokumentatsioon: `docs/results/ci/README.md`

## 3. Kontrollpunktid

| Kontroll         | Kirjeldus                                  |
|------------------|--------------------------------------------|
| Workflow triger | Käivitub `push`/`pull_request` `main` harul |
| Python testid    | Keskkond seadistatud, testid käima          |
| JS testid        | Node 20, `npm ci` ja `npm test`             |
| Artefaktide salvestus | Logifailid arhiveeritud               |
| CI dokumentatsioon    | CI README sisaldab jooksu ja logide linke |

## 4. Riskihindamine

| Risk             | Mõju          | Tõenäosus | Leevendus             |
|------------------|--------------|-----------|-----------------------|
| Sõltuvusvead     | CI töö katkeb| Keskmine  | Kontrolli failid      |
| Testvead         | Workflow peatub | Keskmine| Paranda testid/koodi  |
| Vale artefaktitee | Logid kaovad | Madal     | Tee kontroll          |
| README ei genereeru | Dokumentatsioon puudub | Madal | YAML korrektne       |

## 5. Käitatavad töövahendid

- GitHub Actions, actions/checkout, actions/setup-python/node
- pytest, npm test
- actions/upload-artifact
- Bash README genereerimiseks

## 6. Testkeskkond

- GitHub repo
- Testide käsitsi käitatavus: pytest ja npm test

## 7. Ajakava

| Tegevus             | Vastutaja   | Kestus   |
|---------------------|-------------|----------|
| Workflow koostamine | Martin Sild | 5–10 min |
| Testide lisamine CI | Martin Sild | 5–10 min |
| Artefaktid/README raport | Martin Sild | 5 min    |

## 8. Tulemite dokumenteerimine

- CI logid, artefaktid, README
- PASS kõigil job’id
- Dokumentatsioonis viited jooksule ja logidele

---

# Testiplaan – Ülesanne 9: Locust – Koormustestid

**Projekt:** Backend jõudluse testimine  
**Koostaja:** Martin Sild, 26.11.2025

## 1. Eesmärk

Hinnata, kuidas FastAPI server talub 20–50 üheaegset päringut, mõõta API latentsust ning koostada tulemusaruanded.

## 2. Testimise ulatus

- Backend (`backend/`)
- Locust koormustest: `tests-performance-locust/locustfile.py`
- Tulemid: CSV (`docs/results/locust/koormus_*.csv`), HTML (`docs/results/locust/koormus.html`)
- Ekraanipildid: `docs/results/locust/koormus.md`

## 3. Kontrollpunktid

| Kontroll         | Kirjeldus                           |
|------------------|-------------------------------------|
| Backend aktiivne | Server töötab uvicorniga             |
| Locust-test      | 50 kasutajat, ramp-up 5/s, kestus 5 min |
| Tulemid          | Aruanded loodud ja salvestatud      |
| Edukriteeriumid  | Pole kriitilisi vigu, tulemid olemas|

## 4. Riskid ja leevendus

| Risk                | Mõju       | Tõenäosus | Leevendus                |
|---------------------|------------|-----------|--------------------------|
| Backend ei käivitu  | Test nurjub| Keskmine  | Kontrolli uvicorn         |
| Locust puudu        | Ei saa testida | Madal   | Paigalda pipiga           |
| Aruanne fail puudub | Dok puudub | Madal     | Kontrolli failiteed      |

## 5. Tööriistad

- Locust
- Uvicorn
- Aruandeid vaata Excel, HTML brauser
- Ekraanipiltide talletus

## 6. Keskkonnad

- Python virtuaalkeskkond
- Testfailid, aruandetee olemas
- Testikäsk: Locust koos --csv ja --html parametritega

## 7. Ajakava

| Töö            | Vastutaja   | Kestus   |
|----------------|-------------|----------|
| Backend kontroll| Martin Sild| 2 min    |
| Koormustest    | Martin Sild| 8 min    |
| Piltide lisamine| Martin Sild| 5 min    |

## 8. Raporteerimine

- Loodud aruandefailid (CSV, HTML)
- Ekraanipildid kaustas
- Backend talub koormust, tulemused säilitatud
