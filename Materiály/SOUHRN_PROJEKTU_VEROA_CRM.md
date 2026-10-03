# Veroa CRM – souhrn projektu a dosavadní práce

Aktualizováno: 30. 9. 2026  
Repozitář: `OndraFi/veroa-crm`  
Aktuální produktový název: **Veroa CRM**

## 1. Proč CRM vzniklo

Veroa CRM je interní systém pro správu podnikání zaměřeného především na čištění a údržbu hrobů. Systém pokrývá cestu od první poptávky přes naplánování a provedení práce až po report zákazníkovi, fakturaci a evidenci platby.

Projekt byl původně založen jako GRAVIA CRM. Po zakoupení domény `veroa.cz` byl produkt přejmenován na **Veroa**. Název původní lokální složky a některé technické názvy Docker prostředků mohou stále obsahovat `gravia`; nemá to vliv na název produktu ani produkční adresy.

## 2. Cílové produkční uspořádání

| Část | Cílové umístění | Úloha |
| --- | --- | --- |
| CRM frontend | `https://crm.veroa.cz` na Cloudflare | Interní uživatelské rozhraní a veřejné formuláře |
| Backend API | `https://api.veroa.cz` na Renderu | Autentizace, obchodní logika a integrace |
| Databáze | MongoDB Atlas | Kontakty, poptávky, zakázky, hroby, úkoly a metadata |
| Soubory | privátní Cloudflare R2 bucket `veroa-crm` | Fotografie a dokumenty |
| E-maily | Mailjet | Potvrzení poptávky a dokončovací reporty |
| Faktury | Fakturoid | Vytvoření faktury a její následná evidence v CRM |

Frontend a backend mohou být na různých serverech. Je ale výhodné používat subdomény stejné domény, tedy `crm.veroa.cz` a `api.veroa.cz`, protože přihlašovací refresh cookie pak funguje spolehlivěji než při přímém volání adresy `*.onrender.com`.

Frontend je v repozitáři připravený pro Cloudflare Workers Static Assets přes `frontend/wrangler.jsonc`. Stejný Vite build ve složce `dist` lze obsloužit také přes klasický Cloudflare Pages workflow.

## 3. Použité technologie

### Backend

- Node.js a TypeScript
- Express
- tsoa pro definici controllerů, generování Express rout a OpenAPI dokumentu
- Mongoose a MongoDB
- JWT access token a rotující refresh token v bezpečné cookie
- S3 kompatibilní klient pro MinIO a Cloudflare R2
- Mailjet a Fakturoid jako volitelné produkční integrace
- Vitest pro automatické testy

tsoa nevytváří balíček, který by se musel samostatně publikovat. Při sestavení backendu vygeneruje soubory rout a OpenAPI specifikaci přímo do projektu. Ty se sestaví společně s backendem a nasadí jako jedna aplikace.

### Frontend

- Vue 3
- TypeScript
- Vite
- Vue Router
- Pinia
- responzivní vlastní CSS

### Lokální infrastruktura

Docker Compose spouští:

- MongoDB 8 jako single node replica set,
- MinIO jako lokální náhradu S3/R2,
- backend na portu `3000`,
- frontend na portu `5173`.

MinIO je bezplatný software běžící pouze lokálně v Dockeru. Není nutné si kvůli němu kupovat účet nebo službu. V produkci jeho roli přebírá Cloudflare R2.

## 4. Hlavní datové části CRM

### Projekt a uživatelé

- Data jsou vždy oddělena podle projektu.
- Uživatel má roli `ADMIN` nebo `WORKER`.
- Admin spravuje konfiguraci, uživatele a obchodní data.
- Worker pracuje především s přidělenými zakázkami a úkoly.
- Seed vytvoří projekt Veroa, výchozího admina a ukázkové formuláře.

### Kontakty

Kontakt obsahuje jméno, firmu, e-mail, telefon, adresu a interní poznámky. Při nové poptávce systém normalizuje e-mail a telefon a pokusí se najít existující kontakt. Nejednoznačné výsledky automaticky neslučuje, aby nezaměnil dvě osoby.

Kontakt dále eviduje:

- povolení nebo odmítnutí nabídkových e-mailů,
- datum změny tohoto nastavení,
- zdroj souhlasu,
- související poptávky,
- zakázky,
- hroby.

### Dynamické formuláře

Admin může upravovat formuláře bez zásahu do kódu. Podporované typy polí zahrnují text, delší text, e-mail, telefon, číslo, výběr, checkbox, datum a soubory.

