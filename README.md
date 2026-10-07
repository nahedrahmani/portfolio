# Portfolio, Nahed Rahmani

Site statique, une seule page. Pas de dépendance, pas d'étape de construction :
`index.html` plus le dossier `assets/`. Il s'ouvre tel quel dans un navigateur.

```
index.html          la page entière (structure, styles, script)
assets/             images WebP, vidéos MP4, image de partage
vercel.json         en-têtes de cache et de sécurité
```

## Voir le site en local

Ouvrir `index.html` par un double clic suffit. Pour être au plus près de la
production, servir le dossier :

```bash
python -m http.server 5173
# puis http://localhost:5173
```

## Déployer sur Vercel

### Option A, par le dépôt Git (recommandée)

Le déploiement se refait tout seul à chaque `git push`.

```bash
cd D:\portfolio-nahed
git init
git add .
git commit -m "portfolio: premiere version"
git branch -M main
git remote add origin https://github.com/nahedrahmani/<nom-du-depot>.git
git push -u origin main
```

Ensuite sur vercel.com : **Add New, Project**, importer le dépôt, et laisser
tous les réglages par défaut. Framework Preset doit indiquer **Other**, il n'y a
ni commande de build ni dossier de sortie à renseigner.

### Option B, en ligne de commande

```bash
npm i -g vercel
cd D:\portfolio-nahed
vercel            # premiere fois : connexion, puis questions de configuration
vercel --prod     # met en ligne sur l'URL de production
```

`vercel login` ouvre le navigateur : cette étape demande une vraie personne
devant l'écran, elle ne peut pas être automatisée.

## À faire après le premier déploiement

1. **Corriger l'URL du site.** Dans `index.html`, trois endroits portent encore
   `https://nahed-rahmani.vercel.app/` : la balise `canonical`, `og:url`, et il
   faut les remplacer par l'adresse réelle. Sans cela l'aperçu des liens partagés
   pointe vers le mauvais domaine.
2. **Vérifier l'aperçu de partage** en collant l'URL dans LinkedIn ou dans une
   conversation. L'image `assets/og.jpg` doit apparaître.
3. **Nom de domaine.** Vercel donne une adresse en `.vercel.app`. Un domaine
   personnel s'ajoute dans **Settings, Domains** du projet.

## Contenu à compléter

Repéré dans la page par un texte rouge souligné en pointillés :

- vérifier la date du certificat AWS : il porte 11/09/2025 sans préciser le
  format, lu ici comme le 9 novembre 2025 (format mois/jour, usuel chez AWS).

## Les médias

Les vidéos sont réencodées pour le web et ne doivent pas être remplacées par les
fichiers bruts, qui pèsent cent fois plus lourd.

```bash
# recette utilisée, à réutiliser pour toute nouvelle démonstration
ffmpeg -i source.mp4 -vf "scale=1280:-2" -c:v libx264 -crf 30 -preset slow \
       -pix_fmt yuv420p -an -movflags +faststart assets/nom-demo.mp4

# image d'affiche, prise à la cinquantieme seconde
ffmpeg -ss 50 -i assets/nom-demo.mp4 -frames:v 1 poster.png
```

`-an` retire la piste audio, les vidéos étant lues en sourdine.
`-movflags +faststart` permet la lecture avant la fin du téléchargement.
