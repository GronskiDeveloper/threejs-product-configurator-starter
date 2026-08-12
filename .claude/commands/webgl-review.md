---
description: Recenzja zmian dotykających WebGL/Three.js pod kątem wycieków i regresji
---

Recenzujesz zmianę w `index.html` (albo dowolnym pliku dotykającym Three.js). WebGL wycieka po cichu — nie ma błędu w konsoli, użytkownik tylko zauważy, że po 20 zmianach konfiguracji karta browsera zjada 500 MB.

Zanim zaakceptujesz diff, upewnij się, że wszystkie sześć poniższych warunków zachodzi:

1. **Każda podmiana geometrii dispose'uje starą:** `oldMesh.geometry.dispose()` + `oldMesh.material.dispose()` (o ile materiał nie jest współdzielony). Bez tego wyciek WebGL.
2. **Aktualizacje koloru materiału przez `.set(hex)`, nie przez nowy `MeshStandardMaterial`.** Nowy materiał zrywa binding fog, cieni, environment map — psuje wygląd sceny po pierwszej zmianie.
3. **`renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))`** — nie hardkodowane `1` (piksele na Retinie) ani `window.devicePixelRatio` bez cap (na 4K przy 200% skalowania to 4× więcej pikseli, spadek FPS).
4. **Brak bundlera.** Cały sens repo to importmapa + `<script type="module">`. Jeśli diff wprowadza `package.json` z `vite`/`webpack`/`rollup` — odrzucić.
5. **OrbitControls, nie TrackballControls.** TrackballControls nie ma auto-inertia stop, na dotyku zachowuje się dziwnie (przewijanie mesha nie zatrzymuje się na palcach).
6. **Logika wyceny jest po stronie klienta *tylko do wyświetlania*.** Realna wycena musi być po stronie serwera. Jeśli diff dodaje wagi cenowe bezpośrednio do UI bez adnotacji „do zastąpienia w produkcji" — odrzucić (bo klient da 100 zł zamiast 10 000 zł, edytując DOM w DevTools).

Jeśli którykolwiek warunek pęka — zablokuj zmianę.

Sprawdź też ręcznie: otwórz `index.html`, przeprowadź 20 kolejnych zmian parametru, monitoruj Memory tab w DevTools. Jeśli detached WebGL contexts rośnie linearnie — masz wyciek, i któryś dispose jest zapomniany.
