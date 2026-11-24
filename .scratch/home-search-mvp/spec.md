# Home Search MVP: specyfikacja (szkic)

**Status:** needs-info
**Etap:** MVP, po POC (Q35)

Źródłem prawdy jest `docs/decisions.md`; gdy coś się z nim kłóci, wygrywa `docs/decisions.md`. Numery Q odsyłają do sesji grillowania. Słownictwo: `GLOSSARY.md`. Pięć kwestii jest otwartych i ma własne zadania (patrz Further Notes). Specyfikacja nie proponuje dla nich rozwiązań.

## Problem Statement

Kupujący szuka większego mieszkania, szeregowca, bliźniaka lub domu. Chce, żeby ktoś wyszukiwał dla niego dobre ogłoszenia na rzeczywistym rynku i dzwonił do ludzi z tych ogłoszeń, żeby dowiedzieć się czegoś więcej. Okolica jest dla niego ważna, ale świetny dom poza nią też go interesuje.

To także projekt do portfolio: ma pokazać system AI, którego jakość została zmierzona, i ma zostać dowieziony.

## Solution

Mini dashboard działający lokalnie, z czterema ekranami:

1. **Brief**: kupujący opisuje potrzeby własnymi słowami, Claude zamienia to na formularz, kupujący poprawia i zatwierdza.
2. **Ogłoszenia**: kupujący widzi rzeczywiste Listingi i to, które są Good Listing dla jego Briefu.
3. **Zatwierdzenie telefonu**: żaden telefon nie wychodzi bez zgody kupującego.
4. **Wynik rozmowy**: fakty zebrane po polsku przez agenta głosowego, z cytatami.

W MVP agent dzwoni do rodziny, która gra z ukrytej karty faktów. Oba etapy, wybór ogłoszeń i rozmowa, mają ewaluację z ustalonymi celami.

## User Stories

### Brief

1. As a kupujący, I want opisać własnymi słowami, czego szukam, so that nie muszę zaczynać od formularza. (Q27)
2. As a kupujący, I want zobaczyć mój opis zamieniony przez Claude'a na formularz, so that widzę, jak zostałem zrozumiany. (Q27)
3. As a kupujący, I want poprawić formularz, so that Brief oddaje to, czego naprawdę szukam. (Q27)
4. As a kupujący, I want zatwierdzić Brief, so that dalsza praca opiera się na kryteriach, które potwierdziłem. (Q27)
5. As a kupujący, I want podać okolicę jako jedno z kryteriów, so that świetny dom poza nią też może przejść. (Q29)
6. As a kupujący, I want szukać większego mieszkania, szeregowca, bliźniaka lub domu, so that widzę każdy rodzaj Property, który biorę pod uwagę. (Q2)
7. As a kupujący, I want mieć Brief na osobnym ekranie, so that łatwo go znaleźć. (Q28)

### Ogłoszenia

8. As a kupujący, I want widzieć rzeczywiste Listingi z rynku, so that mogę naprawdę znaleźć dom. (Q24, Q26)
9. As a kupujący, I want wiedzieć, które Listingi są Good Listing dla mojego Briefu, so that zajmuję się tylko wartymi uwagi. (Q26)
10. As a kupujący, I want żeby ocena uwzględniała zgodność z moimi kryteriami, so that wynik pasuje do moich potrzeb. (Q26)
11. As a kupujący, I want żeby ocena uwzględniała atrakcyjność ceny, so that nie tracę czasu na przepłacone Property. (Q26)
12. As a kupujący, I want żeby ocena uwzględniała, jak nowe jest ogłoszenie, so that nie oglądam starych Listingów. (Q26)
13. As a kupujący, I want zobaczyć świetny Listing spoza mojej okolicy, so that nie przegapię wyjątkowego domu. (Q29)
14. As a kupujący, I want mieć Listingi na osobnym ekranie, so that mogę je przejrzeć przed decyzją o telefonie. (Q28)

