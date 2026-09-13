# Сайт Togethis (лендінг) — деплой і обслуговування

Підготовлено 2026-09-13 у межах плану [GEO.md](../GEO.md) (§3). Файли тут —
джерело правди; живе вони в окремому репозиторії `togethis-site`
(GitHub Pages, `https://kyiv0x7c.github.io/togethis-site/`), поруч із
`privacy.html` / `terms.html` / `support.html` з [../legal](../legal).

## Що в папці

| Файл | Що робить |
|---|---|
| `index.html` | лендінг українською: канонічний опис у першому абзаці, факти, 8 реальних питань стартової колоди, FAQ; JSON-LD `SoftwareApplication` + `FAQPage`; hreflang; OpenGraph |
| `en.html` | те саме англійською |
| `llms.txt` | паспорт сутності для AI-агентів (markdown) |
| `robots.txt` | дозвіл усім + AI-краулерам; **на github.io у підпапці не діє** (див. нижче) |
| `sitemap.xml` | 5 URL з hreflang |
| `img/` | скріншоти 276×600 (uk/en 01, 02, 04), `icon-512.png`, `apple-touch-icon.png` |

## 1. Деплой (10 хвилин, через вебінтерфейс GitHub)

1. Відкрити репозиторій `togethis-site` → **Add file → Upload files**.
2. Перетягнути з цієї папки: `index.html`, `en.html`, `llms.txt`,
   `robots.txt`, `sitemap.xml` і **папку `img`** (тягнути саму папку — GitHub
   збереже структуру). Не чіпати `privacy.html`, `terms.html`, `support.html`,
   якщо їх не оновлювали.
3. Commit changes → почекати 1–2 хвилини → перевірити:
   `https://kyiv0x7c.github.io/togethis-site/` (має відкритись лендінг, а не 404),
   `…/en.html`, `…/llms.txt`, `…/sitemap.xml`, `…/img/icon-512.png`.
4. Скинути в чат скріншот головної — я перевірю розмітку валідатором
   (`validator.schema.org`) і виправлю, якщо треба.

Якщо репозиторій є локально — те саме через `git add . && git commit && git push`.

## 2. Одразу після деплою (з GEO.md §3.2)

1. **ASC → App Information → Marketing URL** → `https://kyiv0x7c.github.io/togethis-site/`.
2. **Bing Webmaster Tools**: додати сайт → верифікація «HTML Meta Tag» →
   рядок `<meta name="msvalidate.01" …>` вставити в `index.html` **і** `en.html`
   на місце коментаря `<!-- BING-VERIFY -->` → передеплоїти → Verify →
   Sitemaps → подати `sitemap.xml` → IndexNow → згенерувати ключ → файл
   `<ключ>.txt` (вміст — сам ключ) покласти в корінь репо.
3. **Google Search Console**: URL prefix → «HTML tag» → рядок
   `<meta name="google-site-verification" …>` на місце `<!-- GOOGLE-VERIFY -->`
   в обох html → передеплоїти → Verify → Sitemaps → `sitemap.xml`.
4. Request indexing для `/` та `/en.html` в обох консолях.

## 3. Обмеження GitHub Pages у підпапці

- `robots.txt` читається тільки з кореня домену (`kyiv0x7c.github.io/robots.txt`),
  тобто наш файл ігнорується, доки нема власного домену. Це не блокує
  індексацію — GitHub дозволяє краулінг. Sitemap подаємо напряму в консолі.
- Canonical/hreflang/OG зашиті абсолютними URL на github.io — при переїзді
  замінити (нижче).

## 4. Переїзд на власний домен (коли буде)

1. Купити домен (перевірити `togethis.app` / `togethis.com`), DNS —
   Cloudflare (безкоштовно).
2. У репо `togethis-site` → Settings → Pages → Custom domain → ввести домен →
   Enforce HTTPS. Додати DNS-записи за підказкою GitHub (A/AAAA або CNAME).
3. Замінити базовий URL у файлах сайту однією командою (з папки `marketing/site`):
   ```bash
   LC_ALL=C sed -i '' 's#https://kyiv0x7c.github.io/togethis-site/#https://togethis.app/#g' index.html en.html llms.txt robots.txt sitemap.xml
   ```
   і те саме для `../legal/*.html`, якщо там є абсолютні посилання.
4. Оновити URL у `Togethis/Config/Config.xcconfig` (PRIVACY_POLICY_URL,
   TERMS_OF_SERVICE_URL) — потрапить у наступний реліз; в ASC — Privacy Policy
   URL, Support URL, Marketing URL; у BWT/GSC додати новий домен як окрему
   property; повторно подати sitemap.
5. Старі github.io-адреси GitHub сам редіректить на домен (301).

## 5. Правила оновлення контенту

- Кожна цифра на сторінці (кількість питань, ціни, склад колод) має збігатися
  з тим, що зараз у сторі. Змінили в ASC → змінити тут → оновити `lastmod`
  у `sitemap.xml` → пінг IndexNow.
- Не анонсувати колоди, яких ще нема в App Store (урок реджекту 5.6:
  розходження між обіцянками і реальністю руйнує довіру і в Apple, і в LLM).
- JSON-LD у `<head>` має описувати те саме, що видно на сторінці (FAQ у
  розмітці = FAQ у тексті).
