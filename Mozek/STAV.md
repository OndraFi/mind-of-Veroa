# Aktuální stav a navázání práce

Aktualizováno: 2026-10-03 podle založení verzování společné paměti. Veroa je před prvními placenými zakázkami; cílem je levný start vedlejšího podnikání večer a o víkendech.

| Oblast | Co víme a odkud | Nejbližší krok |
| --- | --- | --- |
| Společná paměť | `STAV.md` slouží jako pořadník, tematické soubory jako zdroj aktuálního výsledku a `Denik/` jako předání mezi sezeními; načtení v novém chatu ověřeno 2026-10-01 | Pokračovat v této struktuře, nezakládat duplicitní obecnou work složku |
| Byznys | Bez placených zakázek, jeden praktický test | Získat přibližně 5 placených zakázek s minimální další investicí |
| Ceník | Schváleno Udržovací čištění / Důkladné čištění + volitelná impregnace, částky ponechány (R-006). Omezený test upsellu květin, luceren a svíček potvrzen (R-008). `CENIK.md` obsahuje rychlou tabulku, výchozí rozdíly pro ochranu, aktuální dodavatele a pracovní nákupní/prodejní ceny | Aktivovat B2B účty Miš Dekor a Velkoobchodplus, ověřit skutečné ceny košíku a venkovní vhodnost dvou kytic; koupit jen malý test. Doplnit konkrétní pravidlo dopravy služby; upsell zatím nezveřejňovat na webu |
| Marketing | Web, Instagram a Facebook existují. V `MARKETING.md` je od 2026-10-02 návrh organické publikace Reelu a pětidenního lokálního testu zpráv za celkem 300 Kč; nejde o schválenou ani spuštěnou kampaň | Zkontrolovat finální Reel a souhlas k záběrům, doplnit text a po schválení spustit jeden test s výzvou poslat obec a fotografie; měřit kvalifikované poptávky, nabídky a placené zakázky |
| Web | Větev `dev` byla 2026-10-02 fast-forward sloučena do `main` na `95bf93e`; stejný commit je na `origin/main` a Cloudflare jej nasadil. Produkční web používá veřejný CRM formulář. Následně odhalený občasný stav po F5 způsobovalo možné ztracení `load` události iframe před hydratací. Lokální commit `67089b8` vytváří iframe až po `onMounted` a má 12s pojistku; typecheck, 3/3 testy a statický build 19 URL prošly. Push selhal na chybějícím GitHub SSH klíči, takže oprava ještě není v produkci | Pushnout `67089b8`, počkat na Cloudflare deploy a opakovaným F5 ověřit načtení; potom odeslat produkční testovací poptávku s fotografií a ověřit záznam i e-mail |
| CRM | `api.veroa.cz` i `crm.veroa.cz` běží. Majitel 2026-10-02 potvrdil funkční faktury i doručování e-mailů po opravě DNS. Commit `2bdb812` přidává značkovou HTML šablonu, Bcc a přílohy. PDF má přednost; fotografie nad bezpečný rozpočet 14 MB se pošlou jako sedmidenní zabezpečené odkazy, takže velikost příloh e-mail nezablokuje. Nevyřešené chyby odeslání jsou červeně na přehledu i detailu a neúspěšný report lze výslovně opakovat. Backend 20 testů a oba buildy prošly. Commit zatím není pushnutý ani nasazený | Po výslovném schválení pushnout a nasadit backend i frontend; potom otestovat podlimitní i nadlimitní fotografie, PDF, Bcc, chybový banner a opakování reportu. Automatické mazání neřešit, dokud nebude schválena retenční doba |
| Provoz | Ruční čištění kartáči vyzkoušené; voskový flek zůstal | Viz PROVOZ.md; doplnit vhodné řešení vosku a zkušenost s impregnací |
| Náklady | Výdaje za prostředky a doménu potvrzené, složka bez dokladů | Doplnit skutečné částky pro kalkulaci |
| Záloha paměti | Kořenový Git repozitář pro `OndraFi/mind-of-Veroa` založen 2026-10-03; web, CRM a citlivé lokální soubory jsou vyloučené (R-009). První push blokuje chybějící nebo nenačtený GitHub SSH klíč | Zprovoznit SSH přístup pro alias `github-personal` a pushnout větev `main` |

