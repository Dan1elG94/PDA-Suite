# PDA Suite — produkčný build

Vylepšenia pre aplikáciu **PDA** bežiacu na `https://hf.simplifier.cloud/appDirect/PDA/`.

Skript nemení aplikáciu na serveri — beží až v prehliadači a dopĺňa do nej UI:
nový dizajn celej aplikácie, slovenské popisky, farebné stavové tlačidlá,
vyhľadávanie zákaziek naprieč pracoviskami, prehľad pracovísk na úvodnej
obrazovke, načítanie výkresov, obsluhu skenera a čítačky kariet.

---

## Inštalácia — stačí jeden skript

**[▶ Inštalovať PDA Suite](https://github.com/Dan1elG94/PDA-Suite/raw/refs/heads/main/production/pda-suite.user.js)**

`pda-suite.user.js` obsahuje **všetkých 18 modulov v jednom súbore**. Nainštaluješ
ho raz a jednotlivé moduly si potom zapínaš a vypínaš v nastaveniach — nemusíš
nič odinštalovávať ani doinštalovávať.

Súbor je **sebestačný**: pozadie aplikácie, obrázok v karte Zákazka a materiál aj
grafika na stavových tlačidlách sú v ňom vložené ako `data:` URI, takže nepotrebuje
žiadne ďalšie súbory z repozitára. Jediná externá závislosť je knižnica `xlsx` z CDN
(`@require`), ktorú Tampermonkey stiahne sám a používa ju len modul výkresov.

### Predpoklady (raz za počítač)

1. **Tampermonkey** —
   [Chrome Web Store](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
2. **Povoliť používateľské skripty** — `chrome://extensions` → Tampermonkey →
   *Podrobnosti* → zapnúť **„Allow user scripts"**.
   ⚠️ Bez tohto kroku sa skripty tvária, že bežia, ale nerobia nič.

V Tampermonkey nechaj **zapnutý vždy len jeden** skript PDA Suite — tento build
a staršie vetvy si navzájom prepisujú tie isté prvky v DOM.

### Overenie

Otvor PDA a daj **Ctrl + F5**. Vpravo dole sa objaví **⚙ ozubené koliesko** —
to je panel nastavení. V konzole prehliadača sa vypíše `[PDA Suite] aktívne
moduly: …`.

---

## Nastavenia

Panel otvoríš **ozubeným kolieskom vpravo dole**, alebo cez ikonu Tampermonkey →
*Nastavenia PDA Suite*. Pýta si heslo (predvolene `123456`, mení sa v paneli
v sekcii *Heslo do nastavení*).

### Moduly

| Modul | Predvolene | Poznámka |
|---|---|---|
| Nový dizajn HF Slovakia | zapnuté | prezlečenie celej aplikácie — pozadie HF, polopriehľadné karty s tmavomodrým rámom, jednotné hlavičky, farebné oblasti pracovísk, priestorové koláče SAP časov, Dokumentácia ako dlaždice, jednotný vzhľad okien aj prihlasovacej stránky |
| Vylepšená hlavička | zapnuté | slovenské popisky pri ikonách, väčšie meno, zvýraznené odhlásenie |
| Farebné tlačidlá | zapnuté | farby podľa pravidiel v nastaveniach (výroba zelená, prestoj oranžový, chyba červená) |
| Vyhľadávanie zákaziek | zapnuté | hľadá naprieč všetkými pracoviskami naraz |
| Prehľad pracovísk na úvode | zapnuté | úvodná obrazovka zbalená do farebných kategórií (Assembly, Welding, Machining, Quality Control) |
| Zoznam zákaziek ako pilulky | zapnuté | každá zákazka v jednom riadku s malými koláčmi časov; zoznam siaha po pätku; po nabehnutí myšou veľké okno s detailom operácie |
| Kompaktná hlavička detailu | zapnuté | zákazka, materiál a Production Order v jednom boxe; výkres, BOM a Operation Complete v jednom riadku |
| Popis operácie na celú obrazovku | zapnuté | veľké tlačidlo, po kliknutí popis v okne, text delený po čiarkach na riadky |
| Priestorové koláčové grafy | zapnuté | časy SAP (Setup / Machine / Labor) naklonené ako pohľad zboku |
| Krajší graf vyťaženia | zapnuté | čas dole len ako HH:MM, tenšie pásy, nižšie plátno |
| Krajšia tabuľka stavov | zapnuté | krátky dátum a čas, stav ako farebná pilulka, striedavo podfarbené riadky |
| Menu HF Slovakia (vpravo) | zapnuté | zvislý panel s pripravovanými funkciami (CHIPS, majster, materiál, TOOLSHOP, Flexus); je v detaile pracoviska aj na úvodnej obrazovke, dá sa schovať pásikom |
| Blokovanie tlačidla Späť | zapnuté | zabráni nechcenému vypadnutiu z aplikácie |
| Panel prepínania používateľov | vypnuté | má zmysel len na zdieľanom termináli |
| Čiarový skener a RFID karty | vypnuté | vyžaduje hardvér |
| Tlačidlo výkresu | vypnuté | vyžaduje firemnú sieť a Excel so zoznamom výkresov |
| Ľavý panel na celú výšku | vypnuté | ⚠️ rozpracované, zatiaľ rozhadzuje rozloženie |
| Ladiaci výpis do konzoly | vypnuté | len na hľadanie chýb |

### Vzhľad

Samostatná časť panela s dvoma posuvníkmi. Zmena je vidieť hneď, uloží sa
tlačidlom *Uložiť a obnoviť stránku* a ide aj do súboru s nastaveniami.

**Viditeľnosť obrázka v pozadí** — 0 až 100 %, predvolene **70 %**. Nižšia hodnota
znamená svetlejšie a menej rušivé pozadie; obrázok prekryje svetlý závoj.

**Výraznosť obrázkov na stavových tlačidlách** — 0 až 200 %, predvolene 100 %.
Do 100 % sa mení priehľadnosť grafiky, nad 100 % sa zvyšuje jej jas.

Zmena modulov sa uloží tým istým tlačidlom. Nastavenia sa dajú zálohovať a prenášať
medzi terminálmi cez *Uložiť do súboru* / *Načítať zo súboru*.

### Citlivé údaje

**Heslá kolegov, ID kariet ani kľúč k službe výkresov nie sú v kóde** a nikdy sem
nepatria — repozitár je verejný. Zadávajú sa v paneli nastavení a ukladajú sa len
lokálne do úložiska Tampermonkey na danom počítači.

V kóde ostávajú dve veci, ktoré skript potrebuje ako predvolené hodnoty:
predvolené heslo do panela nastavení (`123456`, po inštalácii si ho zmeň) a adresa
internej služby výkresov (`http://172.16.77.134:9000`), ktorá je zároveň
v hlavičke ako `@connect`.

---

## Ako to vnútri funguje

Celý skript stojí na troch spoločných častiach, ktoré zdieľajú všetky moduly:

- **`DomWatch`** — jeden `MutationObserver` pre celú stránku. Aplikácia je SAP
  UI5 a prekresľuje si DOM sama, takže moduly musia svoje prvky dokladať znova.
  Všetky prekreslenia sa zlučujú do jednej dávky.
- **`XhrBus`** — jedno odpočúvanie volaní na `/client/1.0/executeBO`. Prepisuje
  `XMLHttpRequest` v kontexte stránky a odpovede rozposiela modulom, ktoré sa
  prihlásili.
- **`UserSwitch`** — prepnutie používateľa cez SAP dialóg. Používa ho bočný
  panel aj čítačka kariet, preto je mimo oboch modulov.

Moduly medzi sebou komunikujú cez objekt `shared` (index zákaziek, aktuálna
operácia), nie cez globálne premenné na `window`.

Obrázky sú konštanty nad modulom nového dizajnu — `POZADIE` (pozadie stránky),
`KARTA_ZAKAZKA` (karta Zákazka a materiál) a desať `OBR_*` (grafika na stavových
tlačidlách). Všetky sú base64 `data:` URI, takže build nezávisí na žiadnom súbore
vedľa. Zdrojové obrázky sú v koreni repozitára ako `pozadie-5.jpg`
a `karta-zakazka.jpg` — slúžia len na ďalšie úpravy, skript ich nečíta.

Modul *Nový dizajn HF Slovakia* má dve vrstvy: základné prezlečenie a nad ním
funkciu `vzhladHF()` (sekcia 3.20), ktorá dolaďuje karty, hlavičky, koláče
a okná. Obe sa zapínajú a vypínajú spolu.

---

## Odkiaľ pochádza vzhľad

Vizuálnu podobu navrhol a vyladil **Jaroslav Tvarožek** vo vetve
[JaroTvarozek/PDA-3J](https://github.com/JaroTvarozek/PDA-3J), ktorá vznikla
z produkčného buildu 2.3.0. Do tohto repozitára je jeho dizajn zlúčený priamo
do modulu *Nový dizajn HF Slovakia* — nie je to samostatný modul, ako to bolo
v 3J. Podrobný zoznam zmien po verziách vedie Jaro v súbore
[`ZMENY.md`](https://github.com/JaroTvarozek/PDA-3J/blob/main/ZMENY.md).

---

## Pridanie nového vylepšenia

1. Napísať funkciu `modNiecoNove()` v sekcii **3. MODULY**
2. Pridať jeden riadok do zoznamu `MODULES` (`id`, `name`, `desc`, `def`, `run`)
3. Zvýšiť `@version` v hlavičke — **inak Tampermonkey aktualizáciu nestiahne**
   (aktualizuje sa z `@updateURL`, teda z vetvy `main` tohto repozitára)
4. `node --check production/pda-suite.user.js`
5. Commit a push

Modul sa automaticky objaví v paneli nastavení. Ak niektorý modul spadne,
ostatné bežia ďalej — spúšťajú sa každý vo vlastnom `try/catch`.

---

Pôvodný základ pochádza z repozitára `Dan1elG94/HF-Slovakia-PDA-scripts`.
