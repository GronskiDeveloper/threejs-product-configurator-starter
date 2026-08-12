# Praca AI-first — notatki dla tego repo

Trzymam ten plik w repozytorium, ponieważ buduję z Claude Code (Anthropic) i chcę, żeby podział „człowiek/AI" był widoczny z drzewa plików, a nie deklarowany w README. Rekruter, klient albo kolega z zespołu ma tu dowody, nie ogólniki.

## Podział pracy człowiek vs AI

| Warstwa | Kto zrobił | Dlaczego tak |
|---|---|---|
| Architektura i decyzja o formacie | **Człowiek** — zdecydowałem, że to *starter* (jeden plik, importmap, bez build stepu), a nie framework. Cały sens tego repo to „otwórz `index.html`, działa". | Zła forma tutaj zabija „wow w 5 sekund". Nie do zlecenia AI. |
| Setup sceny Three.js (renderer, kamera, OrbitControls, oświetlenie) | **Draft AI, weryfikacja człowieka** | To boilerplate, który Claude pisze poprawnie; moim zadaniem było dobrać rig oświetlenia pod podgląd materiałów i odrzucić standardowe 3-punktowe, które maskuje reakcję materiału. |
| Parametryczna geometria + wycena na żywo | **Design człowieka, kod AI** | Flow danych (stan → przebudowa mesha → przeliczenie ceny) mój; pętla dispose'ująca starą geometrię (żeby uniknąć memory leaka WebGL) — Claude. |
| UI (panel kontrolek, licznik ceny) | **Draft AI, style człowieka** | Trzymałem dependency-free vanilla JS na siłę — bez Reacta, bez Tailwinda — żeby dało się to sforkować i wrzucić do motywu WordPress. |
| README + backlinki do grodev.pl | **Człowiek** | Decyzje marketingowe zostają u mnie. |

## Co zweryfikowałem przed wypchnięciem

- Otworzyłem `index.html` w Chrome + Firefox — bez błędów w konsoli, bez problemów CORS z importmapy.
- Podmieniłem parametry w konsoli DevTools (`window.state.width = 3.5; update()`) — potwierdziłem, że nie ma martwych referencji między przebudowami.
- Odpalony build GitHub Pages — potwierdziłem, że działa bez build stepu (o to chodzi).

## Znane pułapki dla następnej iteracji AI

- **Nie dodawać bundlera.** Importmapa + `<script type="module">` to cały pomysł. Jeśli Claude zasugeruje Vite/webpack — odrzucić.
- **Nie zamieniać OrbitControls na TrackballControls** — TrackballControls nie ma auto-inertia stop, na urządzeniach dotykowych zachowuje się jak pijany.
- **Dispose'ować geometrię przed podmianą** (`old.geometry.dispose()`) — inaczej context WebGL cieknie, karta browsera dobija do ~500 MB po 20 zmianach konfiguracji.
- Aktualizacje koloru materiału muszą być przez `.set(hex)` na istniejącym `MeshStandardMaterial`, **nie** przez przypisanie nowego materiału (psuje fog i binding cieni).

## Kiedy sięgać po Claude na tym projekcie, a kiedy pisać samodzielnie

- **Sięgnąć po Claude:** nowy preset (inny kształt produktu — biurko, doniczka, lampa), nowy typ materiału (szkło, szczotkowany metal), nowy format eksportu.
- **Zrobić samodzielnie:** cokolwiek dotykającego logiki wyceny. Ceny to reguła biznesowa; pozwolenie AI na improwizację tam to sposób na wypuszczenie buga, który kosztuje prawdziwego klienta.