## Aktivní pořadí práce podle majitele — aktualizováno 2026-10-02

1. Nasadit připravené úpravy CRM e-mailů a produkčně ověřit přílohy, patičku a skrytou kopii.
2. Rozhodnout retenční dobu fotografií; faktury zatím ponechat ve Fakturoidu a automaticky je nemazat.
3. Ceník impregnace a návrh upsellu jsou doplněné; nyní aktivovat B2B účty, ověřit konečné ceny a venkovní vhodnost kytic. Konkrétní pravidlo dopravy služby zůstává otevřené.
4. Po ověření koupit jen malý test květin a svíček; dražší lucerny zpočátku objednávat až po souhlasu zákazníka. Upsell zatím nezveřejňovat na webu.
5. Zkontrolovat Reel a po schválení spustit navržený malý lokální test zpráv za celkem 300 Kč; současně využít organické místní sdílení a měřit poptávky, nabídky a placené zakázky.

Pro orientaci už jsou základní otázky zodpovězené. Přesný rozpočet reklamy, časy a náklady zjistit při práci na příslušném tématu.

## Zjištěné rozpory a mezery

- **Ceny ochrany:** souhrn značky uvádí 1 790 / 2 290 / 2 990 / od 3 790 Kč, lokální data 2 290 / 2 890 / 3 790 / od 4 990 Kč. Podle R-006 jsou dosavadní částky zachovány jako výchozí celkové ceny individuálních nabídek; impregnace není třetí veřejný balíček. Příčina historického rozdílu zůstává neznámá.
- **Formulář:** produkční web od 2026-10-02 používá veřejný CRM formulář. HTTP dostupnost a přítomnost iframe jsou ověřené; po nasazení ještě zbývá jedna koncová testovací poptávka s fotografií a kontrola e-mailu.
- **Rozsah služeb:** PDF portfolio řeší i rozšíření, která jsou v lokálním webu vypnutá.
- **Dvojí souhrn CRM:** `SOUHRN_PROJEKTU.md` a `SOUHRN_PROJEKTU_VEROA_CRM.md` mají stejný text po normalizaci kódování/řádků. Pro orientaci používat druhý, oba ponechány.
- **Grafika:** souhrn značky zmiňuje bannery sociálních sítí; jejich soubory při cílené inventuře nebyly nalezeny. Umístění je třeba doplnit.

## Poslední předání

[2026-10-02 — interní ceník impregnace a upsellu](Denik/2026-10-02-cenik-impregnace-upsell.md). Doplněni dva doporučení dodavatelé, konkrétní testovací produkty a pracovní prodejní ceny. Nákup neproběhl; je třeba ověřit B2B ceny a venkovní vhodnost kytic.

[2026-10-01 až 2026-10-02 — příprava spuštění](Denik/2026-10-01-priprava-spusteni.md). CRM backend, frontend, projekt, uživatel i formulář jsou zprovozněné. Webový `dev` byl sloučen do `main` a veřejný web už načítá CRM formulář; zbývá koncové testovací podání po produkčním nasazení.

[2026-09-30 — cílová odměna 400 Kč/h](Denik/2026-09-30-cenik-odmena-400.md). Přijat výchozí cíl, nikoli nové veřejné ceny; v kalkulaci oddělit odměnu od krytí režie a rezervy.

[2026-09-30 — rozpracování ceníku a dopravy](Denik/2026-09-30-cenik-a-doprava.md). Návrh je otevřený, ceny a web beze změny; dojezd předběžně vymezen majitelem, časy ověřit na běžných realizacích.

[2026-09-30 — zařazení marketingového průzkumu](Denik/2026-09-30-marketingovy-pruzkum.md). Provozní kontext je v [předchozím předání](Denik/2026-09-30-provozni-realita.md). Příští obchodní téma: doladění uspořádání nabídky a ceníku. Struktura paměti zůstává pracovní první verzí; majitel upozornil, že vznikla před dokončením společné domluvy.
