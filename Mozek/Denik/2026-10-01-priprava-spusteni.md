# Příprava spuštění Veroa

- Datum: 2026-10-01.
- Autor / chat: Codex, spuštění webu a CRM.
- Stav: CRM i produkční webový formulář spuštěné; čeká na koncové testovací podání po produkčním nasazení.
- Zadání: uspořádat dnešní práci, spustit CRM backend na Renderu, ověřit web, připravit interní ceník, následnou rešerši upsellu a malý lokální marketingový test.

## Výsledek

Potvrzeno použití existující struktury: `STAV.md` jako pořadník, tematické soubory jako aktuální výsledek a `Denik/` jako předání mezi sezeními. Aktualizovány `STAV.md`, `SYSTEMY.md`, `CENIK.md` a `MARKETING.md` bez založení duplicitní work složky.

Web: opravena typová chyba HubSpot komponenty změnou DOM reference na `shallowRef`. Po opravě prošel typecheck, 3/3 testy blogu a statické produkční sestavení 19 URL; lint bez chyb, 25 stylových varování. Živá testovací poptávka nebyla odeslána. Pro validaci Mailjetu byl přidán prázdný `public/9b510ca7942f34466409b346d89c30fc.txt`; zdroj i build mají 0 bajtů. Commit `1f61176` byl následně nasazen přes Cloudflare Pages a veřejný validační soubor odpovídá HTTP 200 s délkou 0 bajtů.

CRM backend: produkční build prošel, 6 testovacích souborů a 15 testů prošlo. Majitel následně spustil Render službu `veroa-crm-api` z `OndraFi/veroa-crm`, větev `main`, Docker, kořen `backend`, Dockerfile `./Dockerfile`, Frankfurt a bezplatný tarif. Veřejný endpoint `/health` odpověděl HTTP 200 a `{"ok":true}`. V Renderu byla přidána vlastní doména `api.veroa.cz`; čeká na DNS.

Majitel následně potvrdil funkční `api.veroa.cz` i `crm.veroa.cz`. Kvůli nedostupnému Render shellu ručně založil v Atlasu projekt, administrátora a formulář čištění hrobu a potvrdil přihlášení. Ve webovém repozitáři byla založena větev `dev`; commit `4d8e86d` zkušebně nahrazuje HubSpot vloženým CRM formulářem a aktualizuje informace o ochraně osobních údajů. První preview uvnitř iframe zobrazilo přihlášení, přestože přímá veřejná URL funguje. Odstranění iframe sandboxu v `95bf93e` nestačilo. CRM commit `67c3ff3` proto označuje veřejné routy metadaty a odkládá kontrolu relace do připravení routeru; frontend build prošel, push musí kvůli SSH klíči provést majitel.

Doplnění 2026-10-02: majitel potvrdil funkčnost CRM a schválil spuštění formuláře. Webový `dev` byl fast-forward sloučen do `main` na `95bf93e`; během ověřování Git reflog zaznamenal push stejného commitu na `origin/main`. Produkční kontaktní stránka následně odpověděla HTTP 200 a obsahovala iframe `https://crm.veroa.cz/public/veroa/cisteni-hrobu`; přímá URL formuláře také odpověděla HTTP 200. Typecheck, 3/3 testy blogu a statický build se sitemapou 19 URL prošly. Původní samostatná lokální změna mažící Mailjet validační soubor nebyla součástí merge a zůstala připravená ve working tree.

Po spuštění majitel zjistil, že se formulář při některých F5 nenačte. Příčinou byl závod: iframe byl už v předgenerovaném HTML a jeho `load` událost mohla proběhnout před hydratací Vue, takže `loaded` zůstalo `false` a iframe průhledný. Commit `67089b8` vytváří iframe až po `onMounted` a přidává 12sekundovou pojistku. Typecheck, 3/3 testy a celý build 19 URL prošly. Push na `origin/main` selhal kvůli chybějícímu SSH klíči; oprava proto ještě není nasazená.

## Rozhodnutí a ověření

Majitel určil dnešní pořadí: technické spuštění, interní ceník, rešerše několika květin a luceren a následně přibližně 300 Kč lokální test Facebook propagace. Částka 300 Kč je zatím návrh, ne provedený nebo závazně schválený výdaj. Upsell zatím nemá být na webu. Nové obchodní ceny nebyly určeny, proto `ROZHODNUTI.md` beze změny.

Hodnoty `.env` nebyly zapsány do paměti. První kontrola pouze potvrdila přítomnost klíčů; následná kontrola formátu po upozornění majitele prokázala, že jde převážně o lokální vývojovou konfiguraci. MongoDB není Atlas SRV, `WEB_ORIGIN` je lokální, `COOKIE_SAME_SITE` chybí a S3 nastavení neodpovídá cílovému R2 endpointu, regionu ani bucketu. JWT a seed heslo splňují pouze délku, nikoli ověřený produkční původ. Tento `.env` do Renderu neimportovat.

Majitel určil jako produkční zdroj citlivý `../render env.txt` a schválil jeho použití v Renderu. Bez vypsání hodnot bylo ověřeno všech 12 povinných klíčů a produkční tvar Atlas/R2 konfigurace. Jediná zjištěná odchylka je `COOKIE_SAME_SITE`, který má být pro cílové uspořádání nastaven na `strict`. Majitel hodnoty následně do Renderu vložil a backend spustil; konkrétní tajné hodnoty nebyly zapsány do paměti ani výstupu.

## Otevřené body

- Pushnout webový commit `67089b8`, počkat na Cloudflare deploy a několika F5 ověřit stabilní načtení formuláře.
- Odeslat jednu novou poptávku z produkčního `veroa.cz/kontakt/` a v CRM ověřit vznik kontaktu a poptávky, otevření fotografie a potvrzovací e-mail.
- V Mailjetu spustit ověření domény a samostatně ověřit živé doručení HubSpot poptávky.
- Stanovit dopravu a impregnaci v interním ceníku, následně udělat aktuální dodavatelskou rešerši květin a luceren.
- Teprve potom připravit konkrétní Facebook test a jeho měření.

## Přesný další krok

Z prostředí s funkčním GitHub SSH klíčem v `Gravia/` spustit `git push origin main`, počkat na Cloudflare deploy a ověřit opakovaný F5. Potom odeslat celý testovací tok s fotografií, v CRM zkontrolovat výsledek a potvrdit doručení e-mailu.
