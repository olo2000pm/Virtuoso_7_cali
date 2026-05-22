[OPEN]

# Sesja debug: badlink-black-screen

## Objawy
- Po wybraniu „złej” stacji (np. `http://stream.polskieradio.pl/polskieradio`) ekran robi się czarny.
- Po chwili radio przełącza na stację #1, ale nie zaczyna grać.
- Brakuje komunikatu o błędnym linku na ekranie.
- Zdarza się reset `esp=4 | PANIC`.

## Oczekiwane
- Pokazać komunikat o błędnym linku.
- Przejść do stacji #1 i rozpocząć odtwarzanie.
- Brak czarnego ekranu i brak PANIC.

## Hipotezy
1. `audio_error()` wywoływane wielokrotnie i szybko, a obecna reakcja (w tym `player.sendCommand`) powoduje reentrancję/wyścig w Player/Audio i w konsekwencji PANIC albo brak startu #1.
2. Komunikat o błędnym linku jest ustawiany, ale natychmiast nadpisywany przez `_play(1)` (`const_PlConnect`), więc na ekranie nie ma szans go zobaczyć.
3. Polecenie `PR_PLAY,1` jest wysyłane, ale gubi się w kolejce albo jest anulowane przez `_stop(true)` wykonywany zaraz po wykryciu błędu.
4. Czarny ekran to nie brak UI, tylko tryb `SCREENBLANK/SCREENSAVER` ustawiany przy błędzie/stopie i brak powrotu do `PLAYER` w odpowiedniej kolejności.
5. PANIC wynika z nieobsłużonego przypadku w Audio (np. pętla redirect/parsowania nagłówków) przy odpowiedzi HTML/404.

## Instrumentacja (do zebrania logów)
- `DBG badlink` w `audio_error()` i w momencie decyzji o auto-przejściu na stację #1
- `DBG play` przy obsłudze `PR_PLAY` i wynik połączenia
- `DBG stop` przy `_stop(true)` po błędzie

## Kroki reprodukcji
1. Uruchom radio, wejdź w PLAYER.
2. Włącz stację: `http://stream.polskieradio.pl/polskieradio`
3. Zapisz logi z UART/Telnet od momentu kliknięcia do momentu przełączenia na #1.

