# CRM — e-mailové přílohy a značková šablona

- Datum: 2026-10-02
- Autor / chat: Codex
- Stav: commitnuto lokálně, čeká na push, nasazení a produkční test
- Zadání: Přiložit fotografie a PDF faktury, zlepšit vzhled e-mailů a skrýt adresu kopie.

## Výsledek

- Mailjet adaptér používá Bcc místo viditelného Cc a podporuje Base64 přílohy. PDF dostává přednost a fotografie se přikládají do bezpečného rozpočtu 14 MB; zbytek se nezahodí, ale odešle jako sedmidenní zabezpečené odkazy.
- Potvrzení poptávky přikládá fotografie nahrané formulářem.
- Report zakázky načítá vybrané fotografie z R2 a PDF faktury z Fakturoidu; odkazy zůstávají jako záloha.
- Oba e-maily používají společnou responzivní HTML šablonu Veroa s kontaktní patičkou.
- Rozhraní označuje kopii jako skrytou a přesněji popisuje volby příloh.
- Mailjet odpověď bez Message ID nebo se stavem chyby už není evidována jako úspěšná.
- Selhání odeslání je viditelné červeně na přehledu CRM i detailu záznamu a zapisuje se do serverového logu. Neúspěšný report zakázky lze výslovně odeslat znovu; po úspěchu se stará chyba označí jako vyřešená.

## Rozhodnutí a ověření

- R-007 potvrzuje přílohy, profesionální šablonu a skrytou kopii.
- Backend: 6 testovacích souborů / 20 testů prošlo.
- Backendový TypeScript/tsoa build a frontendový Vue/Vite build prošly.
- Oficiální dokumentace ověřila Mailjet přílohy do 15 MB a Fakturoid endpoint `/invoices/{id}/download.pdf`, který může dočasně vracet 204; implementace krátce opakuje stažení.

## Otevřené body

- Změny jsou v lokálním commitu `2bdb812` (`Improve email delivery and attachment handling`), ale zatím nejsou pushnuté ani nasazené.
- Produkčně je třeba otestovat reálnou fotografii, PDF faktury, Bcc a zobrazení v Gmailu.
- Retenční doba fotografií a faktur zůstává otevřená. Nic se automaticky nemaže.

## Přesný další krok

Po schválení pushnout commit `2bdb812` a nasadit backend i frontend. Potom založit novou testovací poptávku s fotografií a dokončit novou testovací zakázku s fakturou.
