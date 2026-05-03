# Prompt Hebdo — site web

Site statique de la newsletter Prompt Hebdo : **https://www.prompthebdo.fr**

Construit avec [Astro](https://astro.build), déployé sur **Cloudflare Pages**.

## Stack

- **Astro 6** (output statique)
- `@astrojs/sitemap` — génération automatique du sitemap
- Beehiiv pour le formulaire newsletter
- Gumroad pour les produits téléchargeables
- Hosting + DNS : Cloudflare Pages

## Structure

```
github/
├── astro.config.mjs        # Config Astro + sitemap
├── package.json
├── public/                 # Assets servis tels quels
│   ├── _headers            # Headers Cloudflare (HSTS, X-Frame, cache, etc.)
│   ├── robots.txt          # Référence le sitemap
│   ├── og-image.png        # Image Open Graph (1200x630)
│   └── favicon.svg
└── src/
    ├── layouts/
    │   └── BaseLayout.astro    # Nav, footer, meta tags (OG/Twitter/canonical)
    └── pages/
        ├── index.astro         # Landing page
        └── blog/
            ├── index.astro
            ├── 5-outils-ia-gratuits.astro
            ├── guide-prompting-7-regles.astro
            └── automatiser-veille-ia.astro
```

## Développement local

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # génère /dist
npm run preview  # preview du build
```

## Déploiement

Push sur la branche connectée à Cloudflare Pages → rebuild automatique.

Voir [`GUIDE_DEPLOIEMENT.md`](./GUIDE_DEPLOIEMENT.md) pour la première mise
en place (Cloudflare + DNS + custom domain) et [`CHANGEMENTS_INFRA.md`](./CHANGEMENTS_INFRA.md)
pour les corrections d'infrastructure les plus récentes.

## Ajouter un article

1. Copier un fichier existant dans `src/pages/blog/` (ex. `5-outils-ia-gratuits.astro`)
2. L'ajouter au tableau `articles` dans `src/pages/blog/index.astro`
3. Commit + push → Cloudflare rebuild
