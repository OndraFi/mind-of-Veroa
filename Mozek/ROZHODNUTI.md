# Rozhodnutí

Sem patří přijatá rozhodnutí. Návrhy zůstávají v tematických souborech. Zapisuj datum, co platí, proč, zdroj schválení a případné nahrazení.

## R-001 — značka Veroa

- Datum záznamu: 2026-09-30.
- Stav: potvrzeno majitelem v úvodním zadání tohoto chatu.
- Rozhodnutí: používat název Veroa. Gravia je historický název stejného podnikání.
- Dopad: staré názvy složek neinterpretovat jako oddělené značky. Toto rozhodnutí samo o sobě nepřejmenovává technické prostředky.

## R-002 — Undertaker jako společná centrála

- Datum: 2026-09-30.
- Stav: záměr potvrzen majitelem v úvodním zadání.
- Rozhodnutí: soustředit zde kontext firmy, podklady, rozhodnutí a návaznost práce pro Codex i Claude.
- Provedení první verze: společná složka `Mozek/`, vstupy `AGENTS.md` a `CLAUDE.md`, odkazy na původní podklady. Konkrétní uspořádání je první implementace asistenta, nikoli další schválené obchodní rozhodnutí.

## R-003 — levný start vedlejšího podnikání

- Datum: 2026-09-30.
- Stav: potvrzeno majitelem v následných odpovědích tohoto chatu.
- Rozhodnutí: rozjíždět Veroa večer a o víkendech s minimální další investicí. Po přibližně pěti placených zakázkách zvažovat větší reinvestice. Menší výdaj do reklamy je přijatelný, konkrétní limit není stanovený.
- Důvod: nejprve získat zákazníky a ověřit, že podnikání funguje. Hlavní příjem připadá v úvahu až při finanční smysluplnosti.
- Dopad: plánovat podle BYZNYS.md; žádný konkrétní nákup nebo reklamní útrata tím nejsou objednány.

## R-004 — zaměření první fáze

- Datum: 2026-09-30.
- Stav: potvrzeno majitelem v následných odpovědích tohoto chatu.
- Rozhodnutí: nyní začít čištěním hrobů, s impregnací již uvedenou na webu. Kytice, lucerny a pravidelnou péči řešit později, další venkovní povrchy až jako případné budoucí rozšíření.
- Dopad: nerozšiřovat automaticky nabídku podle původních PDF. Rozdělení cenových balíčků bylo při přijetí otevřené; tuto část následně upřesňuje R-006. Odklad kytic a luceren byl 2026-10-02 částečně nahrazen omezeným testem v R-008. Ostatní rozsah první fáze zůstává platný.

## R-005 — počáteční cílová odměna 400 Kč/h

- Datum: 2026-09-30.
- Stav: přijato jako výchozí cíl pro kalkulaci, s následným ověřením.
- Rozhodnutí: počítat 400 Kč za hodinu vlastního času včetně cestování, po nákladech podnikání a před daněmi.
- Důvod: výchozí požadovaná odměna pro rozjezd; ochotu zákazníků zaplatit výsledné ceny teprve ověřit.
- Zdroj schválení: majitel v chatu o ceníku odpověděl „Začal bych 400 uvidíme jestli lidi tohle zaplatí...“ na otázku výslovně vymezující odměnu po nákladech, před daněmi a včetně cest.
- Dopady: CENIK.md a budoucí kalkulace. Nejde o veřejnou hodinovou sazbu ani schválení nové struktury nabídky, konkrétního ceníku či změny webu.

## R-006 — dvě úrovně čištění a ochrana jako doplněk

- Datum: 2026-09-30.
- Stav: přijato; upřesňuje otevřenou strukturu nabídky z R-004.
- Rozhodnutí: přejmenovat Běžnou péči na Udržovací čištění, Hloubkové čištění na Důkladné čištění a ochrannou impregnaci prezentovat samostatně jako volitelný doplněk pod hlavními službami. Rozsah viz CENIK.md. Částky za čištění ponechat; dosavadní celkové ceny čištění s ochranou zachovat jako výchozí pro individuální nabídky, nezavádět pevný samostatný příplatek bez ověření.
- Důvod: zákazník snadněji rozliší náročnost čištění od ochrany povrchu; rentabilitu ověřit na skutečných zakázkách.
- Zdroj schválení: majitel po konkrétním návrhu odpověděl „Okej to ce mi celkem líbí jak jsi to přeformuloval. Takhle to hoď na web a zapiš do mozku jestli někde máme jak máme definované služby.“
- Dopady: úvod webu, služby, ceník a FAQ; CENIK.md, FIRMA.md, STAV.md. Konečná cena včetně dopravy a případné druhé návštěvy předem. Odolné skvrny a mimořádné zanedbání individuálně. Nemění nabídku dalších služeb ani neprokazuje ziskovost.