U pole lze nastavit například:

- popisek a nápovědu,
- povinnost vyplnění,
- validační limity,
- možnosti výběru,
- mapování do kontaktu nebo preferovaného data dokončení.

Editor má samostatný náhled, který neblokuje uložení formuláře vlastní HTML validací. Po přidání nového pole se stránka přesune k nově vytvořené kartě. Checkboxy a karty polí byly vizuálně upraveny.

Každá odeslaná poptávka uchovává snímek tehdejší podoby formuláře. Pozdější změna formuláře proto neznehodnotí stará data.

### Poptávky

Poptávka může vzniknout:

- z veřejného webového formuláře,
- ručně po telefonátu,
- z e-mailu,
- jiným ručním zadáním.

Eviduje zdroj, kontakt, odpovědi formuláře, přílohy, stav, odpovědného pracovníka a volitelné požadované datum dokončení. Termín lze filtrovat a v rozhraní je vidět, co je po termínu.

Stavy poptávky jsou `NEW`, `CONTACTED`, `QUALIFIED`, `LOST` a `CONVERTED`.

Převod poptávky na zakázku je opakovatelně bezpečný: jedna poptávka nemůže vytvořit více zakázek. Cena a další údaje zakázky se dají doplnit a později změnit i po převodu.

### Zakázky

Zakázka může vzniknout převodem poptávky nebo ručně. Obsahuje:

- kontakt a případnou původní poptávku,
- navázaný hrob,
- název, popis a stav,
- cenu a měnu,
- odpovědného pracovníka,
- plánovaný, požadovaný a skutečný termín,
- město, hřbitov, adresu a číslo hrobu,
- fakturační údaje,
- způsob a stav platby,
- způsob předání faktury,
- způsob přijetí objednávky,
- informaci o dřívějším zahájení práce,
- fotografie před prací a po práci,
- úkoly, poznámky, fakturu a historii reportu.

Stavy zakázky jsou `NEW`, `WIP`, `DONE`, `WAITING_FOR_PAYMENT`, `CLOSED` a `CANCELLED`.

Zakázky lze filtrovat podle města, hřbitova, stavu, pracovníka a termínu. To umožňuje seskupit práci například na jeden den v Kutné Hoře a další den v Kolíně. Celé řádky tabulek jsou klikatelné, takže se CRM lépe ovládá i na menší obrazovce.

### Hroby

Hrob je samostatná znovupoužitelná karta. Jeden hrob může být navázán na více kontaktů i zakázek a jeden kontakt může být spojen s více hroby.

Karta hrobu obsahuje:

- vlastní pojmenování a typ hrobu,
- město a název hřbitova,
- adresu hřbitova, oddíl a číslo hrobu,
- GPS souřadnice,
- poznámky k přístupu,
- obecné interní poznámky,
- propojené kontakty, jejich vztah k hrobu a označení hlavního kontaktu,
- libovolný počet pohřbených osob.

U pohřbené osoby lze uložit jméno, datum narození, datum úmrtí, nápis a poznámku. CRM z data úmrtí vypočítává nadcházející výročí a zobrazuje je jako pracovní podklad.

Když je zakázka propojená s hrobem, používá jeho umístění, aby se údaje na několika místech nerozcházely. Pro starší zakázku lze kartu hrobu vytvořit přímo z jejího detailu.

Výroční e-maily se neposílají automaticky. Do vhodných příjemců se zařadí jen kontakt označený jako `Povoleno`. Budoucí zpráva by měla být před odesláním ručně schválena a formulována citlivě, například jako připomenutí možnosti úpravy hrobu nebo položení květin před výročím.

### Úkoly

Úkol může být napojený na poptávku nebo zakázku. Obsahuje název, popis, pracovníka, prioritu, termín a stav. Slouží pro konkrétní kroky, které nemají zapadnout v poznámkách.

### Soubory a fotografie

- V MongoDB se ukládají pouze metadata souboru.
- Samotný obsah je v MinIO nebo R2.
- Bucket je privátní.
- Backend zpřístupňuje soubory pomocí dočasných podepsaných adres až po kontrole přístupu.
- Podporovány jsou JPEG, PNG, WebP a PDF.
- Limit veřejného podání je 6 souborů po 10 MB.
- Fotografie zakázky se rozlišují na `BEFORE` a `AFTER`.

