# Web a CRM

Aktualizováno: 2026-10-02. Backend i frontend CRM jsou spuštěné; připravené změny e-mailových příloh zatím nejsou nasazené.

**Následné sdělení majitele 2026-09-30:** web je spuštěný a poptávky řeší HubSpot. CRM ještě nespouštěl; chce je vyzkoušet 2026-10-01 po očekávaném obnovení limitu Renderu a doladit do první použitelné fáze. Toto je plán, nikoli provedené nasazení; limit poskytovatele nebyl ověřen.

## Web — `../Gravia/`

Samostatný Git repozitář, Nuxt/Vue, obsah přes Nuxt Content, statický build. Název složky je historický.

| Potřeba | Soubor v Gravia |
| --- | --- |
| Identita a kontakty | `config/business.ts` |
| Zapnuté služby | `config/offerings.ts` |
| Ceny a balíčky | `data/pricing.ts` |
| Popisy a otázky | `data/services.ts`, `data/faq.ts` |
| Blog | `content/blog/` |
| Kontaktní formulář | produkční `components/forms/CrmContactForm.client.vue`; původní HubSpot komponenta zůstává v repozitáři, ale kontaktní stránka ji nepoužívá |
| Spuštění a kontroly | `README.md`, `package.json` |

Skripty ověřené v package.json: `npm run dev`, `npm run typecheck`, `npm run lint`, `npm run test:blog`, `npm run build`. Při změně kódu zvolit relevantní kontroly.

**Ověření 2026-10-01:** typová kontrola původně odhalila chybu v `components/forms/HubspotContactForm.client.vue`; DOM reference byla změněna z hlubokého `ref` na `shallowRef`. Po opravě prošel typecheck, všechny 3 testy blogu i statické produkční sestavení 19 URL. Lint skončil bez chyb a s 25 existujícími stylovými varováními. Živá funkčnost HubSpot odeslání nebyla tímto ověřena.

Pro validaci odesílací domény Mailjetu byl do `public/` přidán prázdný soubor `9b510ca7942f34466409b346d89c30fc.txt`. Zdrojový i sestavený soubor mají ověřených 0 bajtů, jak vyžaduje dokumentace Mailjetu. Oprava HubSpot komponenty a validační soubor jsou v commitu `1f61176`, který Cloudflare Pages 2026-10-01 nasadil do produkce. Veřejná URL validačního souboru byla ověřena se stavem HTTP 200 a délkou 0 bajtů.

**Integrace CRM formuláře 2026-10-01 až 2026-10-02:** commit `4d8e86d` nahrazuje na kontaktní stránce HubSpot komponentu vloženým veřejným CRM formulářem z `https://crm.veroa.cz/public/veroa/cisteni-hrobu`, přidává konfigurovatelnou `NUXT_PUBLIC_CRM_FORM_URL`, záložní odkaz a odpovídající změnu textu ochrany osobních údajů. První Cloudflare preview ukázalo uvnitř iframe přihlášení, ačkoli přímá veřejná URL formuláře byla bez přihlášení ověřena jako funkční. Odstranění iframe sandboxu v `95bf93e` samo nepomohlo; problém vyřešila následně nasazená oprava CRM routeru v commitu `67c3ff3` a majitel potvrdil funkční preview včetně vzniku poptávky, kontaktu a fotografie. Dne 2026-10-02 byl webový `dev` fast-forward sloučen do `main` na `95bf93e`; Git reflog zaznamenává následný push na `origin/main`. Produkční `https://veroa.cz/kontakt/` odpovědělo HTTP 200 a zdroj obsahoval iframe CRM formuláře, jeho přímá veřejná URL také odpověděla HTTP 200. Typecheck, 3/3 testy blogu a statický build se sitemapou 19 URL prošly. Po nasazení se ukázalo, že při rychlém F5 mohl iframe dokončit načtení ještě před připojením Vue `load` listeneru a zůstat průhledný. Commit `67089b8` proto iframe vkládá až po `onMounted` a po 12 sekundách jej nouzově zobrazí; kontroly znovu prošly. Push tohoto commitu selhal na `Permission denied (publickey)`, takže produkce zatím tuto opravu neobsahuje.

