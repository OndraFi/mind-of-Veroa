# Mapa zdrojů

Inventura: 2026-09-30. Originály zůstaly na místě. U PDF byly pro založení mapy načteny úvodní stránky a počet stran; neproběhla úplná odborná kontrola všech tvrzení.

| Zdroj v `../Materiály/` | K čemu slouží | Jak s ním zacházet |
| --- | --- | --- |
| `veroa-marketingovy-pruzkum.md` | Průzkum od Claude z 2026-09-29: persony, konkurence, zahraniční příklady a návrhy kanálů | Dodán majitelem 2026-09-30, uložen beze změny z Downloads; přečten celý, externí zdroje neověřeny. Hypotézy a návrhy nejsou rozhodnutí majitele |
| `Business_plan_pece_o_hroby_2026.pdf` (10 stran) | Strategie a fáze růstu | Pracovní plán ze září 2026; ne doklad dosaženého stavu |
| `Upsell_a_cenik_pece_o_hroby_2026.pdf` (9 stran) | Portfolio a návrhy cen | Výslovně pracovní návrh k ověření na reálných nákladech |
| `Manual_cisteni_hrobu_materialy_a_techniky.pdf` (17 stran) | Materiály a pracovní techniky | Pracovní příručka; před konkrétním postupem přečíst relevantní část a ověřit vhodnost |
| `Údržba hrobů průzkum trhu.pdf` (13 stran) | Kontext trhu a původní výzkum | Tvrzení o trhu, právu a technikách nejsou v této inventuře ověřena |
| `VEROA-souhrn.md` | Značka, grafika, dřívější ceny | Část cen nesouhlasí s lokálním webem; některé grafické soubory chybí |
| `souhrn-uprav-webu-veroa.md` | Předání webu a mapa souborů | Historický snímek, technické detaily ověřovat v kódu |
| `SOUHRN_PROJEKTU_VEROA_CRM.md` | Předání CRM | Preferovaný název souhrnu pro orientaci |
| `SOUHRN_PROJEKTU.md` | Druhá kopie CRM souhrnu | Shodný text při porovnání normalizovaných řádků; ponecháno jako originál |

Při podrobném rozboru PDF zapisuj název, stranu/kapitolu, datum a míru ověření přímo do příslušného tématu. Starý průzkum nepoužívej jako aktuální cenovou nabídku konkurence nebo platné právní stanovisko.

## Další zdroje

- `../Gravia/config/` a `../Gravia/data/`: aktuální lokální nastavení nabídky a značky.
- `../Gravia/designs/logo/`: tři nalezené varianty loga/avatara.
- `../designs/veroa-letak-a5-predloha.png`: grafický podklad letáku.
- `../Náklady/`: při inventuře prázdná složka.
- README a deployment dokumenty aplikací: technická orientace, nikoli doklad aktuálně běžící produkce.

## Technické východisko instrukcí

Způsob načítání AGENTS.md ověřen v [oficiální dokumentaci OpenAI](https://learn.chatgpt.com/docs/agent-configuration/agents-md) dne 2026-09-30. Codex hledá projektové instrukce od kořene projektu k pracovní složce. Proto mají oba samostatné repozitáře vlastní krátký vstup odkazující na okolní Undertaker. Načtení v konkrétním novém chatu je vhodné prakticky ověřit.