Cloudflare R2 slouží právě pro fotografie a dokumenty. Údaj „10 GB za měsíc“ v bezplatném limitu znamená průměrně uložený objem, nikoliv povinnost každý měsíc znovu nahrát 10 GB. Samostatně se počítají operace čtení a zápisu. Operace se mohou objevit už při práci v administraci R2 nebo při testování API, i když je výsledné uložené množství `0 B`.

R2 nemá jednoduchý pevný finanční strop. Je proto vhodné mít privátní bucket, omezení velikosti a počtu uploadů v backendu, sledování využití a billing upozornění v Cloudflare. Přístupový token je omezen jen na bucket `veroa-crm`. Omezení tokenu na jednu Render IP není praktické, protože Render používá sady odchozích IP a ty se mohou lišit podle služby nebo regionu.

Pro R2 se používá:

- `S3_BUCKET=veroa-crm`,
- `S3_REGION=auto`,
- S3 endpoint konkrétního Cloudflare účtu,
- Access Key ID a Secret Access Key z R2 API tokenu.

Public endpoint není potřeba a bucket nemá být veřejný.

## 5. E-maily, reporty a faktury

### Potvrzení poptávky

U každého formuláře lze nastavit:

- zda odeslat potvrzení,
- předmět,
- text zprávy,
- e-mail pro kopii.

Adresa a jméno odesílatele se nastavují na úrovni projektu. Skutečná odesílací adresa musí být ověřena u Mailjetu.

### Dokončovací report

V nastavení projektu lze měnit:

- jméno a ověřenou adresu odesílatele,
- Reply-To,
- kopii reportu,
- předmět a text,
- zda přiložit odkazy na fotografie před prací,
- zda přiložit fotografie po práci,
- zda zahrnout fakturu.

Akce **Dokončit a odeslat report** je výslovná. Pouhá změna stavu zakázky na dokončenou nic neposílá. Příjemcem je primárně fakturační e-mail, pokud je vyplněný; jinak se použije e-mail kontaktu.

Pokud má nastavení reportu zahrnovat fakturu, musí být faktura nejprve vytvořená. Systém tím brání odeslání neúplného reportu. U zákazníka bez e-mailu lze zakázku dokončit a stav platby či předání faktury vést ručně bez odeslání reportu.

Odeslání reportu je chráněné proti opakovanému kliknutí a výsledek se ukládá do e-mailového logu.

### Fakturace a platby

CRM shromažďuje fakturační adresu a rozlišuje spotřebitele a podnikatele. Dále eviduje hotovost, převod, kartu nebo poštovní poukázku a stav platby.

Faktura se vytváří explicitně přes Fakturoid. CRM uchovává její externí ID, číslo, adresu a stav; účetní systém zůstává zdrojem samotného účetního dokladu. Cenu lze na zakázce zadat i změnit před vytvořením faktury.

Pro zákazníka bez internetu jsou podporované praktické varianty:

- hotovost proti dokladu,
- bankovní převod podle údajů předaných na papíře nebo po telefonu,
- poštovní poukázka,
- osobní nebo poštovní předání faktury.

## 6. Obchodní workflow

1. **Přijetí poptávky**  
   Zákazník vyplní formulář nebo zavolá. Telefonická poptávka se vloží ručně a používá stejnou strukturu jako webová.

2. **Kontakt a kontrola údajů**  
   CRM najde existující kontakt nebo založí nový. Admin zkontroluje místo, rozsah práce, fotografie a požadovaný termín.

3. **Kvalifikace a nacenění**  
   Poptávka se označí jako kontaktovaná nebo kvalifikovaná. Do zakázky se doplní rozsah, cena, fakturační údaje a způsob platby.

4. **Převod na zakázku**  
   Poptávka se převede nebo se zakázka vytvoří ručně. Propojení se zdrojovou poptávkou zůstane zachované.

5. **Přiřazení hrobu**  
   Vybere se existující hrob nebo se založí nová karta. Doplní se hřbitov, poloha, přístup, kontakty a případně pohřbené osoby.

6. **Plánování trasy a času**  
   Vyplní se plánovaný termín a pracovník. Zakázky se vyfiltrují podle města a hřbitova a seskupí do jednoho výjezdu.

7. **Realizace**  
   Pracovník otevře zakázku, nahraje fotografie před prací, provede údržbu, doplní poznámky a nahraje fotografie po práci.

8. **Dokončení a faktura**  
   Admin zkontroluje cenu a fakturační údaje, vytvoří fakturu a podle způsobu předání ji doručí zákazníkovi.

9. **Report zákazníkovi**  
   U zákazníka s e-mailem se výslovnou akcí odešle upravitelný report s vybranými fotografiemi a fakturou. Bez e-mailu se dokončení zaznamená ručně.

