# Souhrn úprav webu VEROA

Stav k 30. září 2026.

Tento dokument shrnuje hlavní změny, které jsme na webu provedli, a místa v projektu důležitá pro jeho další správu.

## Značka a kontaktní údaje

- Web a značka byly přejmenovány z Gravia na **VEROA**.
- Hlavní kontaktní údaje jsou:
  - telefon: **+420 725 212 870**,
  - e-mail: **info@veroa.cz**,
  - web: **https://veroa.cz**.
- Údaje jsou centralizované v `config/business.ts`, aby se nemusely měnit na více místech.
- Do hlavičky a patičky bylo doplněno logo VEROA.
- Byla připravena favicon, ikona pro Apple zařízení a samostatný symbol loga pro vyhledávače a strukturovaná data.

## Nabídka služeb

Nabídka byla upravena tak, aby odpovídala tomu, co lze v současnosti reálně poskytovat:

- úklid hrobového místa,
- běžné šetrné čištění,
- hloubkové čištění,
- impregnace vhodných povrchů,
- fotodokumentace výsledku.

Svíčka není prezentována jako placená služba. Může být drobnou pozorností, ale není součástí hlavního prodejního sdělení.

Ostatní plánované služby nebyly smazány. Jsou pouze skryté pomocí přepínačů v `config/offerings.ts`, takže je lze později znovu zapnout. Aktuálně jsou vypnuté například dekorace, věnce a doplňky, úprava okolí, fotokeramika, pravidelná celoroční péče a kamenické práce.

## Vymezení vůči kamenickým pracím

Na stránce služeb a ve FAQ je uvedeno, že VEROA neprovádí:

- opravy prasklin,
- renovace pomníků,
- obnovu nápisů,
- stabilizaci poškozených částí,
- jiné kamenické práce.

Pokud je hrob popraskaný, nestabilní nebo vyžaduje odbornou opravu, zákazník je odkázán na kameníka. Před čištěním nebo impregnací se posuzuje stav povrchu.

## Ceník a texty služeb

- Původní „Jednorázová péče“ byla přejmenována na **Běžnou péči**.
- Běžná péče je popsaná jako úklid a šetrné umytí průběžně udržovaného hrobu bez silných dlouhodobých nánosů.
- **Hloubkové čištění** je určené pro zanedbanější hroby, mech, řasy, lišejníky a odolnější usazeniny.
- Text o impregnaci byl upraven tak, aby vysvětloval rozdíly podle savosti, materiálu a povrchové úpravy.
- Cenová tabulka byla upravena pro mobilní zařízení.
- Ceny a jejich varianty jsou soustředěné v `data/pricing.ts`.

## Proces objednávky

- Z textu bylo odstraněno tvrzení, že se na hrob nejprve jezdí pouze podívat.
- Rozsah práce a cenu lze předběžně posoudit podle fotografií od zákazníka.
- Položení svíčky bylo odstraněno z hlavních placených kroků procesu.
- Komunikace zdůrazňuje nezávaznou nabídku, dohodnutý rozsah, šetrný postup a fotodokumentaci.

## Kontaktní formulář

HubSpot formulář byl opraven pro navigaci v Nuxt aplikaci. Skript se nyní vkládá pouze jednou, takže při přechodu na jinou stránku a návratu na kontakt nevzniká chyba o opakovaném načtení skriptu.

Komponenta také obsahuje:

- stav načítání,
- chybové hlášení při selhání skriptu,
- kontrolu chybějící konfigurace.

Implementace je v `components/forms/HubspotContactForm.client.vue`.

## Vzhled a použitelnost

- Hůře čitelný dekorativní font byl nahrazen čitelnějším písmem na nadpisech a dalších místech webu.
- Byly upraveny texty úvodní stránky, sekce „Pro koho je VEROA“, služby, postup objednávky a FAQ.
- Blogový výpis i samotné články byly upraveny do čistšího a modernějšího stylu, který navazuje na vizuální identitu VEROA.
- Navigace obsahuje odkaz na blog na počítači i v mobilním menu.

## Fotografie před a po

Původní interaktivní slider byl z hlavní stránky odpojen, protože nové fotografie nejsou pořízené ze stejného úhlu. Komponenta `components/ui/BeforeAfterSlider.client.vue` zůstala v projektu pro případné budoucí použití.

Na úvodní stránce jsou nyní dvě samostatné karty:

- **Před zásahem**,
- **Po zásahu**.

Pod nimi je popisek „Čištění teracového hrobu“. Na mobilu se fotografie skládají pod sebe.

Fotografie byly oříznuty na poměr 16:9 a převedeny do WebP. Pro každou existuje varianta 640 px a 1200 px, aby prohlížeč mohl stáhnout vhodnou velikost:

