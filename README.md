# Kolejki priorytetowe i kopce — interaktywna wizualizacja

Interaktywna aplikacja edukacyjna (HTML + CSS + JavaScript, bez zależności) prezentująca trzy implementacje **kolejki priorytetowej** i **kopca binarnego (min)**. Program działa w pełni lokalnie — wystarczy otworzyć plik `priority-queue-heaps.html` w przeglądarce. Bez serwera, bez API, bez kont odbiorców.

## Zawartość karty — kompletna realizacja

Wszystkie podtematy karty zrealizowane w **jednej aplikacji**:

| Podtemat | Realizacja |
|---|---|
| **Tablica nieuporządkowana** | Dopisanie elementu na koniec (O(1)); przy pobraniu skan całej tablicy w poszukiwaniu minimum (O(n)). |
| **Tablica uporządkowana** | Wstawianie w odpowiednie miejsce z widocznym kosztem porównań i przesunięć (O(n)); minimum zawsze na pozycji 0 (O(1)). |
| **Kopiec binarny minimum** | Wstawianie z przesiewaniem w górę (sift-up) i pobieranie minimum z przesiewaniem w dół (sift-down), oba O(log n). |

## Dane i zasady

- Do **31 zadań**.
- Każde zadanie ma **unikalny identyfikator** (auto-inkrementacja) oraz **priorytet 0–9** (mniejsza liczba = wcześniejsza obsługa).
- Przy równym priorytecie najpierw obsługiwane jest zadanie **wcześniej dodane** (niższy ID → FIFO).
- Wszystkie trzy warianty porównują **tę samą parę: `(priorytet, kolejność dodania)`** — funkcja `compareHeap(a, b)`.
- Operacje: **dodaj**, **pokaż minimum**, **pobierz minimum**. Zmiana priorytetu nie jest wymagana.

## Funkcje aplikacji

### 1. Interaktywny pokaz (`Krok` / `Start/Pauza` / `Reset`)
- **Krok** — wykonuje pojedynczy krok algorytmu (jedno porównanie / swap / przesunięcie).
- **Start / Pauza** — odtwarza sekwencję kroków automatycznie.
- **Reset** — zeruje bieżącą strukturę.
- **Zmiana tempa** — prędkość animacji 0.3 s / 0.5 s / 1 s / 1.5 s / 2 s.
- **3 gotowe scenariusze** plus **własne dane użytkownika** (ręczne dodawanie). Zmiana danych/scenariusza rozpoczyna nowy przebieg.

### 2. Rzeczywiste kroki (wykonanie z kodu, nie z animacji)
Każdy krok pochodzi z **faktycznego wykonania algorytmu** — algorytmy działają jako generatory (`function*`) zwracające kolejne kroki. Wyświetlany jest:

- **stan struktury** (tablica / drzewo kopca),
- **aktywne elementy** (wyróżnione komórki/węzły),
- **zmienne** (bieżące porównania, swapy, przesunięcia, liczniki),
- **wyróżniony wiersz pseudokodu** (aktualnie wykonywana linia),
- **przyczyna działania** (tłumaczenie każdego kroku w logu).

### 3. Uczy odbiorcę
- Zakładka **Nauka** z **3 pytaniami i odpowiedziami z wyjaśnieniem**; przynajmniej jedno dotyczy aktualnego stanu (reguła równych priorytetów).
- Opisy po polsku.
- **Czytelna legenda i oznaczenia inne niż sam kolor** — symbole tekstowe (`✓`, `✗`, `#`, ikony operacji) oraz ramki w różnych kolorach.

### 4. Porównanie (z karty)
- Tabela porównawcza kosztów **czasowych i pamięciowych** (O(1), O(n), O(log n)).
- **Liczniki**: porównania, przesunięcia/swapy — mierzone na żywo.
- **Rozmiar wejścia**: n = liczba zadań (do 31).
- **Animacja**, **historia klatek** i **kod walidatora** są policzone/utrzymane **oddzielnie od badanego algorytmu** — walidator `Validator` to niezależna klasa nieużywająca kodu algorytmów z interfejsu.

### 5. Testy automatyczne — 10 testów
Każdy test ma zdefiniowane **wejście**, **oczekiwany wynik** i **status (pass/fail)**. Obejmują wszystkie metody i walidację, w tym **2 konkretne stany pośrednie**:

| # | Test | Wejście | Oczekiwany wynik |
|---|---|---|---|
| 1 | Unordered: ekstrakcja zwraca prawdziwe minimum | 31 losowych par | id = poprawne minimum |
| 2 | Ordered: utrzymuje porządek po 31 wstawieniach | 31 losowych par | posortowane wg (priority, id) |
| 3 | Heap: właściwość kopca po 31 wstawieniach | 31 losowych par | rodzic ≤ dzieci |
| 4 | Wszystkie 3 struktury: ten sam porządek ekstrakcji | te same 31 pary | 3 identyczne sekwencje ID |
| 5 | FIFO przy równym priorytecie (kopiec) | 5 zadań p=3 | kolejność 1,2,3,4,5 |
| 6 | **Stan pośredni**: korzeń kopca po 3 wstawieniach | #1/p5, #2/p3, #3/p7 | korzeń = #2 (p=3) |
| 7 | **Stan pośredni**: tablica uporządkowana po 3 wstawieniach | #1/p9, #2/p1, #3/p5 | [#2, #3, #1] |
| 8 | Heap: ekstrakcja + zachowana właściwość | 7 elementów | min=#4/p1, właściwość zachowana |
| 9 | Integralność danych: 31 zadań, poprawne ID | seed=12345 | 31 par, ID 1–31 |
| 10 | Zakres priorytetów 0–9 | 31 losowych par | wszystkie ∈ [0,9] |

Liczba testów jest minimum, nie limitem — w kodzie łatwo dodać kolejne.

## Technologia

- **HTML + CSS + JavaScript** (vanilla, zero bibliotek, zero zależności).
- Algorytmy są **w kodzie projektu** (generatory `function*` i funkcje) — żadna gotowa biblioteka nie zastępuje badanego algorytmu.
- Działa lokalnie — brak płatnego API i kont.

## Uruchomienie

1. Pobierz repozytorium (klon lub ZIP).
2. Otwórz `priority-queue-heaps.html` w dowolnej przeglądarce (Chrome, Firefox, Edge).

Lub przez GitHub Pages: włącz Pages na gałęzi `main`, plik jest gotowy jako strona główna.

## Struktura plików

```
.
├── priority-queue-heaps.html   # cała aplikacja (HTML + CSS + JS w jednym pliku)
└── README.md                   # ten plik
```

## Reguła porównania (sedno karty)

```javascript
// Para (priorytet, kolejność dodania)
function compareHeap(a, b) {
  return a.priority < b.priority                  // mniejszy priorytet wygrywa
      || (a.priority === b.priority && a.id < b.id); // przy równym — wcześniej dodany
}
```