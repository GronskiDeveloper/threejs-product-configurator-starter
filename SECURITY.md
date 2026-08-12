# Zgłaszanie podatności

Bezpieczeństwo tego projektu jest dla mnie ważne — jeśli znalazłeś podatność, zgłoś ją **prywatnie** zamiast otwierać publicznego issue.

## Kanały zgłoszenia

- **Preferowany:** [Security Advisory na GitHubie](https://github.com/GronskiDeveloper/threejs-product-configurator-starter/security/advisories/new) (prywatny, tylko dla mnie do przejrzenia).
- **Alternatywnie:** e-mail bezpośrednio na **dominik@grodev.pl** z tematem `[SECURITY] threejs-product-configurator-starter`.

## Co warto zawrzeć w zgłoszeniu

- Opis podatności (co jest do wykorzystania, jak).
- Kroki reprodukcji (albo minimalny PoC).
- Ocena wpływu (co atakujący może zrobić — kradzież danych, wykonanie kodu, DoS itd.).
- Ewentualnie sugerowany fix.

## Reakcja

- **Potwierdzenie odbioru:** w ciągu 72h.
- **Wstępna ocena:** w ciągu 7 dni.
- **Fix + release:** zależnie od skali (krytyczne — priorytetowo).

Podziękuję imiennie w release notes / CHANGELOG (o ile nie prosisz o anonimowość).


## Kontekst tego projektu

To starter frontendowy — najkrytyczniejsze podatności to **XSS przez parametry URL** wpinane do sceny, **wycieki WebGL memory** (DoS zawieszający browser) i **zaufanie do ceny obliczanej po stronie klienta** (jeśli fork tego repo idzie do produkcji bez server-side wyceny — patrz `.claude/commands/webgl-review.md` punkt 6).

Autor: [Dominik Groński / GroDev](https://grodev.pl)