- `public/images/teracovy-hrob-pred-640.webp`,
- `public/images/teracovy-hrob-pred-1200.webp`,
- `public/images/teracovy-hrob-po-640.webp`,
- `public/images/teracovy-hrob-po-1200.webp`.

Obrázky používají responzivní `srcset`, lazy loading, pevně dané rozměry a popisné alternativní texty.

## Blog

Blog byl převeden na **Nuxt Content**. Zdrojem článků jsou Markdown soubory v `content/blog/`.

Aktuálně připravená témata zahrnují:

- správné čištění hrobu,
- čištění žuly a teraca,
- mech, řasy a lišejníky,
- odstranění rzi,
- impregnaci,
- použití tlakové vody,
- rozpoznání materiálu,
- četnost čištění,
- jarní údržbu,
- přípravu hrobu na Dušičky.

Každý článek má validovaná metadata, datum publikace s časovým pásmem, SEO titulek a popis, štítky a případně datum skutečné aktualizace. Šablona článku automaticky vytváří obsah z nadpisů; na počítači je obsah v postranním panelu a na mobilu ve skládací podobě.

Budoucí články se před buildem filtrují. Do veřejného webu, klientské databáze ani sitemap se nedostanou před nastaveným datem publikace. Protože je web statický, samotné uplynutí data článek nezveřejní — je potřeba nový deployment.

Pro automatické zveřejnění je připraven GitHub Actions workflow `.github/workflows/scheduled-blog-publish.yml`. Vyžaduje nastavit tajnou proměnnou `CLOUDFLARE_DEPLOY_HOOK_URL` s Cloudflare deploy hookem.

## SEO

Byly doplněny nebo upraveny:

- unikátní titulky a meta popisy stránek,
- Open Graph údaje,
- strukturovaná data organizace a blogových článků,
- smysluplná hierarchie nadpisů,
- popisné alternativní texty obrázků,
- interní odkazy mezi články a hlavními stránkami,
- favicon a logo používané vyhledávači,
- `robots.txt`,
- XML sitemap.

Soubor `public/robots.txt` odkazuje na `https://veroa.cz/sitemap.xml`.

Produkční sitemapu vytváří `scripts/generate-sitemap.mjs`. Blogové články se načítají automaticky z Markdown souborů a budoucí články jsou vynechány. Hodnota `<lastmod>` se určuje podle posledního Git commitu, který změnil danou stránku nebo její datové soubory. Pokud jsou relevantní změny ještě necommitnuté, použije se aktuální datum. Není tedy potřeba `lastmod` běžně upravovat ručně.

## Důležité soubory pro další správu

| Oblast | Soubor nebo složka |
| --- | --- |
| Kontaktní a firemní údaje | `config/business.ts` |
| Zapínání a skrývání služeb | `config/offerings.ts` |
| Popisy služeb | `data/services.ts` |
| Ceník | `data/pricing.ts` |
| FAQ | `data/faq.ts` |
| Články blogu | `content/blog/*.md` |
| Schéma blogových dat | `content.config.ts` |
| Výpis blogu | `pages/blog/index.vue` |
| Šablona článku | `pages/blog/[slug].vue` |
| Obsah článku | `components/BlogTableOfContents.vue` |
| Kontaktní formulář | `components/forms/HubspotContactForm.client.vue` |
| Generování sitemap | `scripts/generate-sitemap.mjs` |
| Filtrování budoucích článků | `scripts/prepare-blog.mjs` |
| Hlavní stránka | `pages/index.vue` |

## Kontrola a sestavení

Nejdůležitější příkazy:

```bash
npm run dev
npm run typecheck
npm run lint
npm run test:blog
npm run build
```

`npm run build` vytvoří statický web v `.output/public` a následně vygeneruje produkční sitemapu.

Pro produkční prostředí je vhodné použít Node.js 22.19 nebo novější podporovanou verzi. Lokální sestavení na Node.js 22.17.1 fungovalo, ale Nuxt upozorňuje na nižší než doporučenou verzi.

## Co řešit při dalších změnách

- Novou službu nejprve obsahově a cenově připravit a potom ji zapnout v `config/offerings.ts`.
- Nový článek přidat jako Markdown do `content/blog/` a vyplnit všechna povinná metadata.
- U nových technických tvrzení v článcích uvádět důvěryhodné zdroje.
- Novou běžnou stránku přidat také do seznamu v `scripts/generate-sitemap.mjs`.
- Po významných změnách spustit typecheck, test publikace blogu a produkční build.
- Po nasazení ověřit favicon a nové stránky v Google Search Console; změny ve výsledcích vyhledávání se mohou projevit až po novém procházení webu Googlem.
