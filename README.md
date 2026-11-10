# E-Commerce Sales & Cancellation Analysis (Power BI)

Dashboard stworzony w Power BI, służący do analizy wyników sprzedażowych oraz wskaźnika anulacji zamówień w e-commerce. Projekt skupia się na transformacji i czyszczeniu danych transakcyjnych pochodzących z systemu ERP, poprawnym modelowaniu relacji oraz budowie miar w języku DAX.

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

## Utworzone miary DAX:

**Liczba Zamówień:**
```dax
Liczba Zamówień = 
COUNTROWS(Fakty_Zamowienia)
```
**Łączna Wartość_Brutto:**
```dax
Łączna Wartość_Brutto = 
SUM(Fakty_Zamowienia[Wartosc_Brutto])
```
**Średnia Wartość_Brutto Koszyka:**
```dax
Średnia Wartość_Brutto Koszyka = 
AVERAGE(Fakty_Zamowienia[Wartosc_Brutto])
```
**Top Produkty w Kategoriach:**
```dax
Top Produkty w Kategoriach = 
VAR TopProdukt = CONCATENATEX(TOPN(1, ALL(Wymiar_Produkty[Nazwa_Produktu]), [Wartość_Brutto Zamówienia Zrealizowane], DESC), Wymiar_Produkty[Nazwa_Produktu], ", ")
VAR Wartosc = [Wartość_Brutto Zamówienia Zrealizowane]
RETURN
IF(ISBLANK(Wartosc) || Wartosc = 0, BLANK(), TopProdukt)
```
**Wartość_Brutto Zamówienia Zrealizowane:**
```dax
Wartość_Brutto Zamówienia Zrealizowane = 
VAR WartoscZrealizowana = CALCULATE([Łączna Wartość_Brutto], Fakty_Zamowienia[Status] = "Zrealizowane")
RETURN
IF(ISBLANK(WartoscZrealizowana), 0, WartoscZrealizowana)
```
**Wskaźnik Anulacji:**
```dax
Wskaźnik Anulacji = 
VAR LiczbaZamowienAnulowanych = CALCULATE([Liczba Zamówień], Fakty_Zamowienia[Status] = "Anulowane")
VAR WszystkieZamowienia = CALCULATE([Liczba Zamówień], REMOVEFILTERS(Fakty_Zamowienia[Status]))
RETURN
DIVIDE(LiczbaZamowienAnulowanych, WszystkieZamowienia, 0)
```
**Kalendarz:**
```dax
Kalendarz = 
VAR MinData = MIN(Fakty_Zamowienia[Data_Zamowienia])
VAR MaxData = MAX(Fakty_Zamowienia[Data_Zamowienia])
VAR MinRok = YEAR(MinData)
VAR MaxRok = YEAR(MaxData)
RETURN
CALENDAR(DATE(MinRok, 1, 1), DATE(MaxRok, 12, 31))
```
**Dzień Miesiąca:**
```dax
Dzień Miesiąca = 
DAY(Kalendarz[Date])
```
**Miesiąc Nazwa:**
```dax
Miesiąc Nazwa = 
FORMAT(Kalendarz[Date], "mmmm")
```
**Miesiąc Nr:**
```dax
Miesiąc Nr = 
MONTH(Kalendarz[Date])
```

## Zawartość Repozytorium
- Pełny plik raportu Power BI gotowy do pobrania i sprawdzenia.

### Zrzuty ekranu z projektu:

#### Podgląd raportu
<img width="1307" height="742" alt="zrzut_ekranu_podglad_raportu" src="https://github.com/user-attachments/assets/44e8f235-7667-4273-92ad-9e7884fc3091" />

#### Model danych (relacje)
<img width="1309" height="801" alt="zrzut_ekranu_relacje" src="https://github.com/user-attachments/assets/1511f5f9-a5b1-443e-8ca9-1ac29dea8011" />



