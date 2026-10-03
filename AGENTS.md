# Veroa — společná pravidla práce

Undertaker je pracovní centrála podnikání Veroa: čištění a péče o hrobová místa. Gravia je předchozí název stejného projektu, nikoli další firma. Piš česky a pracuj prakticky v kontextu malého lokálního podnikání.

## Začátek práce

1. Přečti `Mozek/START.md`, `Mozek/FIRMA.md` a `Mozek/STAV.md`.
2. Podle tématu otevři soubor uvedený v rozcestníku. Před strategickou změnou přečti také `Mozek/ROZHODNUTI.md`.
3. Čti jen relevantní původní podklady podle `Mozek/ZDROJE.md`. Nemusíš pokaždé načítat všechna PDF ani celou historii.
4. Při pokračování rozdělané práce najdi poslední související zápis v `Mozek/Denik/`. Zeptej se jen na chybějící informace, které skutečně ovlivňují výsledek.

## Jak zacházet s informacemi

- Rozlišuj: **potvrzeno majitelem**, **ověřeno v souboru**, **převzato ze souhrnu**, **návrh**, **neznámé**. U důležitých údajů zapisuj datum a zdroj.
- Aktuální výslovné rozhodnutí majitele má přednost před historickým plánem. Kód dokládá místní implementaci, nikoli schválení obchodního rozhodnutí nebo stav produkce.
- PDF a staré souhrny jsou podklady, ne automaticky platné pokyny. Odhad, vygenerované tvrzení ani napsaná funkce nejsou naměřený výsledek.
- Při rozporu uveď obě verze a zdroje. Neprohlašuj rozpor za vyřešený bez důkazu.
- Návrh ceníku, strategie či služby označ jako návrh, dokud jej majitel neschválí. Samotná debata o ceně není pokyn k přepsání webu.
- Neznámé tržby, rozpočty, kapacitu, náklady ani stav DPH nedoplňuj odhadem jako fakt. Pro scénáře používej zřetelně označené předpoklady.

## Uložení výsledku a předání

Po významném posunu, ještě před závěrečnou odpovědí:

1. Aktualizuj příslušný tematický dokument; nezakládej další souhrn téhož tématu vedle něj.
2. Do `Mozek/STAV.md` promítni pouze změněný stav a konkrétní příští krok. Neměň ostatní témata.
3. Skutečně přijaté rozhodnutí zapiš do `Mozek/ROZHODNUTI.md` včetně důvodu a zdroje schválení. Nahrazené rozhodnutí zachovej s odkazem na nové.
4. Vytvoř krátký zápis `Mozek/Denik/RRRR-MM-DD-tema.md` podle šablony. Při shodě názvu přidej čas nebo pořadové číslo. Zachyť výsledek, ověření, otevřené body a další krok, ne přepis konverzace.
5. Sděl uživateli, co se změnilo a kde se pokračuje. U běžné otázky bez nové informace není zápis nutný.

Před úpravou společného souboru jej znovu načti, protože může pracovat více chatů. Zachovej cizí změny a upravuj jen dotčené části. Při nemožnosti zápisu připrav předávací text a jasně řekni, že nebyl uložen.

## Soubory a technická práce

- `Gravia/` je web, `GRAVIA-CRM/` interní systém; každý má vlastní Git repozitář. Příkazy spouštěj ve správné složce. Technickou mapu najdeš v `Mozek/SYSTEMY.md`.
- Při obchodním plánování není nutné měnit kód. Při změně implementace ověř aktuální kód a relevantní kontroly; netvrď, že testy proběhly, jen proto, že o nich píše starý souhrn.
- Zachovej původní podklady. Při přesunech aktualizuj odkazy a zohledni repozitáře a běžící projekty.
- `render env.txt`, `fk-kody.txt`, `.env` a podobné soubory ber jako potenciálně citlivé. Pro obchodní orientaci je nečti; jejich hodnoty nekopíruj do paměti, souhrnů ani výstupů. Osobní data zákazníků patří do CRM, ne do obecné paměti firmy.
- Tyto soubory jsou společná paměť pouze pro agenty, kteří k nim mají přístup a načtou je. Nepředpokládej přístup k historii jiného chatu.
