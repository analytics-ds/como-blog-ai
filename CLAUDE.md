# Hugo Site Factory

Ce repo est un template pour creer des sites blogs statiques avec Hugo, optimises SEO/GEO, heberges gratuitement sur GitHub Pages.

## Comment ca marche

Ce repo ne contient pas de site. Il contient les **instructions et templates** pour que Claude Code genere un site complet automatiquement.

### Premier lancement

1. L'utilisateur connecte Claude Code a ce repo
2. L'utilisateur tape `/create-site`
3. Claude pose les questions necessaires (nom du site, couleurs, categories, etc.)
4. Claude genere tout le site Hugo, les fichiers SEO, et configure le deploiement
5. L'utilisateur push sur GitHub, active GitHub Pages, le site est en ligne

### Utilisation courante

- `/create-article` : creer un nouvel article de blog (choix parmi plusieurs types : article standard, comparatif)
- `/seo-setup` : generer ou mettre a jour les fichiers SEO techniques de base (robots.txt, llms.txt, sitemap, structured data)
- `/seo` : mode interactif pour modifier/ajouter des elements SEO (meta tags, JSON-LD, audit on-page, etc.)
- `/serve` : lancer le serveur Hugo en local (previsualisation sur `http://localhost:1313/`)
- `/share` : lancer Hugo + ngrok pour partager le site via un lien public (accessible par n'importe qui)

## Structure du repo

```
.claude/
├── skills/
│   ├── create-site.md           ← Workflow creation de site complet
│   ├── create-article.md        ← Workflow creation d'article (multi-types)
│   ├── seo-setup.md             ← Workflow fichiers SEO techniques (baseline)
│   ├── seo.md                   ← Mode interactif SEO (modifications ponctuelles)
│   ├── serve.md                 ← Lancer le serveur Hugo en local
│   └── share.md                 ← Lancer Hugo + ngrok (partage public)
└── templates/
    ├── hugo-workflow.yml         ← GitHub Actions CI/CD
    ├── main.css                  ← CSS avec variables de charte graphique
    ├── articles/                 ← Templates d'articles par type
    │   ├── article-standard.md   ← Article informatif SEO + GEO (type par defaut)
    │   └── geo-comparatif.md     ← Article comparatif avec mise en avant
    ├── seo/                      ← Fichiers SEO techniques (editables)
    │   ├── robots.txt            ← Modele robots.txt
    │   ├── llms.txt              ← Modele llms.txt
    │   └── structured-data/      ← Schemas JSON-LD
    │       ├── article.json      ← BlogPosting
    │       ├── organization.json ← Organization
    │       ├── author.json       ← Person (auteur)
    │       ├── breadcrumb.json   ← BreadcrumbList
    │       ├── website.json      ← WebSite
    │       └── faq.json          ← FAQPage (a integrer manuellement)
    ├── layouts/
    │   ├── baseof.html           ← Layout de base
    │   ├── home.html             ← Page d'accueil
    │   ├── list.html             ← Pages de liste
    │   ├── single.html           ← Page article (avec affichage auteur)
    │   └── sitemap-html.html    ← Page plan du site (liste toutes les pages)
    └── partials/
        ├── header.html           ← Header/navigation
        ├── footer.html           ← Footer
        └── seo-head.html         ← Meta tags SEO + JSON-LD (OG, Twitter, canonical, schemas)
```

## Contexte du site

> Cette section est remplie automatiquement par le skill `/create-site`.
> Elle permet a Claude de connaitre le contexte du site pour les futures actions.

- **Nom du site** : Premium Drive Magazine
- **Description** : Le magazine de reference pour bien acheter, financer et entretenir sa Mercedes-Benz
- **URL** : https://magazine.como.fr/
- **Couleurs** : primary #0D0D0D (noir), accent #C0C0C0 (argent), cta #00ADEF (bleu Mercedes), background #F5F5F5 (gris clair)
- **Polices** : Playfair Display (titres), Source Serif 4 (corps), Inter (UI)
- **Categories** : Modeles et comparatifs, Financement, Electrique, Occasion, Concessionnaires, Conseils pratiques
- **Langue** : Francais (fr) + Anglais (en) dans /en/
- **Auteur** : La redaction Premium Drive
- **URL auteur** : https://magazine.como.fr/
- **Fonction auteur** : Redaction automobile

## Regles generales

- Toujours utiliser `relURL` dans les templates Hugo pour les liens (compatibilite GitHub Pages)
- Les articles vont dans `content/blog/`
- Les slugs sont en minuscules, sans accents, mots separes par des tirets
- Ne JAMAIS utiliser `&` dans les noms de categories ou de tags — toujours remplacer par "et" (Hugo genere un double tiret `--` dans le slug, ce qui casse les URLs)
- Le ton des articles est impersonnel (pas de je/tu/nous/vous) sauf instruction contraire
- Les specs d'article (mots minimum, H2, blocs obligatoires) dependent du type choisi — lire les `<!-- NOTES POUR CLAUDE -->` dans chaque template d'article
- Chaque article doit contenir au minimum 3 liens internes contextuels vers d'autres articles du blog. L'ancre de chaque lien doit contenir le mot-cle principal de l'article cible
- L'auteur est ajoute automatiquement dans le frontmatter et affiche sur la page (configure dans `hugo.toml [params]`)
- Les templates SEO dans `.claude/templates/seo/` sont editables par l'utilisateur — toujours lire la version en place avant de generer
- Pour ajouter un nouveau type d'article, creer un `.md` dans `.claude/templates/articles/` — il sera automatiquement propose par `/create-article`
- Pour ajouter un schema JSON-LD, creer un `.json` dans `.claude/templates/seo/structured-data/` et utiliser `/seo` pour l'integrer
- Chaque article doit avoir un champ `lastmod` dans le frontmatter (= date de derniere modification). Il est utilise par le sitemap XML, le sitemap HTML et le schema JSON-LD
- Quand un article est modifie, toujours mettre a jour le champ `lastmod` avec la date du jour
- Le sitemap HTML (`/plan-du-site/`) se regenere automatiquement a chaque build Hugo
- Toujours build et verifier (`hugo`) avant de commit

## Comment repondre a l'utilisateur

- Tutoiement, ton decontracte
- Pas de jargon technique sans explication
- Reponses structurees avec listes a puces
- Pas d'emoji sauf demande explicite


## Regle importante : visibilite d'un nouvel article

**A chaque fois qu'un article est mis en ligne, il DOIT etre visible :**

1. **Dans le sitemap.xml** — Hugo l'inclut automatiquement via le layout `themes/<theme>/layouts/sitemap.xml`. Verifier apres chaque build :
   - `https://<domaine>/sitemap.xml` → sitemapindex (FR + EN si multilingue)
   - `https://<domaine>/fr/sitemap.xml` → liste FR des URLs
   - `https://<domaine>/en/sitemap.xml` → liste EN (si site multilingue)

2. **Dans le plan de site HTML** — page `/plan-du-site/` (FR) et `/en/site-map/` (EN). Rendue via `layouts/_default/sitemap-html.html` qui liste toutes les sections (Blog, Categories, Auteurs). Verifier que l'article apparait dans la section "Blog".

3. **Sur la page auteur** — `/authors/<slug-auteur>/` (ex: `/authors/thomas-durand/`). La page liste automatiquement tous les articles dont le frontmatter contient `author: <slug>`. Verifier que le slug de l'auteur dans le frontmatter correspond a un auteur defini dans `data/authors.yaml`.

4. **Dans la page liste du blog** — `/blog/` liste les articles par date decroissante. Hugo l'inclut automatiquement si le fichier est dans `content/blog/` (FR) ou `content/en/blog/` (EN).

5. **Dans le JSON-LD** — l'article genere automatiquement son schema `Article` via `seo-head.html` (date, auteur, headline, etc.).

**Workflow de verification post-publication :**

```bash
# 1. Build Hugo
hugo

# 2. Verifier que l'article est dans le sitemap
grep "<nouveau-slug>" public/sitemap.xml public/fr/sitemap.xml

# 3. Verifier que le plan de site HTML le contient
grep "<nouveau-slug>" public/plan-du-site/index.html

# 4. Verifier que la page auteur le liste
grep "<titre>" public/authors/<slug-auteur>/index.html

# 5. Commit + push
git add -A && git commit -m "Article : <titre>" && git push origin main
```

Tout article nouvellement cree via `/create-article` doit etre verifie sur ces 5 emplacements avant la fin de la session.

