# Git záloha společné paměti

- Datum: 2026-10-03
- Autor / chat: Codex a majitel
- Stav: lokálně dokončeno, push čeká na SSH přístup.
- Zadání: Založit kořenový Git repozitář pro společnou paměť Veroa a vyloučit samostatné repozitáře webu a CRM.

## Výsledek

Kořen Undertaker byl připraven pro repozitář `git@github-personal:OndraFi/mind-of-Veroa.git`. `.gitignore` vylučuje `Gravia/`, `GRAVIA-CRM/`, známé lokální přístupové soubory a běžná tajemství či dočasné soubory. Do verzování patří dokumentace, původní PDF, grafika a prázdná složka `Náklady/` držená souborem `.gitkeep`.

## Rozhodnutí a ověření

Rozsah verzování potvrdil majitel 2026-10-03; zapsáno jako R-009. Inventura souborů proběhla bez načtení citlivých hodnot. Přístup k GitHubu přes alias `github-personal` byl skutečně vyzkoušen, ale skončil `Permission denied (publickey)`.

## Otevřené body

Vzdálený repozitář zatím nelze ověřit ani naplnit, dokud nebude na tomto počítači dostupný správný SSH klíč pro účet s přístupem k `OndraFi/mind-of-Veroa`.

## Přesný další krok

Zprovoznit nebo načíst GitHub SSH klíč pro alias `github-personal`, ověřit `git ls-remote` a pushnout lokální větev `main` do `origin`.
