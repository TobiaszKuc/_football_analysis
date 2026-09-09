# Zadanie 1
1.⁠ ⁠Oblicz różnicę xG (expected goals) dla każdego stanu meczu (wygrana-przegrana-remis) dla obu drużyn.

Nie byłem pewny, jakie dokładnie liczby są ode mnie oczekiwane, dlatego zwróciłem się z prośbą o doprecyzowanie, czy chodzi o różnicę między drużynami, czy między stanami meczu wewnątrz jednej drużyny. Odpowiedź wskazywała, żebym wybrał wariant, który lepiej odpowiada na pytanie "co to mówi sztabowi trenerskiemu" i uzasadnić wybór jednym zdaniem.
### Wybrany został wariant z różnicą xG między drużynami dla każdego stanu meczu

## Wybór metryki xG zamiast Execution xG
Wybór został podyktowany tym, że liczyło się dla mnie stwarzane zagrożenie, a nie jakość samych strzałów (a Execution xG bierze pod uwagę również kierunek i siłę strzału), dlatgo zdecydowałem się na klasyczny model xG.

## Metodologia
1. Filtrowanie po `mecz["event_type_name"] == "Shot"`
2. Odkrycie problemu: każdy strzał zawiera dane `freeze_frame`, przez co powielony jest około 18 razy, bo każdy wiersz to lokalizacja jednego zawodnika w momencie strzału.
3. Dodatkowy filtr `mecz["freeze_frame_player_id"].isna == True` zostawia tylko wiersz z danymi strzału, eliminując pozostałe 17.
4. Weryfikacja: `mecz["id"].nunique()` jest równe `mecz["id"].count()` oraz ręczne sprawdzenie timestampów pozwala stwierdzić, że błąd był naprawiony.
5. Budowa kolumny `gole`: `.apply()`, 1, kiedy padł gol dla Pogoni, -1 kiedy padł gol dla Polonii.
6. Metoda `.cumsum()` pozwala na zrobienie kolumny śledzącej wynik `stan_meczu`
7. Budowa kolumny `korzysc`: `.apply()`, 1 dla `stan_meczu` > 0, -1 dla `stan_meczu` < 0, 0 dla `stan_meczu` = 0.
8. Agregacja `.groupby(["korzysc", "possession_team_name"]).agg({"statsbomb_xg":"sum", "event_type_name":"count"})`
9. Zastosowanie `.pivot()` dla zrobienia tabeli przestawnej, pozwalającej zliczyć różnicę xG

## Napotkane anomalie w danych
Przed filtrowaniem `mecz["freeze_frame_player_id"].isna == True` było 628 eventów o `event_type_name = "Shot"` oraz 79 eventów z `outcome_name = "Goal"`, dużo za dużo jak na jeden mecz. 
Po filtrowaniu były już tylko 34 strzały i 4 gole zgadzające się timestampami z prawdziwymi wydarzeniami z meczu
Dodatkowo werfikowałem po `match_id`, czy to na pewno jeden mecz.

## Wyniki i jego ograniczenia
![Różnica xG w zależności od stanu meczu](images/roznica_xg.png.png)