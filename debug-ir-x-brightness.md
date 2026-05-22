[OPEN]

# Temat
Przycisk IR `X` (wartość podawana: `0x20230CF`) nie uruchamia regulacji jasności — brak reakcji na ekranie i brak widocznych efektów.

# Hipotezy (falsyfikowalne)
1. ESP32 w ogóle nie dekoduje tego przycisku w normalnym trybie (brak ramek / filtr `UNKNOWN` / brak `resume()`), więc kod nigdy nie trafia do logiki sterowania.
2. Ramki są dekodowane, ale `repeat=true` zawsze, a kod jest obsłużony tylko dla `repeat=false`, więc naciśnięcie jest ignorowane.
3. Dekodowana wartość różni się od `0x20230CF` (np. `0x020230CF`, inny protokół, inne `bits`), więc porównanie nie trafia.
4. Regulacja jasności jest wywoływana, ale na LCD800 `setBrightness()`/`dspOn` nie zmienia realnego podświetlenia (brak efektu), więc wygląda jak „nie działa”.
5. Tryb nagrywania IR / mapowanie przechwytuje zdarzenie wcześniej i wycina dalszą obsługę.

# Plan dowodowy
- Doinstrumentować odbiór IR: logować `protocol`, `value`, `bits`, `repeat` dla każdej ramki i wskazać, gdzie jest odrzucana.
- Potwierdzić, czy `setBrightness()` faktycznie zmienia podświetlenie na LCD800 (log wartości).

# Status
- [ ] Instrumentacja dodana
- [ ] Zebrane logi z naciśnięcia `X`
- [ ] Analiza i decyzja o poprawce
- [ ] Weryfikacja po poprawce

