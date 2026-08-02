# SCPP — HSE Control

Application de gestion des risques et anomalies HSE pour la SCPP (échafaudage, peinture,
sablage, travaux sur corde, maintenance navale, mécanique, électricité, soudure, engins &
circulation, manutention, bureau/ergonomie).

C'est une **PWA** (Progressive Web App) : elle s'installe comme une app sur téléphone/PC
et peut aussi être publiée comme véritable application Android.

## Contenu du dossier

```
index.html          → l'application (page unique)
manifest.json        → métadonnées de l'app (nom, icônes, couleurs)
sw.js                 → service worker (fonctionnement hors-ligne basique)
icons/                → icônes de l'app (192, 512, maskable, apple-touch, favicon)
```

## ⚠️ Important : stockage des données

Les données (anomalies, rondes, permis) sont enregistrées avec `localStorage`,
**dans le navigateur de chaque appareil**. Cela veut dire :
- Les données ne sont **pas partagées** entre plusieurs téléphones/PC.
- Vider le cache du navigateur ou désinstaller l'app efface les données.
- Pour une vraie base de données partagée entre toute l'équipe (multi-appareils),
  il faudra ajouter un backend plus tard (ex. Firebase, Supabase) — dites-le-moi si
  vous voulez que je prépare cette étape.

## 1. Déployer sur GitHub Pages

1. Créez un nouveau dépôt GitHub (ex. `scpp-hse-app`).
2. Mettez-y les fichiers de ce dossier tels quels (`index.html` doit être **à la racine**
   du dépôt, ou dans un dossier `/docs`).
3. Dans le dépôt : **Settings → Pages → Source**, choisissez la branche `main` et le
   dossier `/ (root)` (ou `/docs` si vous les y avez mis).
4. GitHub vous donne une URL du type :
   `https://VOTRE-COMPTE.github.io/scpp-hse-app/`
5. Ouvrez cette URL : l'application doit s'afficher. Sur mobile, le navigateur proposera
   "Ajouter à l'écran d'accueil" (l'app s'installe comme une vraie app, avec icône).

## 2. Créer l'application Android (fichier .apk / .aab)

La méthode la plus simple et gratuite est **PWABuilder** (outil de Microsoft) qui génère
un projet Android à partir de votre PWA :

1. Allez sur **https://www.pwabuilder.com**
2. Collez l'URL de votre site GitHub Pages (ex. `https://VOTRE-COMPTE.github.io/scpp-hse-app/`)
3. Cliquez sur **Start**. PWABuilder analyse votre `manifest.json` et vos icônes
   (déjà prêts dans ce projet).
4. Choisissez le paquet **Android**.
5. Téléchargez le package généré : vous obtenez soit un **.apk** (à installer directement
   sur un téléphone, "sideload") soit un **.aab** (à publier sur le Google Play Store).
6. Si vous visez le Play Store, PWABuilder génère aussi les instructions pour signer
   l'app avec une clé (`keystore`) et créer une fiche sur la Google Play Console.

### Alternative en ligne de commande : Bubblewrap

Si vous préférez la ligne de commande (nécessite Node.js) :

```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest="https://VOTRE-COMPTE.github.io/scpp-hse-app/manifest.json"
bubblewrap build
```

Cela génère directement un projet Android Studio complet avec un `.apk`/`.aab` signé.

## 3. Mettre à jour l'application plus tard

Toute modification de `index.html` (nouvelles activités, nouveaux types d'anomalies…)
suffit à mettre à jour l'app : re-poussez le fichier sur GitHub, le site se met à jour
automatiquement, et l'app Android (si empaquetée en TWA) affiche toujours la dernière
version en ligne — pas besoin de republier l'APK à chaque changement de contenu.
