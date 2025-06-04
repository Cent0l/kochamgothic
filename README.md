# EmPeTrójniej / KochamGothic

**Teleturniej inspirowany grą Gothic** – prosta, przeglądarkowa aplikacja do wyświetlania pytań i odpowiedzi z możliwością wybrania avatara dla każdego pytania oraz zarządzania listą graczy z lampkami „Tura” i „Odpowiada”.

---

## Opis funkcjonalności

### 1. Sekcja „Gracze”

- **Dodawanie graczy**  
  - Kliknij przycisk **„Dodaj gracza”**, aby utworzyć nowego gracza z domyślnym imieniem (np. „Gracz 1”).  
  - Gracz pojawia się w liście z avatar­-kołem, imieniem, licznikiem punktów i przyciskami „–” oraz „+” do zmiany punktów.  
  - Po kliknięciu imienia gracza możesz je edytować poprzez pole tekstowe.

- **Avatar gracza**  
  - Każdy gracz ma obrazek w kształcie koła („avatar”).  
  - Kliknięcie w avatar otwiera picker wszystkich dostępnych obrazków z pliku `avatars.json`.  
  - Wybierz grafikę, aby ustawić nowy avatar. Zmiana jest zachowywana w lokalnej pamięci przeglądarki (localStorage).

- **Usuwanie graczy**  
  - Kliknij przycisk „X” obok wybranego gracza, aby usunąć go z listy.  

- **Lampki „Tura” i „Odpowiada”**  
  - Obok każdego gracza znajdują się dwie małe lampki (koliste znaczniki).  
    - Lampka „Tura” sygnalizuje, którego gracza jest kolej.  
    - Lampka „Odpowiada” sygnalizuje, który gracz właśnie odpowiada.  
  - Pod listą graczy znajdują się dwa przyciski:  
    1. **„Następna tura”** – przesuwa zapaloną lampkę „Tura” kolejno na następnego gracza (1 → 2 → 3 → … → 1).  
    2. **„Następny odpowiada”** – analogicznie przesuwa lampkę „Odpowiada”.  

---

### 2. Sekcja „Pytania” (Tabela 6×6)

- **Tabela kategorii**  
  - Wyświetlana jest macierz 6×6 komórek.  
  - Jeśli w pliku `questions.json` znajduje się wpis o indeksie `i < 36`, w danej komórce pokazuje się przycisk z nazwą kategorii (`category`).  
  - Jeśli nie ma wpisu na danym indeksie, pojawia się nieaktywny przycisk `???`.

- **Modal pytania/odpowiedzi**  
  - Po kliknięciu w przycisk danej kategorii wyświetla się własny modal (nakładka) na środku ekranu, pokazujący:
    1. Tytuł **„Pytanie”** oraz treść `question`.  
    2. Po kliknięciu przycisku **„Dalej”** w tym samym modal-u zmienia się na **„Odpowiedź”** z treścią `answer`.  
    3. Po kliknięciu **„Zamknij”** modal zostaje zamknięty, a w komórce tabeli pojawia się pusty avatar (`div.player-avatar`).  
    4. Jednocześnie otwierany jest avatar-picker, aby od razu wybrać grafikę.

- **Avatar w komórce pytania**  
  - Po zamknięciu modal-u w komórce pojawia się avatar (element `.player-avatar`).  
  - **Kliknięcie pojedyncze** w ten avatar otwiera avatar-picker (jak w sekcji graczy).  
  - **Dwukrotne kliknięcie** (`dblclick`) w avatar ponownie otwiera modal z tym samym pytaniem i odpowiedzią.

---

### 3. Avatar-picker (dla graczy i pytań)

- Dostępne grafiki zdefiniowane są w pliku `avatars.json` jako tablica ścieżek do plików obrazkowych (np. `images/bezi.png`, `images/diego.png`).  
- Picker wyświetla siatkę miniatur w stylu panelu nawigacyjnego.  
- Wybór dowolnej miniatury ustawia ją jako `background-image` elementu `.player-avatar` i zapisuje w `localStorage`, aby zachować wybrany avatar między odświeżeniami.

---

### 4. Lokalizacja danych

- Stan listy graczy i ich avatarów jest zapisywany w `localStorage`, dzięki czemu po odświeżeniu strony gracze pozostają zachowani.  
- Tabela pytań ładuje się za pomocą funkcji `fetch("questions.json")`; nie jest na razie zapisywana w `localStorage`.  

---

## Plik `questions.json` (przykład struktury)

Każdy obiekt w tablicy musi zawierać trzy pola:

```json
[
  {
    "category": "Pytanie 1",
    "question": "W którym roku została wydana pierwsza część gry Gothic?",
    "answer": "Pierwsza część Gothic została wydana w 2001 roku."
  },
  {
    "category": "Pytanie 2",
    "question": "Jak nazywa się główny bohater pierwszej części Gothic?",
    "answer": "Głównym bohaterem jest Bezimienny (ang. Nameless Hero)."
  },
  ...
]
```

- **`category`** – tekst przycisku w tabeli.  
- **`question`** – treść pytania wyświetlana w modal-u.  
- **`answer`** – treść odpowiedzi wyświetlana po pytaniu.

Aby tabela była wypełniona, powinno być co najmniej 36 obiektów (6×6). W przeciwnym wypadku puste komórki wyświetlą przycisk `???` i pozostaną nieaktywne.

---

## Plik `avatars.json` (przykład struktury)

```json
[
  "images/bezi.png",
  "images/diego.png",
  "images/xardas.png"
]
```

- Zawiera ścieżki do plików graficznych, używanych w avatar-pickerze.  
- Możesz dodać nowe pliki PNG do folderu `images/` i rozszerzyć tę tablicę.

---

## Plik `style.css`

Cały główny styl strony (kolorystyka, układ, wygląd przycisków, tabela, avatary, lampki itp.) znajduje się w `css/style.css`. Modal pobiera tylko niewielką część styli bezpośrednio w `<head>`, ale możesz je przenieść do tego pliku.

---

## Podsumowanie

- **Gracze**: dodawanie, usuwanie, edycja imienia, zmiana avatara, liczniki punktów, lampki „Tura”/„Odpowiada”.  
- **Pytania**: siatka 6×6 przycisków, własny modal _„Pytanie → Odpowiedź → Avatar”_, dwuklik na avatar ponownie otwiera modal.  
- **Avatar-picker**: wspólny zarówno w sekcji graczy, jak i po wstawieniu avatara w pytaniu, korzystający z listy `avatars.json`.  
- **Lokalne przechowywanie**: stan graczy i ich avatarów zapisywany jest w `localStorage`.

Całość działa w czystym HTML/CSS/JS bez konieczności serwera backendowego. Po prostu wrzuć wszystkie pliki do katalogu projektu i otwórz `index.html` w przeglądarce.
