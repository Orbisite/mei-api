# API starter (JSON + images)

Fichiers de référence pour un dépôt **statique** (comme [`api/`](../../api)) consommé par le site modèle.

## Fichiers

| Fichier | Rôle |
|---------|------|
| `content.json` | Textes, titres, liens, images par section (feuilles `{ fr, en }` pour le bilingue). Optionnel : clé **`pages`** pour le multi-pages (voir ci-dessous). |
| `theme.json` | Couleurs par variante `primary` / `secondary` / `neutral` : champs **obligatoires** (`surface`, `accent`, badges, boutons, `ring`, …) + à la racine optionnel **`contentScheme`** : `"light"` ou `"dark"` (navbar / chrome du site). Si absent : déduit de la luminance de `primary.surface`. + champs **optionnels** pour `@orbisite/blocks` ≥ 0.2.3 (texte et cartes). |
| `site.json` | Titre, description, meta Open Graph / Twitter, favicon (noms de fichiers relatifs à `img/` ou URLs absolues). |
| `img/` | Assets ; les URLs côté site = `VITE_API_IMG_BASE` + nom de fichier. |

## Multi-pages (`pages` dans `content.json`)

Sans **`pages`** : le site reste une **single page** (menu en `#section`).

Avec **`pages`**, chaque clé est un chemin d’URL (ex. `"/contact"`). La valeur est un objet qui peut contenir :

| Champ | Rôle |
|--------|------|
| **`sections`** | Tableau optionnel : ordre des blocs à afficher, parmi `navbar`, `hero`, `features`, `bento`, `pricing`, `testimonials`, `footer`. |
| *autres clés* | Surcharges fusionnées sur le JSON racine pour cette route uniquement (ex. un `hero` différent sur `/contact`). |

Exemple minimal :

```json
"navbar": {
  "links": [
    { "label": { "fr": "Accueil", "en": "Home" }, "href": "/" },
    { "label": { "fr": "Contact", "en": "Contact" }, "href": "/contact" }
  ]
},
"pages": {
  "/contact": {
    "sections": ["navbar", "hero", "footer"],
    "hero": {
      "title": { "fr": "Contact", "en": "Contact" },
      "subtitle": { "fr": "Écrivez-nous.", "en": "Write to us." },
      "cta": { "fr": "Tarifs", "en": "Pricing" },
      "ctaHref": "/#pricing"
    }
  }
}
```

L’accueil **`/`** utilise le JSON racine (tous les blocs par défaut) si tu ne définis pas `pages["/"]`. Toute URL absente de **`pages`** (hors `/`) affiche une page 404 côté app.

## `theme.json` — typographie et cartes (optionnel)

Ces clés peuvent être **omises** : le site applique alors des défauts adaptés à un fond **sombre**. Pour un site **fond clair** (`contentScheme="light"`), définis-les pour garder un bon contraste (voir le dépôt `leo-giraud-api` comme exemple).

| Préfixe | Rôle |
|--------|------|
| **`contentEmphasis`** … **`contentSoft`**, **`contentBorder`** | Texte sur la surface de section (`--p-surface`) : titres, corps, secondaire, bordures de séparation. |
| **`onDarkEmphasis`** … **`onDarkSoft`** | Texte à l’intérieur des cartes ; si omis, recopie des `content*`. Utile si la carte est plus sombre que la section. |
| **`elevatedBg`**, **`elevatedBorder`**, **`elevatedBgSoft`**, **`elevatedBgMid`**, **`elevatedBgFlat`**, **`elevatedBgInset`**, **`elevatedBgNotice`**, **`elevatedBgBand`** | Fonds et bordures des blocs type carte, galerie, CTA, FAQ (la bande peut être un `linear-gradient(...)`). |
| **`elevatedShadow`** … **`elevatedShadow2xl`** | Ombres portées (valeur CSS complète pour `box-shadow`). |

## Images

Dans `content.json` et `site.json`, les références d’images peuvent être une **URL absolue** (`https://...`) ou un **nom de fichier** présent dans `img/`.

## Publication

Héberger sur **GitHub** (branche `main`) : les URLs raw sont du type  
`https://raw.githubusercontent.com/ORG/REPO/main/content.json`  
et pour une image `img/photo.png` :  
`https://raw.githubusercontent.com/ORG/REPO/main/img/photo.png`.

Copier le contenu de ce dossier dans votre repo API, remplacer les textes et images, puis pointer `model/site/.env.local` vers ces URLs (y compris `VITE_SITE_URL` pour `site.json`).
