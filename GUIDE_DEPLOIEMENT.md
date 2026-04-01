# Prompt Hebdo — Guide de déploiement

## Structure du site

Le site est construit avec **Astro** (framework statique ultra-rapide).

**Pages créées :**
- `/` — Landing page (hero, features, audience, produits, CTA)
- `/blog` — Liste des articles
- `/blog/5-outils-ia-gratuits` — Article : 5 outils IA gratuits
- `/blog/guide-prompting-7-regles` — Article : Guide du prompting
- `/blog/automatiser-veille-ia` — Article : Automatiser sa veille IA

---

## Option recommandée : Cloudflare Pages (gratuit)

### Étape 1 : Créer un compte Cloudflare
1. Va sur https://dash.cloudflare.com/sign-up
2. Crée un compte gratuit

### Étape 2 : Transférer ton domaine (ou configurer le DNS)
1. Dans Cloudflare, va dans **"Add a Site"** → entre `prompthebdo.fr`
2. Choisis le plan **Free**
3. Cloudflare te donnera 2 nameservers (ex: `ada.ns.cloudflare.com`)
4. Va chez ton registrar (là où tu as acheté le domaine) et remplace les nameservers par ceux de Cloudflare
5. Attends la propagation DNS (quelques minutes à 24h)

### Étape 3 : Créer un dépôt GitHub
1. Crée un compte GitHub si ce n'est pas déjà fait (https://github.com)
2. Crée un nouveau repository nommé `prompthebdo`
3. Pousse le code du dossier `prompthebdo/` vers ce repo :
```bash
cd prompthebdo
git init
git add .
git commit -m "Initial commit - Prompt Hebdo website"
git remote add origin https://github.com/TON_USERNAME/prompthebdo.git
git push -u origin main
```

### Étape 4 : Connecter à Cloudflare Pages
1. Dans Cloudflare Dashboard → **Workers & Pages** → **Create**
2. Choisis **"Connect to Git"**
3. Connecte ton compte GitHub et sélectionne le repo `prompthebdo`
4. Configuration du build :
   - **Framework preset** : Astro
   - **Build command** : `npm run build`
   - **Build output directory** : `dist`
5. Clique **"Save and Deploy"**

### Étape 5 : Configurer le domaine personnalisé
1. Une fois déployé, va dans les settings du projet Cloudflare Pages
2. **Custom domains** → **Set up a custom domain**
3. Entre `www.prompthebdo.fr` et `prompthebdo.fr`
4. Cloudflare configurera automatiquement les DNS

**Résultat : ton site est en ligne sur https://www.prompthebdo.fr** 🎉

---

## Alternative : Vercel (gratuit aussi)

1. Va sur https://vercel.com et crée un compte
2. Importe ton repo GitHub
3. Vercel détecte automatiquement Astro
4. Déploie en 1 clic
5. Configure le domaine dans Settings → Domains → ajoute `prompthebdo.fr`

---

## Prochaines étapes après le déploiement

### 1. Intégrer Beehiiv (newsletter)
- Crée un compte sur https://beehiiv.com (plan gratuit)
- Crée ta publication "Prompt Hebdo"
- Récupère le code d'embed du formulaire d'inscription
- Remplace le `<div class="signup-placeholder">` dans `src/pages/index.astro` par le code Beehiiv

### 2. Intégrer Gumroad (produits)
- Crée un compte sur https://gumroad.com
- Crée tes produits (Guide du Prompting gratuit, Pack 50 Prompts à 9€)
- Remplace les liens `href="#"` dans la section produits par tes liens Gumroad

### 3. Ajouter des articles
Pour ajouter un nouvel article :
1. Crée un fichier `.astro` dans `src/pages/blog/` (copie un article existant comme modèle)
2. Ajoute l'entrée dans le tableau `articles` de `src/pages/blog/index.astro`
3. Commit et push → Cloudflare redéploie automatiquement

---

## Commandes utiles

```bash
npm run dev      # Serveur de développement local (http://localhost:4321)
npm run build    # Génère le site statique dans /dist
npm run preview  # Prévisualise le build localement
```