10. **Platba a uzavření**  
    Po přijetí platby se zakázka označí jako zaplacená a uzavře.

11. **Další péče**  
    Historie zůstává u kontaktu i hrobu. Výročí a případné opakované služby mohou později vytvořit další poptávku nebo plánovaný úkol.

## 7. Opakované služby a katalog služeb

Probírali jsme také katalog předdefinovaných služeb, například:

- základní úklid,
- umytí,
- chemické čištění,
- odstranění mechu,
- výměna nebo položení květin,
- pravidelná čtvrtletní nebo roční péče.

Cílem je při nacenění vybrat položky a automaticky předvyplnit cenu. U opakované služby má systém po potvrzení návštěvy vypočítat další termín a vytvořit nový úkol nebo návštěvu, aby zakázka nezmizela v historii.

Tato část byla produktově navržena, ale samostatný katalog služeb, cenotvorba a automatické opakování zatím nejsou dokončené v kódu.

## 8. Přehled uživatelského rozhraní

CRM obsahuje nebo připravuje tyto hlavní sekce:

- Dashboard
- Poptávky
- Zakázky
- Kontakty
- Hroby
- Úkoly
- Formuláře
- Uživatelé
- Nastavení projektu

Na dashboardu se u posledních poptávek a zakázek zobrazují jména zákazníků místo technických ID. Tabulky mají klikatelné celé řádky. Formuláře, detaily a ovládací prvky byly upraveny pro pohodlnější použití na desktopu i mobilu.

## 9. Bezpečnostní rozhodnutí

- Hesla se hashují pomocí Argon2id.
- Access token je krátkodobý.
- Refresh token je rotující a v databázi je jen jeho hash.
- Produkční refresh cookie je `HttpOnly`, `Secure` a používá vhodný `SameSite` režim.
- Backend kontroluje projekt i roli u interních endpointů.
- CORS povoluje pouze nakonfigurovaný frontend.
- Veřejné formuláře mají limity požadavků a uploadů.
- Soubory nejsou veřejné a jejich názvy se nepoužívají jako storage key.
- Tajné klíče patří jen do proměnných prostředí a nesmí se commitovat.
- E-maily s nabídkou se nemají rozesílat bez příslušného právního základu a možnosti jednoduchého odhlášení.

Údaje o zemřelých samy o sobě nespadají pod GDPR stejným způsobem jako údaje žijících osob. Kontaktní údaje příbuzných a informace o jejich vztahu k hrobu jsou však osobní údaje žijících osob a musí se s nimi podle toho zacházet.

## 10. Lokální spuštění

1. Zkopírovat `.env.example` jako `.env`.
2. Nastavit dlouhý `JWT_SECRET`, heslo admina a stejné lokální přístupové údaje pro MinIO/S3.
3. Spustit:

   ```bash
   docker compose up --build -d
   ```

4. Vytvořit výchozí data:

   ```bash
   docker compose exec -T backend node dist/seed/seed.js
   ```

5. Otevřít `http://localhost:5173`.
6. Přihlásit se účtem `admin@example.local` a heslem z `SEED_ADMIN_PASSWORD`.

Další lokální adresy:

- API: `http://localhost:3000`
- OpenAPI: `http://localhost:3000/openapi.json`
- MinIO konzole: `http://localhost:9001`
- veřejný formulář hrobů: `http://localhost:5173/public/veroa/cisteni-hrobu`

## 11. Produkční proměnné prostředí

### Backend na Renderu

| Proměnná | Význam |
| --- | --- |
| `NODE_ENV=production` | Produkční režim |
| `MONGO_URI` | Připojení k MongoDB Atlas a databázi `veroa` |
| `JWT_SECRET` | Dlouhé náhodné tajemství |
| `WEB_ORIGIN=https://crm.veroa.cz` | Povolený frontend |
| `COOKIE_SAME_SITE=strict` | Cookie pro subdomény stejného webu |
| `S3_ENDPOINT` | Cloudflare R2 S3 endpoint |
| `S3_REGION=auto` | Region používaný R2 |
| `S3_BUCKET=veroa-crm` | Název bucketu |
| `S3_ACCESS_KEY` | R2 Access Key ID |
| `S3_SECRET_KEY` | R2 Secret Access Key |
| `SEED_ADMIN_PASSWORD` | Heslo prvního admina při seedu |
| `MAIL_FROM_NAME=Veroa` | Výchozí jméno odesílatele |