## CRM — `../GRAVIA-CRM/`

Samostatný Git repozitář. Backend Node.js/TypeScript/Express/tsoa/Mongoose, frontend Vue/Vite/Pinia. Balíčky mají vlastní package.json v `backend/` a `frontend/`, ne v kořeni CRM.

Podle souhrnu pokrývá kontakty, formuláře, poptávky, zakázky, hroby, úkoly, fotografie, reporty a fakturaci. Katalog služeb, opakované návštěvy, nákladová evidence a další rozšíření jsou v souhrnu vedené jako nedokončené. Před implementací zkontrolovat aktuální kód; tato inventura není funkční audit.

- Architektura a orientace: `README.md`, `IMPLEMENTATION_PLAN.md`.
- Nasazení: `DEPLOYMENT.md`, `render.yaml`, `frontend/wrangler.jsonc`.
- Lokální infrastruktura: `compose.yaml`.
- Souhrn předchozí práce: `../Materiály/SOUHRN_PROJEKTU_VEROA_CRM.md`.
- Backend skripty: `npm run build`, `npm test` ve složce `backend/`.
- Frontend kontrola sestavení: `npm run build` ve složce `frontend/`.

**Ověření backendu 2026-10-01:** produkční TypeScript/tsoa build prošel a Vitest dokončil 6 souborů / 15 testů bez chyby. Majitel následně spustil službu `veroa-crm-api` v Renderu z GitHub repozitáře `OndraFi/veroa-crm`, větev `main`, Docker, kořen `backend`, Dockerfile `./Dockerfile`, Frankfurt a bezplatný tarif. Veřejný endpoint `https://veroa-crm-api.onrender.com/health` odpověděl HTTP 200 a `{"ok":true}`. Kořenová URL vrací aplikační 404, což samo o sobě není chyba health checku.

Kořenový `../GRAVIA-CRM/.env` není vhodný k importu do Renderu: kontrola bez vypsání hodnot ukázala nesíťové MongoDB URI, lokální `WEB_ORIGIN`, chybějící `COOKIE_SAME_SITE` a S3 nastavení neodpovídající cílovému R2 endpointu, regionu `auto` a bucketu `veroa-crm`; S3 access key navíc vypadá jako krátká vývojová hodnota. JWT a seed heslo splnily pouze minimální délku, jejich původ ani vhodnost pro produkci nebyly potvrzeny. Do Renderu je nekopírovat. Pro produkci použít Atlas URI, nové JWT tajemství, nové seed heslo a samostatné R2 přístupy podle `DEPLOYMENT.md`.

Majitel následně určil jako zdroj produkčních hodnot citlivý soubor `../render env.txt`. Kontrola bez vypsání hodnot našla všech 12 požadovaných klíčů; formátem odpovídají produkčnímu `NODE_ENV`, Atlas URI s databází `veroa`, R2 endpointu, regionu `auto`, bucketu `veroa-crm`, produkčnímu webovému originu a minimálním délkám tajemství. `COOKIE_SAME_SITE` má pro cílové uspořádání `crm.veroa.cz` / `api.veroa.cz` být `strict`. Majitel následně backend v Renderu spustil; konkrétní tajné hodnoty nejsou v této paměti zapsané.

**Produkční diagnostika e-mailu a faktury 2026-10-01 až 2026-10-02:** formulář má `confirmationEmail: true`. Kolekce `emaillogs` obsahuje potvrzení formulářů i e-mail dokončené zakázky ve stavu `SENT`, se `sentAt` a skutečnými Mailjet `providerMessageId`; Render při odeslání nehlásí chybu. Produkční `MAILJET_API_KEY` odpovídá Mailjet podúčtu `Veroa`, proto statistiky hlavního účtu nejsou důkazem aktivity tohoto klíče. Odesílatel `info@veroa.cz` i doména `veroa.cz` jsou v Mailjetu aktivní, ale autentizace domény má SPF i DKIM ve stavu `Error`. Veřejná DNS kontrola 2026-10-02 našla SPF `v=spf1 include:_spf.mx.cloudflare.net ~all`, tedy bez Mailjetu, a nenašla žádný TXT na `mailjet._domainkey.veroa.cz`. Mailjet požaduje jediný kombinovaný SPF `v=spf1 include:spf.mailjet.com include:_spf.mx.cloudflare.net ~all` a DKIM TXT na hostu `mailjet._domainkey` s hodnotou z jeho průvodce. To je potvrzená příčina nedůvěryhodné autentizace a pravděpodobný důvod nedoručování; konkrétní stav jednotlivých zpráv je po opravě třeba ověřit v Activity podúčtu `Veroa`.

