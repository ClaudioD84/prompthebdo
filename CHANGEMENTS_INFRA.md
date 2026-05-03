# Corrections infra appliquées le 2026-05-03

Tous les points relevés lors de l'audit du site live ont été corrigés en local.
Il reste à pousser sur la branche connectée à Cloudflare Pages pour déclencher
le déploiement.

## Ce qui a changé

### 1. Liens produits Gumroad cassés en prod
**Cause** : la version déployée sur Cloudflare est antérieure à l'ajout des
liens Gumroad. Le code local (`src/pages/index.astro`) et le build local
(`dist/index.html`) contiennent déjà les bons liens (`dellutri.gumroad.com/l/sdfjrz`,
`/lkzjp`, `/uzsbp`). **Le simple fait de redéployer suffit à corriger.**

### 2. Tags Open Graph + canonical
Fichier modifié : `src/layouts/BaseLayout.astro`

Ajout de `og:title`, `og:description`, `og:url`, `og:image`, `og:type`,
`og:site_name`, `og:locale`, `twitter:card`, `twitter:title`, `twitter:description`,
`twitter:image` et `<link rel="canonical">`. Tous calculés dynamiquement par page
à partir de `Astro.url`.

Image OG créée : `public/og-image.png` (1200x630, branding Prompt Hebdo).
Tu peux la remplacer plus tard par ta propre version.

### 3. Sitemap XML
- Package ajouté : `@astrojs/sitemap`
- Config : `astro.config.mjs` (intégration sitemap activée, `trailingSlash: 'ignore'`)
- Génère automatiquement `sitemap-index.xml` + `sitemap-0.xml` au build

### 4. Headers de sécurité Cloudflare
Fichier créé : `public/_headers`

Headers appliqués sur tout le site :
- `Strict-Transport-Security` (HSTS, 2 ans, preload-ready)
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy` (caméra/micro/géoloc bloqués)
- `X-XSS-Protection`

Cache-control optimisé :
- Assets `/_astro/*` : 1 an, immutable
- Favicon, OG image : revalidation périodique

### 5. robots.txt
Fichier créé : `public/robots.txt`

Allow `/` + référence le sitemap.
**Attention** : ce fichier remplace le robots.txt auto-généré par Cloudflare
(qui contenait les Content-Signals anti-scraping IA). Si tu veux les garder,
réactive l'option dans Cloudflare → Bots / AI Audit, ou ajoute manuellement
les directives `Content-Signal:` dans le fichier.

## Ce qu'il reste à faire (par toi)

### Pousser le code
Ton dossier local n'est pas un repo Git. Si Cloudflare Pages est connecté à
un repo GitHub que tu maintiens ailleurs, il faut synchroniser ce dossier
avec ce repo, puis :
```
git add astro.config.mjs package.json package-lock.json src/layouts/BaseLayout.astro public/
git commit -m "Infra: OG tags, sitemap, security headers, robots.txt"
git push
```
Cloudflare Pages rebuildera automatiquement en quelques minutes.

### Activer HSTS preload (optionnel mais recommandé)
Dans Cloudflare Dashboard → SSL/TLS → Edge Certificates → HTTP Strict
Transport Security (HSTS) → Enable. Ça double la protection au cas où
le `_headers` ne serait pas appliqué.

### Soumettre le sitemap à Google
Une fois en ligne, dans Google Search Console :
Sitemaps → ajouter `https://www.prompthebdo.fr/sitemap-index.xml`

## Vérification après déploiement

```bash
# Headers
curl -sI https://www.prompthebdo.fr/ | grep -iE 'strict|x-content|x-frame|referrer|permissions'

# Sitemap
curl -s https://www.prompthebdo.fr/sitemap-index.xml

# Robots
curl -s https://www.prompthebdo.fr/robots.txt

# OG tags (doit afficher og:title, og:image, etc.)
curl -s https://www.prompthebdo.fr/ | grep -E 'og:|twitter:|canonical'

# Test partage social (LinkedIn / Facebook)
# https://www.linkedin.com/post-inspector/
# https://developers.facebook.com/tools/debug/
```
