# Zadanie 2 - analiza potencjalnego wahadłowego na podstawie danych StatsBomb 

Odpowiedź opisuje sposób podejścia do posłużenia się danymi Statsbomb w celu oceny zawodnika X, podkreślając jednak ograniczenia związane z analizą na podstawie samych danych eventowych. Dodatkowo chcę zaznaczyć, że jest to praca w całości koncepcyjna, opierająca się na mojej wiedzy dotyczącej zawartości danych StatsBomb.

## Definicja roli wahadłowego

Na podstawie obserwacji z meczu Pogoni Grodzisk Mazowiecki z Wartą Poznań, wyróżniłem kluczowe cechy wahadłowych w systemie gospodarzy. Warto dodać, że listę należałoby uzupełnić o wymagania uzgodnione z trenerem.  
Faza ofensywna:
- dynamika i duża ilość prób dryblingu po obu stronach
- bliskie podania z końcowej linii boiska do środka pola karnego, w tym tzw. *cut-back passes*, oraz podania zakończone strzałem, nazywane później ***kluczowymi podaniami*** (z ang. "Key Passes")

Faza obronna:
- najszerzej ustawieni w linii pięciu (wraz z trzema obrońcami)

Faza przejściowa:
- podania progresywne podczas kontrataków

## Przygotowania do analizy

### Analiza pozycji zawodnika
Na podstawie eventów "Starting XI" sprawdzę w jakim ustawieniu grała drużyna zawodnika X i do jakiej pozycji był on przypisany. Wyfiltruję też eventy *Tactical Shift*, aby dowiedzieć się, czy zmieniał pozycję w trakcie meczu. Pozwoli mi to ustalić priorytetowe dane w analizie, według hierarchii:
1. Wahadłowy
2. Skrzydłowy/Boczny obrońca
3. Reszta pozycji

### Filtrowanie dla konkretnych eventów
To dotyczy filtrowania przy każdym evencie pozycji zawodnika.
W przypadku etykiety skrzydłowego, skupiłbym się na jego akcjach defensywnych (*defensive contributions*), natomiast dla bocznego obrońcy ważniejsze byłyby miary ofensywne (jak wyżej wspomniane podania z linii końcowej). W skrajnym przypadku (np. zawodnik grał przez 90% czasu na pozycji defensywnego pomocnika), stworzę metryki na podstawie eventów z pozycjami wahadłowy/boczny obrońca/skyrzdłowy, zaznaczając przy małej próbie, że nie jest ona bardzo istotna statystycznie. Przy całkowitym braku takich eventów, ocena zawodnika pod kątem wahadłowego jest niemożliwa za pomocą samych danych.

## Metryki
Metryki powstały na podstawie ustaleń związanych z zestawem cech z sekcji pierwszej. 
1. **Dribbles per 90** + success rate - liczy się ilość podejmowanych prób oraz skuteczność, później wyrażona dodatkowo przez OBV.
2. **Through balls per 90** + success rate - do tego mapa pokazująca skąd dokąd padały.
3. Podania rozpoczęte w `location_x` = [110,120] i zakończone w polu karnym czyli `location_x` = [102,120] i `location_y` = [18,62] - chodzi o **podania z końca boiska do pola karnego**, w tym *cut-back passes*.
4. **Defensive Contributions per 90** - "Clearance", "Block", "Interception" wraz z mapą pokazującą lokalizacje tych zdarzeń. Szczególnie istotne w przypadku gracza ofensywnego, aby zobaczyć jego udział w grze obronnej.
5. **On Ball Value** (kolumna *obv_for_net* zagregowana z podziałem na kategorie) - rozbite osobno na **dryblingi, through balls, podania z końca boiska do pola karnego oraz kluczowe podania**. Ten model określa zwiększenie prawdopodobieństwa zdobycia bramki bądź zmniejszenie ryzyka jej stracenia, co lepiej określi efektywność.
6. **Podania progresywne** - różne definicje (Opta nie definiuje ich w ogóle, a FBref wprowadza ich konkretne ramy geometryczne) sprawiają, że zdecydowałem się nie zawierać tej metryki bez konsultacji z trenerem bądź głównym analitykiem zespołu.
7. Możliwość odniesienia tych statystyk do **wahadłowych Pogoni Grodzisk Mazowiecki** poszerzy kontekst, jeśli takie dane będzie można uzyskać.


## Ograniczenia i wnioski
### Ograniczenia
Dane eventowe wychwytują zachowania umykające podczas zwykłej obserwacji, natomiast w tym przypadku napotykają kilka ograniczeń:
1. Nie da się odróżnić **systemu od indywidualnej preferencji zawodnika** - nawet w przypadku samej Pogoni: czy schodzenie do środka Zbróga wynika z jego prawonożności, czy chodzi o asymetrię ról wahadłowych
2. Dane zdarzeniowe rejestrują tylko **momenty z piłką przy nodze**, natomiast duża część pracy wahadłowego (udział w pressingu, cofanie się do bloku obronnego) obejmuje pracę bez piłki. Freeze_frames pokazują lokalizacje zawodników tylko dla poszczególnych eventów, a nie ciągły ruch.
3. **Brak kontekstu rywala** oraz **założeń taktycznych drużyny zawodnika X** - nie wiadomo, która drużyna dyktowała grę, np. czy duża ilość dryblingów wynikała z tego, że 20 meczów było przeciwko drużynom grającym niskim blokiem, czy z wykorzystania rozproszenia obrony podczas kontrataku.
4. 20 meczów to niewiele ponad połowa sezonu w 1. lidze. Dla zdarzeń rzadkich, takich jak *through balls*, może się to przełożyć na duże zaszumienie danych wynikające z **małej próby**.

### Wnioski
Na podstawie tych danych i metryk można zdecydować o wstępnym odrzuceniu zawodnika (np. w sytuacji, gdzie wszystkie metryki wypadają znacznie poniżej danych zawodników Pogoni). Jednak bardziej prawdopodobne jest, że metryki te mają na celu skierować uwagę na konkretne aspekty przy dalszej analizie wideo, jak na przykład przy niskiej liczbie dryblingów można się przyjrzeć, czy wynika ze słabej techniki zawodnika, czy z założeń taktycznych. 