### Zatwierdzenie telefonu

15. As a kupujący, I want zatwierdzać każdy telefon ręcznie, so that agent nigdy nie dzwoni bez mojej zgody. (Q5)
16. As a kupujący, I want nie zatwierdzić telefonu, so that rozmowa się nie odbędzie. (wynika z Q5)
17. As a kupujący, I want zatwierdzać telefony na osobnym ekranie, so that decyzja jest świadoma. (Q28)
18. As a kupujący, I want żeby numer z listy blokad nie mógł zostać wybrany, so that szanujemy czyjś sprzeciw. (wynika z Q6)
19. As a kupujący, I want żeby agent dzwonił z prawdziwego numeru w Twilio, so that telefon działa jak w rzeczywistości. (Q21)
20. As a kupujący, I want najpierw dzwonić do rodziny, so that sprawdzę agenta, zanim zadzwoni do Advertiserów. (Q4)

### Rozmowa

21. As a rozmówca, I want od razu usłyszeć, że dzwoni AI, so that wiem, z czym rozmawiam. (Q6)
22. As a rozmówca, I want usłyszeć, w czyim imieniu dzwoni agent, so that wiem, kto pyta. (Q6)
23. As a rozmówca, I want usłyszeć, po co agent dzwoni, so that mogę zdecydować, czy chcę rozmawiać. (Q6)
24. As a rozmówca, I want rozmawiać po polsku, so that rozmowa jest dla mnie naturalna. (Q8)
25. As a rozmówca, I want żeby agent nie udawał człowieka, so that nie zostaję wprowadzony w błąd. (Q6)
26. As a rozmówca, I want żeby agent nie negocjował ze mną, so that to tylko rozmowa o informacjach. (Q6)
27. As a rozmówca, I want żeby agent nie składał ofert, so that nic nie zostaje zaproponowane w imieniu kupującego. (Q6)
28. As a rozmówca, I want żeby agent nie umawiał niczego wiążąco, so that rozmowa do niczego nas nie zobowiązuje. (Q6)
29. As a rozmówca, I want powiedzieć „proszę nie dzwonić”, so that agent kończy rozmowę. (Q6)
30. As a rozmówca, I want żeby mój numer trafił wtedy na listę blokad, so that agent nie zadzwoni ponownie. (Q6)
31. As a rozmówca, I want żeby audio nie było zapisywane, so that nie zostaje nagranie mojego głosu. (Q18)
32. As a rozmówca, I want żeby transkrypcja była usuwana po 30 dniach, so that treść rozmowy nie zostaje na zawsze. (Q18)

### Wynik rozmowy

33. As a kupujący, I want zobaczyć fakty z rozmowy na osobnym ekranie, so that dowiaduję się o Property więcej niż z Listingu. (Q18, Q28)
34. As a kupujący, I want widzieć przy każdym fakcie `odpowiedź`, `nie wie` albo `odmówił`, so that odróżniam brak wiedzy od odmowy. (Q18)
35. As a kupujący, I want widzieć cytat przy fakcie, so that mogę sprawdzić, skąd się wziął. (Q18)
36. As a kupujący, I want żeby fakty zostały po usunięciu transkrypcji, so that nie tracę zebranych informacji. (wynika z Q18)

### Rozmowy z rodziną

37. As a członek rodziny, I want zgodzić się wcześniej na udział, so that telefon od agenta mnie nie zaskoczy. (Q17)
38. As a członek rodziny, I want grać z ukrytej karty faktów, so that odpowiedzi agenta można porównać z prawdą. (Q17)
39. As a członek rodziny, I want żeby transkrypcja mojej rozmowy nie była publikowana, so that moja rozmowa zostaje prywatna. (Q17)

### Ewaluacja wyboru ogłoszeń

