# E-Commerce Sales & Cancellation Analysis (Power BI)

## Dashboard stworzony w Power BI, służący do analizy wyników sprzedażowych oraz wskaźnika anulacji zamówień w e-commerce. Projekt skupia się na transformacji i czyszczeniu danych transakcyjnych pochodzących z systemu ERP, poprawnym modelowaniu relacji oraz budowie miar w języku DAX.

## Zakres prac i Architektura Modelu
- **ETL & Czyszczenie danych (Power Query):**
  - Standaryzacja statusów zamówień i czyszczenie błędów wprowadzania ręcznego.
  - Obsługa wartości ujemnych i korekt (sprowadzenie ilości i wartości brutto do wartości bezwzględnych).
  - Usunięcie zbędnych kolumn.

- **Model Danych (Star Schema):**
  - Projekt oparty na Schemacie Gwiazdy łączącym centralną tabelę faktów z wymiarami produktów, klientów oraz dedykowaną wymiarową tabelą kalendarza.
  - Zastosowanie relacji wiele-do-1.

- **Logika DAX & Wskaźniki KPI:**
  - Utworzenie dedykowanej tabeli `_Miary` dla zachowania porządku w modelu.
  - Wskaźnik anulacji zamówień wykorzystujący `REMOVEFILTERS`, odporny na kontekst filtrów statusu.
  - Dynamiczne wyznaczanie lidera sprzedaży w kategoriach zabezpieczone przed remisami i pustymi wartościami (`TOPN`, `CONCATENATEX`, `VAR/RETURN`).

## Kluczowe Wnioski
- **Przychód Zrealizowany:** Wyniósł 2,97 tys. zł w analizowanym okresie.
- **Wskaźnik Anulacji:** Ogólna średnia dla sklepu to 20,00%.
- **Wykryte Ryzyko:** W kategorii *Opakowania* wskaźnik anulacji wynosi aż 26,67%, co wskazuje na potencjalne problemy w procesie pakowania lub dostępności tego asortymentu i wymaga dalszej weryfikacji.

## Zawartość Repozytorium
- Pełny plik raportu Power BI gotowy do pobrania i sprawdzenia.
