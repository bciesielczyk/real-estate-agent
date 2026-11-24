# Ustalenia

Decyzje z sesji grillowania (2026-11-24, rundy 1-5). Numery Q odsyłają do pytań z rozmowy.
Plik jest publiczny: bez danych rodziny, adresów, kwot zakupu i numerów telefonów.
Zmiana decyzji: popraw wpis i dopisz, co było wcześniej i dlaczego się zmieniło.

## Cel

- Projekt do portfolio (Applied AI Engineer, ElevenLabs lub podobne) i prawdziwe narzędzie do znalezienia domu. Gdy się kłócą, wygrywa portfolio. Zasada KISS. (Q1, Q4)
- Pierwszy z serii projektów; najważniejsze jest **dowiezienie**. (Q10)
- Dwie najważniejsze rzeczy: (1) Claude dobrze wyszukuje oferty, (2) agent głosowy dobrze działa. (Q13)
- Kupno: większe mieszkanie, szeregowiec, bliźniak lub dom. (Q2)
- Limity: 3-4 h dziennie, 200 zł miesięcznie. (Q10)
- Nic nie tniemy: lepiej dłużej, ale porządnie. Bez sztywnych dat, za to z kolejnością, po której widać postęp. (Q23, Q34)

## Wybór ogłoszeń

- Dobra oferta = zgodność z kryteriami + atrakcyjna cena + w miarę nowe ogłoszenie. (Q26)
- Okolica to jedno z kryteriów; świetny dom poza nią też może przejść. (Q29)
- Użytkownik opisuje potrzeby własnymi słowami, Claude zamienia to na formularz, użytkownik poprawia i zatwierdza (Brief). Na początek; potem zmiany po uwagach użytkowników. (Q27)
- Interesują nas rzeczywiste ogłoszenia z rynku. (Q24, Q26)

## Rozmowy

- Rozmowy po polsku. (Q8)
- Najpierw dzwonimy do rodziny, docelowo do prawdziwych ogłoszeniodawców. (Q4)
- Prawdziwi ogłoszeniodawcy dopiero w Pilocie, z realnym zamiarem zakupu. Wcześniej (Q12): możliwe już teraz; zmienione przy pisaniu specyfikacji. (Q35)
- Każdy telefon zatwierdza człowiek. (Q5)
- Agent na początku mówi, że jest AI, w czyim imieniu i po co dzwoni. Nie udaje człowieka, nie negocjuje, nie składa ofert, nie umawia wiążąco. Na „proszę nie dzwonić” kończy i zapisuje numer na liście blokad. Prawdziwe nazwisko przy prawdziwych telefonach, fikcyjne w demo. (Q6)
- Po rozmowie zostają fakty (`odpowiedź` / `nie wie` / `odmówił`) z cytatem. Bez audio; transkrypcja usuwana po 30 dniach. (Q18)
- Twilio i prawdziwy numer; w razie potrzeby kupujemy. (Q21)

## Ewaluacja

- Oceniamy oba etapy: wybór ogłoszeń i rozmowę. (Q7)
- Wybór: ok. 40 ogłoszeń oznaczonych przed zobaczeniem oceny Claude'a, 20 do strojenia i 20 odłożonych. Obok procentów podajemy liczby, bo próba jest mała. Oznaczenie to zapis „zadzwoniłbym / nie / nie wiem”. Poprawność faktów, odsetek dobrych ofert, które przeszły (cel ≥ 90%), trafność `call`. W repo tylko wyniki zbiorcze i zestaw syntetyczny. (Q16)
- Scenariusze obejmują: niechętny rozmówca, poczta głosowa, agencja, sprzeczne odpowiedzi. (Q7)
- Rozmowa: 12 symulowanych scenariuszy po 3 razy i min. 5 rozmów z rodziną, która gra z ukrytej karty faktów. Ujawnienie AI i sprzeciw 100%, zakazane obietnice 0, zgodność faktów ≥ 90%. Zgoda wcześniej, transkrypcji nie publikujemy. (Q17)
- Publiczne demo tylko z symulacjami i osobami, które zgodziły się na publikację. (Q4)