## R-007 — zákaznické e-maily s přílohami a skrytou kopií

- Datum: 2026-10-02.
- Stav: potvrzeno majitelem v chatu; implementace připravena lokálně, zatím nenasazena.
- Rozhodnutí: potvrzení poptávky a report dokončené zakázky mají používat profesionální značkovou šablonu. Fotografie se mají posílat jako přílohy; u reportu také PDF faktura. Nastavená kopie e-mailu má být skrytá přes Bcc, aby zákazník adresu kopie neviděl. Velké fotografie nesmějí zablokovat odeslání: PDF má přednost a soubory, které se nevejdou do limitu, se předají zabezpečenými odkazy. Pokud samotné odeslání selže, administrátor to musí vidět v CRM a u reportu musí mít možnost nový pokus výslovně spustit.
- Důvod: zákazník má dostat kompletní a profesionálně působící předání bez závislosti jen na dočasných odkazech; interní adresa kopie nemá být zveřejněna.
- Zdroj schválení: výslovný požadavek majitele v chatu 2026-10-02.
- Dopady: CRM backend, dashboard, detail poptávky a zakázky, nastavení projektu a formuláře, e-mailové šablony. Přílohy podléhají limitu Mailjetu 15 MB; implementace používá rozpočet 14 MB. Retenční doba fotografií ani faktur tímto rozhodnutím stanovena není.

## R-008 — omezený upsell při čištění

- Datum: 2026-10-02.
- Stav: potvrzen záměr a rozsah testu; konkrétní produkty a prodejní ceny zůstávají návrhem k ověření.
- Rozhodnutí: vedle čištění začít zákazníkům individuálně nabízet malý výběr umělých květin, hřbitovních luceren a svíček. Vést nákupní i prodejní ceny ve společném interním `CENIK.md`.
- Důvod: zvýšit hodnotu jedné návštěvy a vyřešit zákazníkovi současně péči i drobnou výzdobu bez širokého skladu a velké počáteční investice.
- Zdroj schválení: majitel v chatu 2026-10-02: „K čištění bych dál chtěl dělat i upsel. Takže pár kytek, luceren a nějaké svíčky které bych mohl prodávat.“
- Dopady: částečně nahrazuje odklad kytic a luceren v R-004. Upsell zatím není pokyn ke zveřejnění na webu ani ke konkrétnímu nákupu. Sortiment, venkovní vhodnost, skutečné B2B ceny a marže je třeba ověřit malým testem podle `CENIK.md`.

## R-009 — samostatné verzování společné paměti

- Datum: 2026-10-03.
- Stav: potvrzeno majitelem v chatu.
- Rozhodnutí: verzovat kořen Undertaker v repozitáři `git@github-personal:OndraFi/mind-of-Veroa.git`, včetně `Mozek/`, původních materiálů a grafiky.
- Důvod: zálohovat a sdílet společnou paměť projektu nezávisle na aplikacích.
- Zdroj schválení: majitel výslovně požádal dát složku „se vším všudy do gitu“ a vyloučit web a CRM, protože již mají vlastní repozitáře.
- Dopady: `Gravia/`, `GRAVIA-CRM/`, `render env.txt`, `fk-kody.txt` a obecné lokální tajné soubory jsou vyloučené přes kořenový `.gitignore`. Web a CRM se nadále verzují odděleně. První push je podmíněný funkčním GitHub SSH klíčem pro alias `github-personal`.

## Šablona nového záznamu

- ID a datum:
- Stav: přijato / nahrazeno rozhodnutím …
- Rozhodnutí:
- Důvod:
- Zdroj schválení: konkrétní sdělení majitele nebo jeho dokument.
- Dopady: dotčené dokumenty, web, CRM, provoz.
