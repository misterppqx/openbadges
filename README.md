# openbadges
Site web destiné à faire découvrir les badges numériques et la reconnaissance ouverte

## Mille façons de reconnaître

Le dossier [`mille-facons/`](mille-facons/) contient un mini-site statique pour les jeunes de l'enseignement agricole : découvrir les mille façons de reconnaître et d'être reconnu, comprendre comment est construit un badge, puis suivre un parcours jusqu'à la fiche de son badge ou de son endossement.

- Un seul fichier `index.html` et les polices dans `polices/` (Nunito, Baloo 2, licence SIL OFL), sans appel à un service extérieur.
- Aucune donnée enregistrée, aucun cookie.
- Les pages internes utilisent des adresses en `#` (par exemple `#passeur`) : aucune configuration de serveur n'est nécessaire.

Publication avec GitHub Pages : *Settings > Pages > Build and deployment > Deploy from a branch*, branche `main`, dossier `/ (root)`. Le site est alors servi à l'adresse `https://misterppqx.github.io/openbadges/mille-facons/`.

## Pour les développeurs

Le site tient dans **un seul fichier HTML** (`mille-facons/index.html`, environ 260 Ko), avec son CSS et son JavaScript, sans framework, sans outil de construction ni serveur applicatif.

### Fichiers

```
openbadges/
├── index.html              redirection vers mille-facons/
├── README.md, LICENSE      présentation, licence CC BY-SA 4.0
├── .nojekyll               GitHub Pages sert les fichiers tels quels
└── mille-facons/
    ├── index.html          tout le site : HTML + CSS + JS
    ├── consigne-test.html  page autonome pour les testeurs
    └── polices/            Nunito et Baloo 2 en woff2 (SIL OFL)
```

### Architecture

- **Application d'une seule page.** Le HTML de départ ne contient que l'en-tête, `<main id="app">` et le pied de page. La fonction `afficher()` lit l'ancre de l'adresse (`#mille-facons`, `#construire-faire-stage`…) et y affiche la page demandée. Aucune configuration serveur n'est nécessaire ; le fichier fonctionne aussi ouvert en local.
- **Contenu rangé dans des objets JavaScript**, transformés en pages par des fonctions :
  - `VUES` : les pages fixes (Temps 1 et 2, adultes, à propos), une fonction par page ;
  - `PARCOURS` : les 18 parcours du Temps 3, affichés par `vueParcours()` ;
  - `BUILD` et `TYPE_BUILD` : les formulaires de création de badge et l'aperçu de la fiche (`vueConstruire()`, `ficheHTML()`) ;
  - `EX` et `PUBLIC` : les exemples adaptés aux trois publics (`college`, `bac`, `bts`), avec repli sur les exemples par défaut via `ex()` ;
  - `ETAPES` et `T` : l'ordre des pages de chaque temps et la barre de progression.
- **Gestion des clics centralisée** : la plupart des interactions sont écoutées au niveau du document et reconnues par des attributs `data-*`.
- **État en mémoire seulement** : les réponses écrites sont gardées dans `REFLEXIONS` le temps de la visite. Aucun cookie, aucun `localStorage`, sauf en mode test.
- **Téléchargements générés dans le navigateur** (fiches .txt, carnet, retours .csv), sans serveur.

### Mode test

Avec `?test` dans l'adresse (`…/mille-facons/?test`), un panneau permet à un adulte testeur de donner un avis par page, de signaler des passages en les touchant et de commenter. Les retours sont gardés dans le `localStorage` du navigateur (clé `mille-facons-retours`) et exportés en CSV (séparateur point-virgule, UTF-8 avec BOM), importable dans Grist ou un tableur. Sans `?test`, rien de tout cela n'est chargé.

### Appels extérieurs

- Polices hébergées avec le site, images intégrées au fichier.
- Seule la vidéo YouTube appelle un service extérieur : elle ne se charge qu'au clic, depuis `youtube-nocookie.com`.
- Les autres adresses extérieures sont de simples liens.

### Accessibilité et affichage

- `aria-pressed` sur les boutons d'état, `aria-live` sur les zones de réponse, focus placé sur le contenu à chaque changement de page.
- Respect du réglage « animations réduites » (`prefers-reduced-motion`).
- Affichage adapté sous 860 px ; feuille d'impression pour la fiche « Animer un atelier ».
- Code couleur : vert pour le texte et la structure, jaune pour les choix et les saisies de l'apprenant, rouge pour la navigation.

### Ajouter du contenu

- **Une page fixe** : ajouter une fonction dans `VUES`, puis l'inscrire dans `ETAPES` (et `T`) pour qu'elle apparaisse dans la progression.
- **Un parcours** : ajouter une entrée dans `PARCOURS` et, s'il mène à un badge, l'associer à un type dans `TYPE_BUILD`.
- **Un exemple adapté à un public** : ajouter la clé dans `EX.college`, `EX.bac` ou `EX.bts`.

### Limites connues

- Contenu et code sont dans le même fichier : modifier un texte demande de le chercher dans le JavaScript. Sortir les textes dans un fichier de données séparé faciliterait les relectures.
- Pas de tests automatisés dans le dépôt. Les modifications ont été vérifiées avec des scripts Playwright (toutes les adresses, liens internes, erreurs JavaScript), à ajouter au besoin.
- Contenu produit par le JavaScript, donc peu visible des moteurs de recherche ; sans conséquence pour un support d'atelier.
- Un appel à `claude.use("downloads")`, hérité de la première version publiée sur claude.ai, est sans effet sur GitHub Pages : les téléchargements passent alors par un `Blob`.


## Crédits

- **Conception et contenus** : Philippe Petitqueux
- **Cadre** : réalisé dans le cadre d'[acoustice.educagri.fr](https://acoustice.educagri.fr/), un dispositif de l'enseignement agricole (MAASA)
- **Soutenu par** : Reconnaître – Open Recognition Alliance
- **Remerciements** : aux enseignants, formateurs et professionnels qui ont partagé leurs conseils
- **Développement assisté par IA** : Claude (Anthropic)
- **Licence** : [Creative Commons BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.fr) (texte complet dans [`LICENSE`](LICENSE)). Vous pouvez reprendre et adapter ce site, en citant la source et en partageant sous la même licence. Exceptions : les polices de `mille-facons/polices/` (licence SIL OFL) et l'image du badge « Je suis écoresponsable », qui appartient à l'enseignement agricole.

Pour citer : *Mille façons de reconnaître*, Philippe Petitqueux, acoustice.educagri.fr, CC BY-SA 4.0, https://misterppqx.github.io/openbadges/mille-facons/