40. As a oceniający, I want oznaczyć ok. 40 Listingów jako „zadzwoniłbym / nie / nie wiem”, zanim zobaczę ocenę Claude'a, so that moja ocena nie jest nią zasugerowana. (Q16)
41. As a oceniający, I want używać 20 Listingów do strojenia i odłożyć 20, so that wynik końcowy jest uczciwy. (Q16)
42. As a oceniający, I want mierzyć poprawność faktów, so that wiem, czy Claude dobrze czyta Listingi. (Q16)
43. As a oceniający, I want mierzyć odsetek dobrych Listingów, które przeszły, z celem ≥ 90%, so that wiem, ile dobrych przepada. (Q16)
44. As a oceniający, I want mierzyć trafność `call`, so that wiem, czy polecone Listingi są warte telefonu. (Q16)
45. As a oceniający, I want widzieć liczby obok procentów, so that mała próba nie wprowadza w błąd. (Q16)

### Ewaluacja rozmowy

46. As a oceniający, I want uruchomić 12 symulowanych scenariuszy po 3 razy, so that wynik nie zależy od jednej próby. (Q17)
47. As a oceniający, I want przeprowadzić min. 5 rozmów z rodziną, so that agent jest sprawdzony w prawdziwej rozmowie telefonicznej. (Q17)
48. As a oceniający, I want żeby ujawnienie AI i obsługa sprzeciwu działały w 100% przypadków, so that te zasady nigdy nie zawodzą. (Q17)
49. As a oceniający, I want żeby liczba zakazanych obietnic wynosiła 0, so that agent nie przekracza swojej roli. (Q17)
50. As a oceniający, I want żeby fakty zgadzały się z kartą w ≥ 90%, so that mogę ufać wynikom rozmów. (Q17)
51. As a oceniający, I want żeby scenariusze obejmowały niechętnego rozmówcę, pocztę głosową, agencję i sprzeczne odpowiedzi, so that widzę, gdzie agent zawodzi. (Q7)

### Portfolio

52. As a czytelnik portfolio, I want zobaczyć zbiorcze wyniki ewaluacji, so that wiem, czy system działa. (Q16)
53. As a czytelnik portfolio, I want uruchomić ewaluację na zestawie syntetycznym, so that mogę sprawdzić ją bez prywatnych danych. (wynika z Q16)
54. As a czytelnik portfolio, I want obejrzeć demo z symulacjami i osobami, które zgodziły się na publikację, so that nikt nie jest pokazany bez zgody. (Q4)
55. As a czytelnik portfolio, I want żeby w demo było fikcyjne nazwisko, so that dane kupującego zostają prywatne. (Q6)
56. As a czytelnik portfolio, I want czytać kod po angielsku, so that mogę go ocenić niezależnie od języka rozmów. (Q8)

### Właściciel projektu

57. As a właściciel projektu, I want uruchamiać MVP lokalnie, so that hosting mogę odłożyć. (Q22, Q35)
58. As a właściciel projektu, I want zmieścić się w 200 zł miesięcznie, so that projekt nie przekracza budżetu. (Q10)
59. As a właściciel projektu, I want żeby w publicznym repo nie było danych rodziny, adresów, kwot zakupu ani numerów telefonów, so that prywatne dane zostają prywatne. (`docs/decisions.md`)
60. As a właściciel projektu, I want widzieć kolejność prac bez sztywnych dat, so that śledzę postęp. (Q34)

## Implementation Decisions

