# Opis pracy inżynierskiej

## Temat pracy w języku polskim

**Wieloplatformowy system wspomagania treningu siłowego z modułem zdalnej opieki trenerskiej i geolokalizacyjnego doboru partnerów treningowych**

## Temat pracy w języku angielskim

**A multiplatform system for supporting and monitoring strength training with progress analytics, remote coaching, and geolocation-based workout partner matching**

## Zakres pracy

Celem pracy jest opracowanie rozproszonego, wieloplatformowego systemu informatycznego działającego w architekturze **Offline-First**. System ma kompleksowo wspierać proces treningu siłowego, usprawniać zdalną opiekę trenerską oraz ułatwiać bezpośrednie interakcje między użytkownikami obiektów sportowych, wspierając tym samym przeciwdziałanie izolacji społecznej.

System będzie składał się z serwera, aplikacji mobilnej przeznaczonej dla osób ćwiczących oraz panelu webowego dla kadry trenerskiej. Zakres funkcjonalny obejmuje następujące moduły:

### Rejestracja i obsługa jednostek treningowych

Aplikacja mobilna umożliwi zapisywanie ćwiczeń, serii, obciążeń, liczby powtórzeń oraz wskaźników intensywności. Dzięki architekturze Offline-First rejestrowanie treningu będzie dostępne także bez połączenia z siecią, a dane zostaną zsynchronizowane po odzyskaniu łączności. Moduł obejmie również trwałe notatki techniczne, presety konfiguracji maszyn oraz asynchroniczną wymianę informacji zwrotnej między ćwiczącym a trenerem.

### Analityka postępów i analiza trendów

Komponent analityczny będzie wspierał ocenę postępów treningowych i zmian sylwetkowych. Jego zadaniem będzie ograniczanie wpływu szumu pomiarowego w danych o masie ciała, monitorowanie zmian względem zadeklarowanego celu, szacowanie obciążeń maksymalnych oraz analiza rozkładu objętości i tonażu pomiędzy grupami mięśniowymi w celu wykrywania potencjalnych dysproporcji.

### Panel zarządzania procesem trenerskim

Panel webowy dla trenerów umożliwi bezpieczne parowanie profili trenera i podopiecznego, projektowanie cykli treningowych oraz ich automatyczną synchronizację z aplikacją mobilną. Trenerzy będą mogli zdalnie przekazywać gotowe plany, przeglądać rejestrowane treningi i postępy podopiecznych, a także generować okresowe raporty podsumowujące.

### Geofencing i przetwarzanie danych przestrzennych

Moduł geolokalizacyjny będzie wykorzystywał mechanizmy geofencingu do pasywnego wykrywania obecności użytkownika w klubach fitness. Na tej podstawie system będzie mógł automatycznie inicjować sesję treningową i aktualizować w czasie rzeczywistym stan aktywności użytkownika na terenie siłowni.

### Algorytmiczny dobór partnerów treningowych

Silnik scoringowy będzie rekomendował użytkowników, którzy mogą odbyć wspólną jednostkę treningową. Rekomendacje zostaną wyznaczone na podstawie wektorów cech opisujących użytkowników.

### Moduł ofert trenerskich

Dwukierunkowy moduł ogłoszeniowy umożliwi filtrowanie bazy instruktorów, publikowanie przez ćwiczących zgłoszeń dotyczących poszukiwanego wsparcia oraz bezpośrednie składanie ofert współpracy przez trenerów.
