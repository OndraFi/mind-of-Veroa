# Veroa — pracovní centrála

Začni v [Mozek/START.md](Mozek/START.md). Najdeš tam mapu firmy, současný stav a odkazy podle tématu.

Nové obchodní chaty otevírej nad složkou **Undertaker**. Můžeš zadat například „Pojďme řešit ceník“, „Připrav marketingový plán“ nebo „Pokračuj v byznys plánu“. Pravidla v `AGENTS.md` a `CLAUDE.md` směrují agenta do stejné paměti.

Paměť tvoří uložené soubory; obsah jiné konverzace se sem sám nepřenese. Již otevřenému chatu napiš: „Načti AGENTS.md a Mozek/START.md v Undertaker a dál udržuj společnou paměť podle těchto pravidel.“ V novém chatu lze ověřit načtení zadáním: „Shrň, co je Veroa, co je rozdělané a z jakých souborů vycházíš.“

| Umístění | Co zde je |
| --- | --- |
| `Mozek/` | Průběžně udržovaná paměť a pracovní dokumenty |
| `Materiály/` | Původní PDF a souhrny dřívějších chatů |
| `Gravia/` | Web Veroa, samostatný Git repozitář |
| `GRAVIA-CRM/` | CRM Veroa, samostatný Git repozitář |
| `Náklady/` | Místo pro nákladové podklady; při založení paměti prázdné |
| `designs/` | Návrh letáku; další grafika je uvnitř webu |

Názvy Gravia jsou historické. Přejmenování složek není nutné pro fungování paměti. Původní soubory zůstaly na místě.

Kořen Undertaker je verzovaný samostatně v repozitáři `OndraFi/mind-of-Veroa`. Web `Gravia/`, CRM `GRAVIA-CRM/` a lokální soubory s přístupovými údaji jsou vyloučené přes `.gitignore`; aplikace zůstávají ve svých vlastních repozitářích. Dokumentace, původní materiály, grafika a prázdná složka `Náklady/` patří do tohoto repozitáře.
