[OPEN]

# Sesja debug: webui-freeze-panic

## Objaw
- Po wejściu na stronę WWW radia (np. `http://192.168.33.25/`) radio potrafi się zawiesić lub zresetować (`reset: esp=4 | PANIC`).
- Jednocześnie przeglądarka/strona też się potrafi „zawiesić” (brak odpowiedzi UI, brak ładowania).

## Oczekiwane
- Strona WWW ładuje się i działa płynnie, bez wpływu na stabilność radia.

## Kroki odtworzenia (do uzupełnienia)
- Uruchom radio → poczekaj aż poda IP → otwórz stronę WWW → obserwuj zachowanie i logi.

## Hipotezy (falsyfikowalne)
1. Przeglądarka odpala pętlę żądań (HTTP/WebSocket) powodując zalew zdarzeń, wzrost użycia RAM i reset/panic.
2. WebSocket wysyła zbyt duże/za częste payloady (np. status/konfig), co blokuje task sieciowy lub powoduje alokacje prowadzące do crasha.
3. Obsługa HTTP plików (SPIFFS) przy wielu równoległych requestach blokuje inne zadania (watchdog / deadlock).
4. Błąd w JS na stronie powoduje nieskończoną pętlę po stronie przeglądarki, a równolegle radio jest przeciążane retry/reconnect.
5. Konkretne endpointy (np. `/ws`, `/config`, `/status`) zwracają niepoprawne dane i powodują wyjątek/panic w firmware.

## Plan dowodów
- Zbierz: pełny log z momentu wejścia na WWW aż do resetu (z backtrace jeśli jest).
- Zbierz: lista requestów z przeglądarki (Network: ilość, częstotliwość, błędy, WS frames).
- Dodaj instrumentację (bez zmiany logiki): liczniki requestów, rozmiary payloadów, free heap/psram, czas obsługi.

