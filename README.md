# Portfolio — Jémima Egla

Site statique Jekyll, **HTML + CSS uniquement** (aucun JavaScript), publié avec GitHub Pages.

## Modifier le contenu

| Quoi | Où |
|---|---|
| Nom, e-mail, LinkedIn, texte « À propos », expertises, bandeau défilant | `_data/profile.yml` |
| La phrase d'accroche avec les mots en gras | `index.html` (lignes `{% include word.html … %}`) |
| Les projets | un fichier par projet dans `_projects/` |
| Les images des projets | `assets/projects/<nom-du-projet>/` |
| Couleurs, typos | variables en haut de `assets/css/main.css` |

Un champ laissé vide (`""` ou `[]`) n'est **pas affiché** : on peut publier maintenant et compléter plus tard.

## Ajouter un projet

1. Copier un fichier de `_projects/` (ex. `semaine-sherq.md`) et le renommer, ex. `mon-projet.md`.
   Le nom du fichier devient l'adresse : `/projets/mon-projet/`.
2. Modifier l'en-tête (entre les `---`) :
   ```yaml
   title: Mon projet
   order: 6                 # position dans la liste (1 = premier)
   client: Nom du client
   year: 2025
   tagline: Une phrase qui résume le projet.
   role: Mon rôle
   expertises: [Événementiel, Brand content]
   tone: accent             # couleur du bloc si pas d'image : accent | ink | sand
   cover: /assets/projects/mon-projet/cover.jpg
   cover_alt: Description de l'image de couverture
   gallery:
     - { src: /assets/projects/mon-projet/01.jpg, alt: "Description" }
     - { src: /assets/projects/mon-projet/02.jpg, alt: "Description", caption: "Légende", wide: true }
   links:
     - { label: "Voir sur Instagram", url: "https://…" }
   ```
3. Écrire le texte en dessous en Markdown (`## Titre`, paragraphes, listes `- …`).

## Glisser une image ou une vidéo entre deux paragraphes

Dans le texte d'un projet (sous les `---`), laisser une ligne vide avant et après :

```liquid
{% include media.html src="/assets/projects/mon-projet/photo.jpg" alt="Description" caption="Légende" %}

{% include media.html src="/assets/projects/mon-projet/film.mp4" caption="Légende" %}             ← avec boutons play/son
{% include media.html src="/assets/projects/mon-projet/film.mp4" loop=true %}                    ← auto, en boucle, muet
{% include media.html youtube="ID_DE_LA_VIDEO" caption="Légende" %}                              ← YouTube
```

`caption` est facultatif. Exemple complet dans `_projects/boya-food-edzrom.md`.
Vidéos : MP4 (H.264), idéalement < 10 Mo (GitHub refuse les fichiers > 100 Mo) ; au-delà, passer par YouTube.

## Images

- Créer le dossier `assets/projects/<nom-du-projet>/` et y mettre `cover.jpg`, `01.jpg`, `02.jpg`…
- Format conseillé : **JPG ou WebP, 1600 px de large, < 400 Ko** (compresser sur squoosh.app).
- Couverture : plutôt verticale ou 4:3, le sujet au centre (elle est recadrée automatiquement).
- Toujours remplir `alt` : une courte description de l'image.

## Voir le site en local

Avec Ruby :
```bash
bundle install
bundle exec jekyll serve
# → http://localhost:4000/portfolio/
```
Ou avec Docker, sans rien installer :
```bash
docker run --rm -p 4000:4000 -v "$PWD":/srv/jekyll -w /srv/jekyll ruby:3.3 \
  sh -c "bundle install && bundle exec jekyll serve --host 0.0.0.0"
```

## Publier (prod & staging)

Le workflow `.github/workflows/pages.yml` publie automatiquement à chaque push :

| Branche | Rôle | URL |
|---|---|---|
| `develop` | staging (non indexé, badge « Staging ») | https://josias-koffi.github.io/portfolio/staging/ |
| `main` | prod | https://josias-koffi.github.io/portfolio/ |

Méthode : travailler sur `develop`, vérifier sur l'URL de staging, puis fusionner `develop` dans `main` pour mettre en prod.

## Effets (tous en CSS)

- Nom qui monte ligne par ligne au chargement.
- Mots en gras qui font apparaître une image ou une note au survol.
- Bandeau défilant bordeaux.
- Liste de projets : titre qui glisse en italique et couverture qui apparaît au survol.
- Transition « morph » de la couverture entre l'accueil et la page projet (Chrome, Edge, Safari).
- Apparition des blocs au scroll (Chrome, Edge, Safari ; ailleurs le contenu s'affiche normalement).
- Tout est désactivé si le visiteur a demandé à réduire les animations.
