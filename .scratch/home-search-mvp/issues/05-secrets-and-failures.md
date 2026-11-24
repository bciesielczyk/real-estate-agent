# 05: Sekrety i awarie

**Status:** needs-info
**Etap:** do ustalenia (Q35 umieszcza je w Produkcji)
**Blocked by:** None. Kolejność wybiera właściciel projektu.

## Pytanie

Jak chronimy dostępy do usług i co robi aplikacja, gdy coś zawiedzie?

## Już ustalone

- Repo jest publiczne: bez danych rodziny, adresów, kwot zakupu i numerów telefonów. (`docs/decisions.md`)
- `.gitignore` pomija pliki `.env`, nagrania, transkrypcje i katalog `data/`.
- MVP działa lokalnie; hosting później. (Q22, Q35)
- Limit 200 zł miesięcznie. (Q10)
- Sposób obsługi sekretów i awarii jest ważny dla portfolio. (Q10)

## Do ustalenia

- Jakie sekrety ma aplikacja i gdzie są trzymane?
- Które awarie obsługujemy w MVP?
- Co widzi wtedy kupujący?
- Jak pilnujemy limitu kosztów?
- Co należy do MVP, a co do Produkcji?

## Zasady

Bez researchu i prototypów, dopóki właściciel ich nie zleci.

## Gotowe, gdy

Decyzja jest w `docs/decisions.md`, a specyfikacja jest uzupełniona.

## Comments