## Użytkownicy

- Testują: nietechniczna osoba z rodziny (Q20) i kolega, który podsunął pomysł (Q3).

## Technologia

- Kod po angielsku. (Q8)
- Python i SQL; Claude do wyszukiwania i oceny, ElevenLabs Agents do rozmowy. (Q9)
- Backend FastAPI; UI od razu w docelowym stacku: Next.js, TypeScript, Tailwind, ElevenLabs UI. (Q28, Q36)
- Mini dashboard, na początek cztery ekrany: Brief, ogłoszenia, zatwierdzenie telefonu, wynik rozmowy. (Q28)
- MVP: SQLite, bez frameworka do agentów, odpytywanie API po rozmowie zamiast webhooka. Hosting (AWS lub inny) później. (Q22)
- Model w rozmowie wybiorą wyniki ewaluacji. (Q22)
- Automatyczne testy na razie przez API backendu; Claude, ElevenLabs i źródło ogłoszeń podstawione. Później coś na kształt [software factory](https://simonwillison.net/2026/Feb/7/software-factory/) albo Meticulous AI. (przy pisaniu specyfikacji)

## Etapy (Q35)

- [ ] **POC**: czy to w ogóle działa? Listing, Score, numer i pierwsza rozmowa, każde osobno.
- [ ] **MVP**: czy całość daje wartość? Pełny przepływ w dashboardzie, telefony do rodziny, ewaluacje.
- [ ] **Pilot**: czy działa u innych? Hosting, testujący użytkownicy, prawdziwi ogłoszeniodawcy.
- [ ] **Produkcja**: czy można na tym polegać? Codzienne użycie, monitoring, awarie, sekrety.

## Osobne zadania

Praca iteracyjna, małymi częściami. Właściciel projektu decyduje, które zadanie i kiedy.

- [ ] [**01 Listing**](../.scratch/home-search-mvp/issues/01-listing.md) (Q24): skąd brać rzeczywiste ogłoszenia. Zaczynamy od pomysłów właściciela; prototypy do wypróbowania.
- [ ] [**02 Score**](../.scratch/home-search-mvp/issues/02-score.md) (Q25): jak podzielić ocenę między Claude'a i kod.
- [ ] [**03 Numer telefonu**](../.scratch/home-search-mvp/issues/03-phone-number.md) (Q31): jak zdobyć i skonfigurować.
- [ ] [**04 Przebieg rozmowy**](../.scratch/home-search-mvp/issues/04-conversation.md): o co agent pyta i jak rozmawia.
- [ ] [**05 Sekrety i awarie**](../.scratch/home-search-mvp/issues/05-secrets-and-failures.md): jak chronimy dostępy i co robimy, gdy coś zawiedzie.

## Odrzucone

- dane.gov.pl jako źródło: nie są to rzeczywiste ogłoszenia z rynku. (Q24, Q26)
- Sztywne filtrowanie po okolicy przed telefonem. (Q29)
- Dodatkowe progi przed pierwszym prawdziwym telefonem (wyniki Q17, 5 rozmów, test numeru, klauzula, limit dzienny). (Q30)
- Odkładanie prawdziwych telefonów do przyszłego roku. (Q12)
- Samo CLI w pierwszej wersji. (Q19)
- Testy na przypadkowych dostępnych numerach. (Q21)
- Research z `docs/research/feasibility.md` jako wkład do zadań: jest tylko do wglądu. (Q38)
- Porównywanie oznaczeń dwóch osób: każda wybierze co innego. (Q32)
- Przegląd prawny (PKE, RODO) przed prawdziwymi telefonami.

## Na razie niepotrzebne

- Osobny Brief drugiego użytkownika. (Q32)
