# Porównanie cen — sonem/deku

Strona informacyjna aplikacji integrującej się z **Allegro REST API**, przygotowana zgodnie
z wymogiem identyfikacji nagłówka `User-Agent` (art. 3.4.c Regulaminu REST API Allegro).

Celem tej strony jest umożliwienie administratorom Allegro jednoznacznej identyfikacji
właściciela oprogramowania oraz zapoznania się z celem i zakresem jego działania.

---

## Identyfikacja aplikacji

| | |
|---|---|
| **Nazwa aplikacji (zarejestrowana)** | Porównanie cen - sonem/deku |
| **Nagłówek User-Agent** | `Porownanie-cen-sonem-deku/1.0.0 (+https://github.com/UZUPELNIJ/porownanie-cen-info)` |
| **Aktualna wersja produkcyjna** | 1.0.0 |
| **Typ aplikacji** | Prywatna aplikacja desktopowa (Electron), używana wyłącznie przez właściciela |
| **Charakter** | Narzędzie wewnętrzne — nie jest udostępniane osobom trzecim ani publicznie |

> Nazwa w nagłówku `User-Agent` została znormalizowana do postaci ASCII bez spacji i ukośnika
> (`Porownanie-cen-sonem-deku`), ponieważ specyfikacja nagłówka HTTP nie dopuszcza spacji ani
> znaku `/` w członie nazwy. Odpowiada ona zarejestrowanej aplikacji „Porównanie cen - sonem/deku".

---

## Właściciel / operator

| | |
|---|---|
| **Podmiot** | Sonem |
| **Kontakt (administratorzy Allegro)** | sklep@deku.pl |
| **Obsługiwane konta sprzedażowe (własne)** | `sonem`, `sonem_pl`, `karmywsieci_pl`, `agdwsieci_pl`, `deku_pl`, `czarodziejeu` |
| **Sklepy internetowe** | https://deku.pl, https://agdwsieci.pl |

Aplikacja działa wyłącznie na **własnych kontach sprzedażowych** operatora. Nie pobiera ani nie
przetwarza danych innych sprzedawców ani kupujących.

---

## Cel działania aplikacji

Aplikacja służy do **zarządzania cenami własnych ofert** operatora rozproszonych na kilku kontach
Allegro oraz dwóch sklepach Shoper. Konkretnie:

- pobiera listę **własnych ofert** ze wszystkich kont operatora,
- scala je w jedną bazę produktów (kluczem jest EAN), aby uniknąć duplikatów tego samego towaru
  wystawionego na różnych kontach,
- umożliwia **hurtową zmianę cen** wielu ofert naraz (cena stała, rabat procentowy, marża od ceny
  zakupu, korekta kwotowa),
- obsługuje **promocje czasowe** — z automatycznym lub ręcznym wyłączeniem i przywróceniem cen bazowych.

Aplikacja **nie prowadzi scrapingu**, nie monitoruje cen konkurencji przez API Allegro i nie
generuje ruchu wykraczającego poza obsługę własnego katalogu ofert.

---

## Zakres wykorzystania Allegro REST API

Autoryzacja: **OAuth 2.0 Device Flow** (każde konto autoryzowane osobno przez właściciela).

| Operacja | Endpoint | Zakres |
|---|---|---|
| Odczyt własnych ofert | `GET /sale/offers` | odczyt |
| Odczyt karty produktu / EAN | `GET /sale/product-offers/{id}` | odczyt |
| Odczyt aktualnej ceny oferty | `GET /offers/{offerId}` | odczyt |
| Zmiana ceny własnej oferty | `PUT /offers/{offerId}/change-price-commands/{commandId}` | zapis |

Wymagane uprawnienia (scope): odczyt i zapis ofert sprzedażowych na kontach operatora
(`allegro:api:sale:offers:read`, `allegro:api:sale:offers:write`).

### Charakter ruchu
- Synchronizacja ofert uruchamiana ręcznie lub cyklicznie (domyślnie co 30 minut, w godz. 7:00–23:00).
- Stronicowanie zgodne z limitami API (100 ofert na stronę).
- Zmiany cen wysyłane pojedynczo, poleceniem `change-price-command`.
- Każde żądanie zawiera stały, whitelistowany nagłówek `User-Agent` (patrz wyżej).

---

## Przetwarzanie danych i prywatność

- Wszystkie dane (oferty, ceny, historia zmian, tokeny) przechowywane są **lokalnie** na komputerze
  operatora. Nic nie jest wysyłane do zewnętrznych serwerów ani chmury poza samym API Allegro/Shoper.
- Klucze i sekrety (`client_id`, `client_secret`, tokeny, dane logowania Shoper) trzymane są w
  lokalnym pliku konfiguracyjnym i **nie znajdują się w tym repozytorium**.
- Aplikacja nie udostępnia żadnych danych podmiotom trzecim.

---

## Zgodność

Aplikacja korzysta z Allegro REST API zgodnie z Regulaminem REST API Allegro, w tym z wymogiem
identyfikacji nagłówkiem `User-Agent` (art. 3.4.c). Treść nagłówka jest stała (zmienia się wyłącznie
numer wersji przy aktualizacji aplikacji).

## Kontakt

W sprawach dotyczących ruchu API generowanego przez tę aplikację prosimy o kontakt:
**bartula.michal1@gmail.com**
