[OPEN]

# Temat
Niektóre streamy HTTPS nie odtwarzają się (np. eurozet/smcdn), podczas gdy inne HTTPS działają (slotex).

# Przykłady
- Działa: `https://s1.slotex.pl:7022/;`
- Nie działa:
  - `https://an04.cdn.eurozet.pl/ANTBAL.mp3`
  - `https://ch02.cdn.eurozet.pl/CHIJAZ.mp3`
  - `https://ic1.smcdn.pl/2070-1.mp3`

# Hipotezy (falsyfikowalne)
1. Problem z TLS (SNI/certyfikaty/cipher suite/ALPN) dla konkretnych hostów powoduje brak połączenia.
2. Serwer wymaga nagłówków HTTP (User-Agent/Accept/Icy-MetaData) lub blokuje embedded klienta (403/451) – połączenie jest, ale audio nie startuje.
3. Serwer wysyła redirect (301/302) na inny host/protokół i klient go nie obsługuje.
4. Serwer używa chunked/HTTP/2 lub nietypowego Content-Type i parser w Audio/HTTP nie przechodzi do strumieniowania.
5. Połączenie startuje, ale po chwili jest rozłączane przez timeout/keep-alive, co kończy się autostopem.

# Plan dowodowy
- Doinstrumentować etap `connecttohost()`/HTTP handshake: protokół (http/https), host, port, status odpowiedzi HTTP, redirect, oraz info o błędzie TLS/połączenia.
- Zebrać logi dla 1 działającego i 1 niedziałającego URL.

# Status
- [ ] Instrumentacja dodana
- [ ] Logi z reprodukcji
- [ ] Analiza
- [ ] Poprawka
- [ ] Weryfikacja

