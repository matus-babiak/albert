# Prompt pre Claude: postav hru

Skopírujte celý text pod čiarou do Claude (najlepšie Claude Code v tomto priečinku, alebo do bežného Claude chatu). Hranaté zátvorky si pred odoslaním upravte.

---

Vytvor jednoduchú počítačovú hru pre päťročného chlapca. Hra sa volá **„[Albert] a Hviezdna záhrada“**. Podrobný návrh je v súbore `NAVRH.md` v tomto priečinku. Ak k nemu nemáš prístup, drž sa tohto zadania.

## Technické riešenie
- **Jeden súbor `index.html`** (HTML + CSS + čistý JavaScript), ktorý sa spustí dvojklikom v prehliadači (Chrome/Safari) **bez internetu**, bez inštalácie, bez knižníc, bez sledovania a bez sieťových požiadaviek.
- Grafika kreslená v kóde (SVG alebo Canvas): mäkké pastelové tvary, veľké okrúhle postavičky, žiadne blikanie ani rýchle efekty.
- Hlas: slovenčina cez `speechSynthesis` (hlas `sk-SK`, ak je dostupný, pomalšie tempo `rate ≈ 0.85`). Každý text, ktorý zaznie, sa zobrazí aj ako malý titulok pre rodiča. Ak slovenský hlas chýba, hra funguje ďalej s titulkami.
- Zvuky generované cez Web Audio API (jemné ťuky, žblnknutie, tóny kvetov). Žiadna hudba v slučke.
- Ovládanie iba myšou: klik a ťahanie. Všetky ciele aspoň 120 px a s „prilepením“ pri ťahaní. Dieťa nemusí vedieť čítať.
- Postup sa ukladá do `localStorage` automaticky po každom kroku.
- Na začiatku súboru bude objekt `NASTAVENIA` s komentármi po slovensky: `menoDietata`, `menoKamarata` (predvolene „Bruško“), `druhKamarata` (predvolene býček), `oblubenaFarba`, `oblubeneZviera`, `vypravNaSpustenie` (1), `budikMinuty` (0 = vypnutý), `hraVoDvojici` (false), `hlas` (true), `aktivityDovidenia` (zoznam textov).

## Herná štruktúra
- Domov: domček na kopci, ohník, záhrada so **6 hlinenými kruhmi**. Na každej výprave dieťa pomôže postavičke a dostane **jedno semienko**. Spolu je ich presne 6 a nič ďalšie sa nezbiera.
- **Každá výprava má rovnakú kostru:** ranný pozdrav → krátka cesta (animácia) → pomoc (hlavná aktivita, bez časomiery) → poďakovanie, postavička povie, ako sa cíti → návrat domov, zasadenie a poliatie semienka → „počkáme a pozeráme“ (10 s rastie rastlinka, kamarát pomaly dýcha, nič sa nedá kliknúť) → večer pri ohníku s otázkou na rozhovor s rodičom → obrazovka **„Dovidenia“** s aktivitou bez obrazovky a jediným tlačidlom „Hotovo“.
- Výpravy (odomykajú sa po poradí):
  1. **Záhradka pri domčeku**: úvod, naučí ťahanie a klikanie.
  2. **Šepkajúci les**: roztriediť lieskovce, šišky a jabĺčka do 3 košíkov pre ježka, potom nájsť 3 schované zvieratká (obľúbené zviera z nastavení je jedno z nich).
  3. **Hudobná lúka**: 5 kvetov s rôznymi tónmi (pentatonika, aby to vždy znelo pekne); dieťa naťuká 4 až 8 tónov, hra ich prehrá ako „včielkinu pesničku“. Neexistuje zlá odpoveď.
  4. **Morský breh**: smutná rybka. Dieťa vyberie z 4 tváričiek, ako sa cíti (smutná, nahnevaná, vystrašená, veselá). Kamarát emóciu pomenuje a povie, že je v poriadku ju cítiť. Potom vyberie, čo pomôže (objatie, pesnička, kamarát). Na záver nájde 2 tvary v oblakoch.
  5. **Dielňa v oblakoch**: postaviť most k vtáčiemu hniezdu z dosiek, lán a kameňov. Funguje aspoň 3 rôzne riešenia; nefunkčné sa vtipne zakolíše a kamarát povie „Skúsime inak?“.
  6. **Hviezdna hora**: spájať hviezdy klikaním do vlastného súhvezdia; hra vyzve dieťa, aby ho pomenovalo nahlas rodičovi. Po tejto výprave záhrada rozkvitne do hviezd: **záverečná oslava a jasný koniec hry**.

## Ochrana pred závislosťou (dôležité, nevynechaj)
- Po `vypravNaSpustenie` výpravách sa zobrazí „Dovidenia“ a domov ukazuje spiacu záhradu. Ďalšia výprava sa ponúkne až po novom otvorení hry. Hra sa **nikdy** nepýta „Ešte jednu?“.
- **Žiadne** body, mince, hviezdičky za výkon, denné série, časovače, odpočítavanie, rebríčky, náhodné odmeny, reklamy, nákupy, notifikácie ani výčitky typu „chýbal si mi“.
- Chyba sa nikdy netrestá: nesprávny ťah sa jemne vráti späť, žiadne červené X ani smutný zvuk.
- Veľké tlačidlo 🏠 v rohu: kedykoľvek uloží hru a pokojne ukončí deň („Dobre, ideme si oddýchnuť“).
- Ak je nastavený `budikMinuty`, kamarát po uplynutí času láskavo navrhne dokončiť a ísť domov. Aktivitu neprerušuje násilne.
- Ak je `hraVoDvojici` zapnuté, niektoré úlohy (napr. ťažké poleno) treba ťahať dvakrát, „raz ty, raz mama/tato“, a hra to povie.
- Po dokončení všetkých 6 výprav sa dajú hrať znova, ale nič nové sa už neodomyká.

## Rodičovský kútik
- Ikonka ozubeného kolieska. Otvorí sa až po **podržaní 3 sekundy**.
- Dá sa v nej meniť `NASTAVENIA` (uložia sa do `localStorage` a majú prednosť pred predvolenými), zvoliť výpravu, vymazať postup.

## Jazyk a tón
- Všetko po slovensky, krátke vety (najviac 10 slov), láskavý a pokojný tón. Kamarát pomenúva pocity („Ježko je teraz rád.“) a kladie otvorené otázky („Čo myslíš ty?“).

## Ako postupovať
1. Najprv vytvor `index.html` s domovom, rodičovským kútikom a výpravou 1, aby sa dala hra hneď vyskúšať. Potom pridaj výpravy 2 až 6.
2. Na konci napíš krátky návod pre rodiča: ako hru spustiť, ako zmeniť nastavenia a ako rýchlo otestovať všetky výpravy (napr. v rodičovskom kútiku voľba „preskočiť na výpravu“).
3. Ak môžeš, hru si sám otvor v prehliadači, prejdi každú výpravu a over, že sa dá dokončiť iba myšou a že po výprave sa naozaj zobrazí „Dovidenia“.
