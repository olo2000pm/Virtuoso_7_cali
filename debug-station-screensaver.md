[OPEN]

# Temat
Wybrane streamy (np. `https://ch03.cdn.eurozet.pl/CHIWJZ.mp3`, `https://ch02.cdn.eurozet.pl/CHIJAZ.mp3`) powodują przejście UI w wygaszacz.

# Oczekiwane
Podczas grania stacji UI nie powinno samoczynnie przechodzić w wygaszacz / blank.

# Hipotezy (falsyfikowalne)
1. Player dla tych URL-i przerywa odtwarzanie (status STOPPED/buffer underrun), więc logika wygaszacza uznaje „nie gra” i wchodzi w wygaszacz.
2. Dla tych URL-i występuje długi brak danych/metadata, co zatrzymuje `player.isRunning()` na tyle długo, że wygaszacz wchodzi mimo że audio wraca.
3. Występuje błąd TLS/HTTP/redirect dla tych URL-i, co powoduje chwilowe rozłączenia i przejście w SCREENSAVER.
4. UI przechodzi w wygaszacz z powodu timerów `screensaverTicks`/`screensaverPlayingTicks` (błędne warunki resetowania przy tych stacjach).
5. Jest przypadkowe wywołanie `display.putRequest(NEWMODE, SCREENSAVER/SCREENBLANK)` po błędzie streamu.

# Plan dowodowy
- Doinstrumentować: (a) wejście/wyjście z `SCREENSAVER/SCREENBLANK` wraz z powodami/timerami, (b) zmiany `player.isRunning()` i `player.status()`.
- Zebrać logi z przełączenia na wadliwą stację i momentu wejścia w wygaszacz.

# Status
- [ ] Instrumentacja dodana
- [ ] Logi z reprodukcji
- [ ] Analiza
- [ ] Poprawka
- [ ] Weryfikacja

