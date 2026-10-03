# CRM — fakturace a potvrzovací e-mail

Datum: 2026-10-01

## Výsledek

- Veřejný CRM formulář po nasazení opravy routeru funguje a test vytvořil poptávku, kontakt i fotografii.
- Produkční `emaillogs` eviduje potvrzení testovací poptávky jako `SENT` s Mailjet Message ID. Backend tedy zprávu Mailjetu předal; doručení do schránky ještě není prokázané.
- Faktura testovací zakázky je v `invoicereferences` jako `FAILED_REVIEW_REQUIRED` bez externího ID a URL. Tento bezpečnostní záznam blokuje opakování, aby nevznikla duplicitní faktura.
- Slug otevřeného účtu Fakturoid je podle URL `ondrejfialka`.
- Commit `884b114` je nasazený. Nový pokus jednoznačně odhalil `Fakturoid OAuth error 401`; názvy proměnných na Renderu jsou správné, ale Fakturoid odmítá Client ID / Client Secret.
- Lokální commit `aefc40a` přesouvá OAuth ověření před vytvoření ochranného záznamu a vrací čitelnou chybu. Backend build a 6 souborů / 17 testů prošly. Produkční push zatím nebyl schválen.

## Otevřené body

- Ve Fakturoidu projít `Nastavení → Uživatelský účet → API přístupy`, zadat 2FA kód a zkopírovat aktuální Client ID a Client Secret do Renderu. Nepoužít OAuth integraci z `Napojení na jiné aplikace`.
- V Mailjet Activity dohledat testovací Message ID a zjistit `Delivered`, `Blocked` nebo `Bounced`; při `Delivered` zkontrolovat Spam, Hromadné a Všechny zprávy v Gmailu.

## Další krok

Aktualizovat Client ID a Secret na Renderu, nasadit `aefc40a`, odstranit pouze záznamy vzniklé při prokázaném OAuth 401 a vytvoření faktury zopakovat. Pokud další chyba nastane až po OAuth, vyhodnotit nový konkrétní log.