Majitel 2026-10-02 potvrdil, že faktury už fungují. Předchozí OAuth 401 je tím provozně uzavřený; přesný způsob opravy přístupů nebyl samostatně auditován.

Majitel následně 2026-10-02 potvrdil, že po opravě DNS e-maily chodí, a rozhodl, že kopie formulářových e-mailů má být skrytá a zákaznické e-maily mají mít profesionální vzhled, fotografie a fakturu jako skutečné přílohy. Lokální implementace mění `Cc` na `Bcc`, přidává jednotnou HTML hlavičku a patičku, přikládá fotografie už při potvrzení poptávky, načítá fotografie zakázky z R2 a PDF faktury přes oficiální Fakturoid endpoint. PDF faktury má v rozpočtu příloh přednost; přílohy se plní jen do 14 MB a nadlimitní fotografie zůstávají jako sedmidenní zabezpečené odkazy, takže limit Mailjetu 15 MB nezablokuje zprávu. Selhání načtení jednotlivé přílohy také nezastaví e-mail, pokud lze odeslat tělo a odkazy. Odpověď API bez Message ID nebo se stavem chyby se nepovažuje za úspěch.

Neodeslané zprávy se ukládají jako `FAILED`, zapisují se do serverového logu a administrátor je vidí v červeném banneru na přehledu i v detailu poptávky či zakázky. Po neúspěšném reportu je dostupné výslovné tlačítko pro nový pokus; po úspěchu se předchozí chyba označí jako vyřešená. Výpadek samotného Mailjetu nelze spolehlivě hlásit dalším e-mailem přes stejnou službu, proto je CRM banner hlavní nezávislou provozní kontrolou. Backend dokončil 6 souborů / 20 testů a backendový i frontendový build prošly. Změny jsou v lokálním commitu `2bdb812` (`Improve email delivery and attachment handling`), zatím nejsou pushnuté ani nasazené.

Automatické mazání fotografií ani faktur nebylo zavedeno. Majitel uvedl záměr fotografie později mazat, ale retenční doba není rozhodnutá. Faktury zatím zůstávají ve Fakturoidu.

Commit CRM `884b114` s kontrolou kompletní konfigurace, čitelnějším logováním a lepším stavem formuláře je nasazený. Následný lokální commit `aefc40a` provádí ověření OAuth ještě před založením `invoicereferences`, takže odmítnuté přihlášení už nezanechá falešný zámek proti duplicitě a administrátor dostane čitelnou hlášku. Backend dokončil 6 souborů / 17 testů a build bez chyby. Commit zatím není pushnutý; produkční push do `main` vyžaduje výslovné potvrzení majitele.

Cílové uspořádání ze souhrnu: crm.veroa.cz / Cloudflare, api.veroa.cz / Render, MongoDB Atlas, privátní R2, Mailjet a Fakturoid. Majitel 2026-10-01 potvrdil funkční `api.veroa.cz` i `crm.veroa.cz`. Kvůli nedostupnému Render shellu ručně založil v Atlasu projekt Veroa, administrátora a formulář `cisteni-hrobu` podle lokální definice a potvrdil přihlášení do CRM. Tajné přihlašovací údaje nejsou v paměti uloženy.

## Nevyřešená návaznost webu a CRM

Produkční web už používá CRM formulář. Preview tok byl dříve ověřen včetně fotografie; po produkčním nasazení zbývá odeslat jednu novou testovací poptávku a ověřit vznik kontaktu a poptávky, otevření fotografie a doručení potvrzovacího e-mailu.