Volitelně se doplní přístupové údaje Mailjetu a Fakturoidu popsané v `DEPLOYMENT.md`.

### Frontend na Cloudflare

Build proměnná:

```text
VITE_API_URL=https://api.veroa.cz
```

Jde o proměnnou použitou při sestavení Vite. Musí být nastavena mezi build variables, nikoliv pouze jako runtime proměnná Workeru.

## 12. Hostingová rozhodnutí

Backend nelze bez většího přepsání jednoduše přesunout do Cloudflare Pages nebo Workeru. Současná aplikace je dlouho běžící Express server, používá Mongoose a očekává běžné Node.js prostředí. Cloudflare je vhodný pro frontend a R2; backend je připravený pro Docker službu na Renderu.

Render free služba se může po nečinnosti uspávat. Pravidelné externí pingování by spotřebovávalo dostupné provozní hodiny a obcházelo zamýšlené uspávání, proto není vhodným dlouhodobým řešením. Pro nepřetržitý provoz je potřeba placený backend, jiný poskytovatel s odpovídajícím bezplatným limitem, nebo vlastní server.

## 13. Co bylo ověřeno

Při posledních implementačních úpravách proběhlo:

- sestavení backendu,
- sestavení frontendu,
- automatické backendové testy,
- integrační ověření vytvoření hrobu, navázání kontaktu, uložení zemřelé osoby a vyhledání výročí,
- vizuální kontrola formulářů a evidence hrobů,
- lokální spuštění všech služeb přes Docker Compose.

Poslední ověřená sada obsahovala 15 úspěšných backendových testů v 6 souborech.

## 14. Dosud nehotové nebo plánované části

- katalog služeb a automatické nacenění,
- opakované návštěvy a automatické vytvoření dalšího termínu,
- přehled provozních výdajů a jednoduché porovnání obratu s náklady,
- automatická synchronizace změn stavu faktury z Fakturoidu,
- plný audit log všech uživatelských změn,
- kanban s přetahováním,
- inbox komunikace,
- automatizované výroční kampaně; nyní existuje pouze přehled vhodných výročí,
- ostré nasazení backendu, pokud ještě nebyl k dispozici Render limit nebo jiný server.

Výdaje lze později přidat jako jednoduchou provozní evidenci pro manažerský přehled. Pro účetnictví a daňové doklady má zůstat hlavním zdrojem účetní nebo fakturační systém.

## 15. Důležité soubory v repozitáři

| Soubor | Obsah |
| --- | --- |
| `README.md` | Rychlý přehled a lokální spuštění |
| `DEPLOYMENT.md` | Podrobný produkční postup a proměnné prostředí |
| `IMPLEMENTATION_PLAN.md` | Původní architektura a fáze implementace |
| `compose.yaml` | Lokální MongoDB, MinIO, backend a frontend |
| `render.yaml` | Render Blueprint backendu |
| `frontend/wrangler.jsonc` | Nasazení statického frontendu na Cloudflare |
| `backend/src/database/models.ts` | Datové modely a indexy |
| `backend/src/common/CrudController.ts` | Hlavní aplikační API |
| `backend/src/graves/service.ts` | Výpočet výročí a pomocná logika hrobů |
| `backend/src/generated/swagger.json` | Vygenerovaná OpenAPI dokumentace |

## 16. Historie hlavních kroků

1. Vytvoření základní architektury backendu, frontendu, MongoDB a MinIO.
2. Zavedení kontaktů, dynamických formulářů, poptávek, zakázek, úkolů a uživatelů.
3. Přejmenování produktu z Gravia na Veroa a nastavení domén `crm.veroa.cz` a `api.veroa.cz`.
4. Příprava Cloudflare R2 a oprava lokálního MinIO image.
5. Příprava Render Blueprintu a Cloudflare buildu.
6. Doplnění fakturačních údajů, způsobů platby a práce s fakturou.
7. Doplnění plánování podle města, hřbitova a termínu.
8. Doplnění preferovaného data dokončení do formulářů a poptávek.
9. Projití CRM, opravy ovládání tabulek, cen zakázek a názvů na dashboardu.
10. Vizuální úpravy formulářového editoru a konfigurovatelné e-mailové šablony.
11. Doplnění evidence hrobů, vazeb na kontakty a zakázky, zemřelých osob a přehledu výročí.

Tento dokument zachycuje stav a produktová rozhodnutí k 30. 9. 2026. Přesný aktuální postup nasazení má vždy přednost v `DEPLOYMENT.md` a technické detaily v aktuálním zdrojovém kódu.
