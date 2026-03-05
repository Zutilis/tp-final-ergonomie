# Loafly — Reproduction d'une landing page

## Présentation du projet

Ce projet est la reproduction d'une landing page de boulangerie artisanale appelée **Loafly**, dont le design original vient de Dribbble. Le but était de reproduire fidèlement le design : la mise en page, les espacements, l'organisation visuelle et le comportement sur mobile et desktop, en utilisant Vite et Tailwind CSS v4.

### Structure générale

La page est composée des sections suivantes dans l'ordre :

- **Header fixe** — logo, navigation desktop, panier, menu burger mobile
- **Hero** — titre principal plein écran avec un bouton d'appel à l'action
- **Categories** — onglets de catégories scrollables + 3 cartes produit
- **Testimonials** — carrousel de témoignages clients
- **Team** — présentation des boulangers en cartes rondes
- **Gallery** — mosaïque de photos en grille asymétrique
- **Booking** — formulaire de réservation + horaires d'ouverture
- **Blog** — 3 articles récents en cartes avec image de fond
- **Footer** — newsletter, navigation, coordonnées

-------

## Application des concepts d'ergonomie

### Hiérarchie visuelle

Les titres de section sont grands (jusqu'à `text-6xl` sur desktop), en majuscules avec la police `Lilita One`. Les prix et sous-titres sont dans des tailles intermédiaires. Le texte courant est en petit (`text-sm` ou `text-xs`) pour ne pas concurrencer les éléments importants. La couleur orange `#faa941` attire l'œil sur les éléments clés (prix, badges, noms), et le noir et le marron structurent les blocs.

### Réduction de la charge cognitive

Chaque section ne traite qu'un seul sujet à la fois. Le nombre d'éléments par bloc est limité volontairement : 3 cartes produit, 4 membres d'équipe, 3 articles de blog. Les badges `badge-sticker` au-dessus de chaque titre permettent à l'utilisateur de comprendre de quoi parle la section d'un coup d'œil, sans avoir à tout lire.

### Pattern de lecture

La page suit un **pattern Z** : sur le hero, l'œil part du titre en haut, traverse vers la droite, puis descend vers le bouton centré. Dans les sections avec plusieurs cartes, on retrouve un **pattern F** : l'utilisateur lit la première ligne de gauche à droite, puis descend en scannant le côté gauche.

### Mise en avant des CTA

Les boutons principaux (composant `button`) ont un fond orange qui contraste fort avec tous les fonds de la page. Sur la carte Baguettes qui a déjà un fond orange, le bouton bascule en `button-black` pour garder ce contraste. Sur le hero, le bouton "Explore Our Items" est seul sous le titre, bien espacé, sans élément concurrent autour.

### Heuristiques de Nielsen

- **Visibilité de l'état du système** : le lien actif dans la navigation est en orange (`aria-current="page"`), on sait où on est.
- **Correspondance avec le monde réel** : les textes sont simples et directs ("Add to Cart", "Book your table", "Opening Hours").
- **Contrôle utilisateur** : le menu mobile se ferme via le bouton ✕ ou en cliquant sur l'overlay sombre — deux façons de sortir.
- **Cohérence** : les composants réutilisables (`button`, `badge-sticker`, `separator`) ont le même look partout sur la page.
- **Prévention des erreurs** : les champs du formulaire utilisent les types HTML natifs (`type="email"`, `type="date"`) et `required`, ce qui empêche les soumissions invalides.
- **Reconnaissance plutôt que rappel** : chaque onglet de catégorie affiche une image au-dessus de son label — pas besoin de mémoriser ce que représente chaque catégorie.
- **Flexibilité** : la navigation est disponible en haut de page et dans le footer, pratique pour les utilisateurs en bas de page.
- **Esthétique et minimalisme** : chaque section ne contient que l'essentiel. Le témoignage par exemple n'affiche qu'une citation, un nom et une photo.

---

## Accessibilité

### Respect des critères WCAG AA

Les principales associations couleur/fond ont été vérifiées : texte blanc sur marron `#7b4125`, texte noir sur orange `#faa941`, texte orange sur noir `#1c1311` — tous au-dessus du ratio minimal de 4.5:1. Les seuls textes un peu moins contrastés (`text-white/50`, `text-white/60`) sont uniquement utilisés pour des informations secondaires non essentielles comme les crédits dans le footer.

### Gestion du focus

Tous les éléments cliquables (boutons, liens, inputs, select) ont un `focus-visible:ring-2` visible. Le menu mobile fonctionne sans JavaScript grâce au checkbox hack CSS, ce qui garde la navigation au clavier fonctionnelle.

### Choix sémantiques HTML

La structure utilise les bonnes balises partout :

- `<header>` pour la navigation fixe en haut
- `<main>` qui englobe tout le contenu de la page
- `<section>` avec `aria-labelledby` relié au `<h2>` de chaque section
- `<article>` pour les cartes produit, les membres d'équipe et les articles de blog
- `<nav>` avec un `aria-label` différent pour chaque zone de navigation
- `<blockquote>` avec `<footer>` et `<cite>` pour le témoignage
- `<address>` pour les coordonnées dans le footer
- `<dl>`, `<dt>`, `<dd>` pour les horaires d'ouverture
- `<form>` avec `aria-label` pour les formulaires

### Alternatives textuelles

Les images décoratives (icônes de catégorie) ont `alt=""` et `aria-hidden="true"`. Les images de contenu ont des `alt` descriptifs. Les éléments purement visuels comme les guillemets ou les chevrons de navigation sont masqués aux lecteurs d'écran avec `aria-hidden="true"`.

-------

## Choix techniques Tailwind CSS v4

### Les tokens `@theme`

Toutes les couleurs et polices sont définies dans `@theme` pour éviter de mettre des valeurs en dur dans le HTML :

@theme {
  --color-black: #1c1311;
  --color-brown: #7b4125;
  --color-light-orange: #f8edd0;
  --color-orange: #faa941;
  --color-white: #ffffff;

  --font-google: 'Google Sans Flex', sans-serif;
  --font-lilita: 'Lilita One', cursive;
}

Le noir est un noir chaud (`#1c1311`) plutôt que pur, ce qui colle mieux à l'ambiance chaleureuse du design. Les deux polices correspondent aux deux usages de la page : `Lilita One` pour les titres expressifs, `Google Sans Flex` pour le texte courant.

### Les utilitaires `@utility`

Six utilitaires ont été créés pour éviter de répéter les mêmes classes partout :

- **`button`** — le bouton orange arrondi utilisé sur tous les CTA principaux
- **`button-black`** — variante noire du bouton, hérite de `button`
- **`badge-sticker`** — l'étiquette orange avec Lilita One, pour les labels de catégorie
- **`badge-sticker-black`** — variante noire du badge pour les onglets inactifs
- **`title-badge-sticker`** — badge centré en absolu au-dessus des titres de section
- **`separator`** / **`separator-black`** — ligne fine de séparation dans les cartes
- **`gallery-mosaic`** — définit la grille mosaïque asymétrique sur desktop

### Structure des sections

Chaque section suit le même schéma : `max-w-5xl mx-auto` pour limiter la largeur, `px-6 md:px-20` pour les marges latérales, `py-12 md:py-16` pour l'espacement vertical. Les grilles passent d'1 colonne sur mobile à leur layout final via les préfixes `sm:` ou `md:`.

### Gestion du responsive

Tout est pensé mobile-first. Les deux breakpoints principaux sont `sm` (640px) pour les ajustements intermédiaires et `md` (768px) pour les changements de layout majeurs. La galerie utilise `grid-flow-dense` pour combler les trous sur mobile, tout en gardant la mosaïque `gallery-mosaic` sur desktop.

-------

## Difficultés rencontrées

### Trous dans la galerie sur mobile

Sur desktop, la galerie fonctionne bien avec sa grille sur mesure. Sur mobile en `grid-cols-2`, les éléments larges (`col-span-2`, `col-span-3`) créaient des trous car le navigateur ne cherche pas à les combler tout seul. La solution : ajouter `grid-flow-dense`, qui dit au navigateur de remplir automatiquement les cases vides avec les éléments suivants qui peuvent y rentrer.

### Menu burger CSS-only

Le menu mobile fonctionne sans JavaScript grâce au checkbox hack (`peer` + `peer-checked` de Tailwind). La contrainte : l'`<input>` doit être placé avant tous les éléments qu'il contrôle dans le DOM pour que le sélecteur CSS fonctionne. Il a donc fallu le mettre tout en haut du `<body>`, avant même le `<header>`.

### Badges flottants dans le footer

Les badges "Tips", "Stories", "Offers" sont positionnés en absolu à l'intérieur du `<h2>` du footer. Trouver le bon positionnement (`right-[15%]`, `right-[75%]`, `left-1/2 -translate-x-1/2`) pour qu'ils collent au design sans déborder sur mobile a demandé pas mal d'ajustements.