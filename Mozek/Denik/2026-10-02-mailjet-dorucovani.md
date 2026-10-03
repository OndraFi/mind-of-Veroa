# Mailjet — diagnostika nedoručovaných e-mailů

- Datum: 2026-10-02
- Autor / chat: Codex, produkční diagnostika s majitelem
- Stav: čeká na úpravu DNS
- Zadání: Zjistit, proč potvrzení formuláře ani e-mail dokončené zakázky nepřicházejí.

## Výsledek

- Majitel potvrdil, že fakturace už funguje.
- CRM eviduje několik potvrzení formuláře a e-mail dokončené zakázky jako `SENT` se skutečnými Mailjet Message ID. Render při odeslání neukázal chybu.
- Produkční Mailjet API klíč patří podúčtu `Veroa`; nulové statistiky hlavního účtu proto nebyly relevantní.
- `info@veroa.cz` a doména `veroa.cz` jsou v Mailjetu aktivní, ale SPF i DKIM mají stav `Error`.
- Veřejné DNS obsahuje SPF jen pro Cloudflare a neobsahuje `mailjet._domainkey.veroa.cz`.

## Rozhodnutí a ověření

- Ověřeno v produkčním CRM, MongoDB Atlas, Renderu, Mailjetu a veřejném DNS 2026-10-02.
- Potvrzeno majitelem 2026-10-02: faktury fungují; e-maily do schránky nepřicházejí.
- Pravděpodobná příčina nedoručování je chybějící autentizace Mailjetu pro doménu. Stav konkrétní zprávy `Delivered` / `Blocked` / `Bounced` zatím v podúčtu `Veroa` ověřen nebyl.

## Otevřené body

- V DNS je potřeba upravit jediný SPF TXT na kombinovanou hodnotu z Mailjetu a přidat jeho DKIM TXT.
- Po propagaci musí Mailjet zobrazit SPF i DKIM `OK`.
- Potom poslat novou poptávku i e-mail dokončené zakázky a ověřit jejich stav v Activity podúčtu `Veroa` a přijetí do Gmailu.

## Přesný další krok

V DNS správě `veroa.cz` upravit SPF na `v=spf1 include:spf.mailjet.com include:_spf.mx.cloudflare.net ~all`, přidat TXT host `mailjet._domainkey` s DKIM hodnotou z Mailjet průvodce a po propagaci kliknout v Mailjetu na `Refresh`.
