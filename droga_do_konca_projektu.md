# Droga do końca projektu

Odhaczaj po kolei. Decyzje: [docs/decisions.md](docs/decisions.md). Specyfikacja i zadania: [.scratch/home-search-mvp](.scratch/home-search-mvp/spec.md).

## Trzy zasady

- **Jedna sesja = jedno zadanie.**
- **`/clear` dopiero po zapisaniu wyniku i commicie.**
- **Nie wiesz, co dalej?** Weź pierwszą nieodhaczoną rzecz z tej listy.

## Etap 0: Plan

- [ ] Przeczytać [decisions.md](docs/decisions.md): czy to moje decyzje, czy czegoś brakuje
- [ ] Przejrzeć [GLOSSARY.md](GLOSSARY.md)
- [ ] Przeczytać [spec.md](.scratch/home-search-mvp/spec.md): Problem Statement, Solution, Out of Scope, User Stories (uważnie 16, 18, 36 i 53), Implementation i Testing Decisions
- [ ] Przejrzeć „Do ustalenia” w pięciu [zadaniach](.scratch/home-search-mvp/issues/)
- [ ] Wysłać uwagi w sesji „planowanie i spece” (`claude --resume`)
- [ ] Poprawki, „commit i push”, `/clear`

## Etap 1: POC, czyli czy to w ogóle działa

Kolejność polecana; można ją zmienić.

- [ ] [01 Listing](.scratch/home-search-mvp/issues/01-listing.md): kilka dni i wiele sesji
- [ ] [03 Numer telefonu](.scratch/home-search-mvp/issues/03-phone-number.md): można równolegle z 01
- [ ] [02 Score](.scratch/home-search-mvp/issues/02-score.md)
- [ ] [04 Przebieg rozmowy](.scratch/home-search-mvp/issues/04-conversation.md), razem z pierwszą rozmową
- [ ] [05 Sekrety i awarie](.scratch/home-search-mvp/issues/05-secrets-and-failures.md): co w MVP, a co w Produkcji

Jak robić jedno zadanie:

1. Nowa sesja i polecenie (podmień plik zadania):
   > /mattpocock-skills:grill-with-docs Zadanie .scratch/home-search-mvp/issues/01-listing.md. Przeczytaj je, spec.md obok, docs/decisions.md i GLOSSARY.md. Zacznij od moich pomysłów, bez researchu i kodu bez mojego polecenia. Na koniec zapisz ustalenia w zadaniu i w docs/decisions.md.
2. Sam zlecam research (`/mattpocock-skills:research`) i małe prototypy, jeśli są potrzebne.
3. Decyzja zapisana w zadaniu i w `docs/decisions.md`, commit, `/clear`.

**Koniec etapu:** wszystkie pięć zadań jest rozstrzygniętych.

- [ ] `/mattpocock-skills:retro` w ostatniej sesji, przed `/clear`

## Etap 2: MVP, czyli cała aplikacja lokalnie

- [ ] Uzupełnić braki do specyfikacji:
  > /mattpocock-skills:grill-with-docs Przygotowujemy specyfikację MVP. Przeczytaj .scratch/home-search-mvp/, docs/decisions.md, GLOSSARY.md i kod prototypów. Pytaj tylko o to, czego brakuje. Nie zmieniaj podjętych decyzji i nie rób researchu bez mojego polecenia.
- [ ] Zaraz potem, w tej samej sesji, uzupełnić specyfikację:
  > /mattpocock-skills:to-spec Uzupełnij .scratch/home-search-mvp/spec.md na podstawie tej rozmowy, docs/decisions.md i rozstrzygniętych zadań. Czego nie ustaliliśmy, oznacz jako otwarte; nie zgaduj.
- [ ] W tej samej sesji podzielić ją na zadania do budowy: `/mattpocock-skills:to-tickets`
- [ ] Budować: `/mattpocock-skills:implement` na jedno zadanie, z testami i review, `/clear`, następne
- [ ] Oznaczyć ok. 40 ogłoszeń **przed** zobaczeniem oceny Claude'a (20 do strojenia, 20 odłożonych)
- [ ] Uruchomić 12 scenariuszy rozmowy po 3 razy
- [ ] Zrobić min. 5 rozmów z rodziną
- [ ] Poprawiać i mierzyć ponownie, aż osiągnę ustalone cele
- [ ] Opublikować README z wynikami i demo
- [ ] **Od tego momentu można wysłać aplikację o pracę**
- [ ] `/mattpocock-skills:retro` w ostatniej sesji, przed `/clear`

## Etap 3: Pilot, czyli inni użytkownicy i prawdziwe rozmowy

- [ ] Krótka specyfikacja, zadania, budowa hostingu
- [ ] Testy: nietechniczna osoba z rodziny i kolega; poprawki według ich uwag
- [ ] Telefony do prawdziwych ogłoszeniodawców, każdy zatwierdzony ręcznie
- [ ] `/mattpocock-skills:retro` w ostatniej sesji, przed `/clear`

## Etap 4: Produkcja, czyli codzienne użycie

- [ ] Monitoring, obsługa awarii, sekrety i koszty
- [ ] Codzienne używanie, zapisywanie wyników, uzupełnianie README
- [ ] `/mattpocock-skills:retro` w ostatniej sesji, przed `/clear`
