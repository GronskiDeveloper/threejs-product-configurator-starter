# Case study - `threejs-product-configurator-starter`, minimal reference

Krótka retrospektywa: dlaczego napisałem *starter*, a nie kolejny gotowy konfigurator jednej kategorii, i dlaczego to jeden plik HTML zamiast Vite/Webpack setupa.

## Brief (30 sekund)

Minimalny konfigurator produktowy 3D w Three.js, zbudowany jako **starter template** - punkt wyjścia, nie produkt końcowy. Pokazuje pięć rzeczy, których każdy konfigurator musi mieć:

1. **Parametric geometry** - zmiana wymiaru w formularzu = zmiana kształtu w scenie
2. **PBR materials** - realistyczne wykończenia (drewno, metal matowy, chrom)
3. **Interactive controls** - obrót, zoom, click-to-select w scenie
4. **Live spec panel** - w formularzu widać aktualną konfigurację + cenę
5. **Export state** - JSON z aktualną konfiguracją (do wysłania na backend)

Trzy twarde wymagania:

1. **Zero build stepu.** `index.html` + Three.js z CDN. Otwierasz plik w przeglądarce, działa. Nie ma `npm install`, nie ma `vite dev`. Ktoś kto klonuje repo o 21:30 w piątek ma działający konfigurator o 21:32.
2. **Zero framework opinion.** Ma to zostać rozszerzone w React, Vue, Svelte, plain JS - to jest **starter**, nie „React library". Framework wybierasz Ty w drugim commicie po forku.
3. **Realny model, nie kostka.** Startowy produkt to *biurko z regulowanymi wymiarami* (widoczna wartość edukacyjna), nie box z materiałem. Bo „przykład z Three.js docs" każdy widział; tutaj chcę pokazać domyślne UX konfiguratora.

## Gdzie faktycznie zdarzyła się praca człowieka

### 1. Wybór wzorca - starter, nie library (człowiek)

Pierwszy odruch: „napiszę bibliotekę Three.js dla configuratorów, wystawię na npm". Wróciłem z tego. Powody:

- Konfigurator jest domain-specific. Konfigurator biurek, drzwi, saun, pergoli - każdy ma inną geometrię, inne materiały, inną logikę wyceny. Wspólna biblioteka byłaby albo za ogólna (użytkownik i tak pisze wszystko od zera), albo za wąska (nie pasuje do 80% branż).
- Starter template to **odpowiedni poziom abstrakcji** dla konfiguratora. Ludzie klonują, wywalają moje UI, wstawiają swój produkt, mają działający konfigurator w tydzień zamiast miesiąca.

Wszystkie moje realne wdrożenia konfiguratorów ([9 subdomen 3D live na grodev.pl](https://grodev.pl/konfigurator-produktowy-3d)) startowały od bardzo podobnego szkieletu.

### 2. Wybór Three.js zamiast Babylon albo three-fiber (człowiek)

**Three.js** wygrywa dla starterów bo:
- Największa dokumentacja + community (Stack Overflow, r/threejs, forum)
- Minimum abstrakcji nad WebGL - nauczasz się grafiki 3D, nie framework specyfik
- `import from "three"` bez React zależności - działa w każdym stacku

**Three-fiber** byłby lepszy dla React apps, ale zamyka użytkownika w Reactowej myśli. **Babylon** ma lepsze editor tools, ale mniejszy community i większy bundle.

### 3. AI zrobił parametric mesh + PBR setup (Claude, ~40 min)

Kod `updateGeometry(width, depth, height)` który rebuilduje BoxGeometry z parametrów, PBR material setup (roughness, metalness, envMap), OrbitControls z limitami, GUI slider - to wszystko Claude Code. Trwało 40 minut z 2 iteracjami: (1) Claude domyślnie dodał lil-gui (external dep) - poprosiłem o vanilla HTML `<input type=range>` żeby zero-deps, (2) roughness map z texture zamiast wartości - poprosiłem o wartość liczbową (mniej pobrań).

### 4. Deliberate omissions (człowiek)

To co świadomie **nie ma** w starterze:

- Load / save z localStorage - bo różne wdrożenia mają różne backendy
- Multiple lights setup (studio 3-point) - dla biurka wystarczy ambient + directional
- Post-processing (bloom, DOF) - premature optimization dla większości branż
- Cennik logiki - bo cena zależy od modelu biznesowego klienta

## Gdzie to się sprawdza

- **Zamysł na własny konfigurator** - klonujesz, zamieniasz biurko na Twój produkt, tydzień pracy zamiast miesiąca
- **Nauka Three.js** - kompleto działający konfigurator z komentarzami w kodzie
- **Referencja architektoniczna** - pokazuje jak parametric geometry + UI panel + state export się łączą

## Kiedy nie ten starter

- Jeśli potrzebujesz production konfiguratora *dzisiaj* dla klienta - to jest fundament, dojście do produkcji to jeszcze 2-6 tygodni pracy (branding, integracja z ERP/sklepem, mobile QA, WCAG). Zobacz jak wyglądają [9 gotowych wdrożeń na grodev.pl](https://grodev.pl/konfigurator-produktowy-3d).
- Jeśli chcesz gotowy SaaS (Threekit, Cylindo, Variantic) - to nie to; to jest kod, którego jesteś właścicielem, nie miesięczna subskrypcja.

## Kontakt

Wdrożenie własnego konfiguratora produktowego 3D pod branding klienta (używając tego startera jako podstawy albo od zera): [dominik@grodev.pl](mailto:dominik@grodev.pl) · [grodev.pl/konfigurator-produktowy-3d](https://grodev.pl/konfigurator-produktowy-3d).

Starter MIT - forkuj, komercjalizuj, buduj na nim swój produkt.