- **Backend:** Python, FastAPI, SQL; w MVP SQLite. (Q9, Q22, Q28)
- **Frontend:** Next.js, TypeScript, Tailwind, ElevenLabs UI, od razu w docelowym stacku. (Q28, Q36)
- **Podział ról:** Claude wyszukuje i ocenia, ElevenLabs Agents prowadzi rozmowę. (Q9)
- **Bez frameworka do agentów.** (Q22)
- **Po rozmowie** aplikacja odpytuje API zamiast odbierać webhook. (Q22)
- **Model w rozmowie** wybiorą wyniki ewaluacji. (Q22)
- **Telefonia:** Twilio i prawdziwy numer. (Q21)
- **Zatwierdzenie:** telefon wychodzi tylko po ręcznym zatwierdzeniu i nigdy na numer z listy blokad. Zasady pilnuje backend, więc da się ją sprawdzić przez jego API. (Q5, Q6; wynika z decyzji o testach)
- **Fakt z rozmowy** ma status `odpowiedź`, `nie wie` albo `odmówił` oraz cytat. (Q18)
- **Przechowywanie:** bez audio; transkrypcja usuwana po 30 dniach. (Q18)
- **Obszary:** Brief, Listing, Score, rozmowa, ewaluacja, dashboard. Interfejsy między nimi, schemat danych i kontrakty API nie są ustalone, bo zależą od otwartych zadań.

## Testing Decisions

- **Jedno miejsce testów: API backendu**, wywoływane tak, jak robi to dashboard. Claude, ElevenLabs i źródło Listingów są w testach podstawione: nic nie kosztuje i nikt nie dzwoni.
- **Dobry test** sprawdza zachowanie widoczne z zewnątrz, a nie szczegóły implementacji.
- **Co testujemy:** Brief od opisu do zatwierdzenia, zatwierdzanie telefonu, listę blokad, zapis faktów z rozmowy, usuwanie transkrypcji po 30 dniach. Testy Listing i Score powstaną po rozstrzygnięciu ich zadań.
- **Testy to nie ewaluacje.** Testy sprawdzają nasz kod. Jakość Claude'a i agenta głosowego mierzą ewaluacje (Q16, Q17).
- **Prior art:** brak, w repo nie ma jeszcze kodu.
- **Na później, poza MVP** (pomysł właściciela, niezbadany): coś na kształt [software factory](https://simonwillison.net/2026/Feb/7/software-factory/) albo Meticulous AI.

## Out of Scope

- **Pilot:** hosting, testy innych użytkowników, prawdziwi Advertiserzy (potwierdzone przez właściciela). (Q35)
- **Produkcja:** codzienne użycie, monitoring. (Q35)
- Osobny Brief drugiego użytkownika. (Q32)
- Najem. (wynika z Q2)
- Webhook i framework do agentów. (Q22)
- Odrzucone: dane.gov.pl, sztywne filtrowanie po okolicy, dodatkowe progi przed pierwszym prawdziwym telefonem, odkładanie prawdziwych telefonów do przyszłego roku, samo CLI, testy na przypadkowych numerach, porównywanie oznaczeń dwóch osób, przegląd prawny przed prawdziwymi telefonami.

## Further Notes

### Otwarte kwestie

Właściciel projektu decyduje, które zadanie i kiedy. Specyfikacja może przejść na `ready-for-agent` dopiero po ich rozstrzygnięciu.

| Zadanie | Pytanie | Od tego zależą |
|---|---|---|
| [Listing](issues/01-listing.md) | Skąd i jak brać rzeczywiste Listingi? | Historie 8-14 i 40-45 |
| [Score](issues/02-score.md) | Jak podzielić ocenę między Claude'a i kod? | Historie 9-13 i 40-45 |
| [Numer telefonu](issues/03-phone-number.md) | Jak zdobyć i skonfigurować numer? | Historie 19, 20 i 47 |
| [Przebieg rozmowy](issues/04-conversation.md) | O co agent pyta i jak prowadzi rozmowę? | Historie 21-30, 33-38 i 46-51 |
| [Sekrety i awarie](issues/05-secrets-and-failures.md) | Jak chronimy dostępy i co robimy, gdy coś zawiedzie? | Całe MVP |

### Niejasność do rozstrzygnięcia

**Sekrety i awarie:** etapy z Q35 umieszczają je w Produkcji, a tutaj są otwartą kwestią MVP. Podział rozstrzygnie zadanie 05.
