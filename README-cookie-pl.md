# Cookie lišta — PL verze (Consent Mode V2 + GA4 + Ads)

Tento balíček řeší **souhlas s cookies** pro polskou verzi webu.

- **Consent Mode V2** se inicializuje **před** načtením `gtag.js`.
- `gtag.js` se načítá **jen jednou** (s GA4 ID) a následně se nakonfigurují:
  - GA4 (PL): `G-E8L8HY25GB`
  - Google Ads (PL): `AW-10853105582`
- Měření a Ads se aktivují až po udělení souhlasu přes banner/modál.

## Soubory v balíčku
- `cookie-pl-pretty.html` — čitelná, komentovaná verze (UI v polštině).
- `cookie-pl-min.html` — minifikovaná verze (UI v polštině), doporučeno pro produkci.

## Co smazat z kódu webu
Odstraň **všechny** následující skripty mimo těchto souborů:
- `https://www.google-analytics.com/analytics.js` *(legacy UA — smazat)*
- jakékoliv `...gtag/js?id=UA-...` *(Universal Analytics je ukončený — smazat)*
- **duplicitní** načítání `gtag/js` pro **stejná ID** (i s parametry `&cx=...`)
- GA4 ID `G-BHQEJ5EC2D` *(nepatří do PL — smazat)*

> **Důležité:** GA/Ads skripty **nepřidávej** nikam jinam. Vše je již obsaženo v těchto HTML souborech.

## Co ponechat
V cookie souboru (PRETTY/MIN) jsou **jediná** povolená měření pro PL:
- **GA4:** `G-E8L8HY25GB`
- **Google Ads:** `AW-10853105582`

## Kam vložit
- Vlož **jeden** z HTML souborů (doporučujeme `cookie-pl-min.html`) do layoutu PL webu tak, aby se spustil na všech stránkách.
- Dejte pozor, aby soubor nebyl zahrnut dvakrát.

## URL zásad cookies
- V HTML je odkaz `/page/cookies`. Pokud má PL web jinou URL, uprav na správnou v:
  - banneru („Więcej informacji“)
  - modálu („Więcej o plikach cookie i ich celach“)

## Úprava vzhledu
- Primární barva: `:root { --ccy-primary: #49A4EE; }`
- Tlačítka/bannery/modál: třídy `.ccy-btn`, `.ccy-banner`, `.ccy-modal`
- Plovoucí tlačítko „Zarządzaj plikami cookie“: `.ccy-manage-trigger`

## Logika souhlasu
- **Default:** `analytics/ad_storage/ad_user_data/ad_personalization = denied`
- Kliky uživatele:
  - „Akceptuj wszystko“ → vše `granted`
  - „Odrzuć“ / „Odrzuć wszystko“ → vše `denied`
  - „Ustawienia“ → ruční volba 4 přepínačů
- Stav se ukládá do `localStorage` pod klíčem `mojra_consent_v2_pl` na **180 dní**.

## Test po nasazení
1. Otevři PL web v anonymním okně → zobrazí se banner.
2. Před souhlasem zkontroluj v DevTools `window.dataLayer`, že `consent default` má `denied`.
3. Klikni „Akceptuj wszystko“ → v `dataLayer` se objeví `consent update` s `granted`.
4. Reload → banner se už nezobrazí, zůstane kulaté tlačítko vlevo dole.
5. Ověř události v GA4 DebugView/Tag Assistant.

## Mini API (volitelně)
- `window.mojraConsent.get()` → vrátí uložený stav (nebo `null`)
- `window.mojraConsent.apply(obj)` → aplikuje a uloží zadaný stav
- `window.mojraConsent.openSettings()` → otevře modál se stavem z `localStorage`

---

**Poznámka:** Pokud budete přidávat měření typu „enhanced conversions“ pro Ads, zapněte až po auditu (a jen pokud je skutečně používáte).