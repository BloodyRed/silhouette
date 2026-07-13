# 🏠 Silhouette — Carnet de progression

Application web de suivi de régime et de remise en forme : poids, mensurations, photos avant/après, rappels de sport, planning hebdomadaire, liste de courses et synchronisation cloud — le tout dans **un seul fichier HTML**, sans installation, sans serveur, sans dépendance.

> Interface sombre moderne (dégradé rose → violet → bleu), 100 % en français, optimisée mobile avec barre de navigation fixe façon app native.

---

## ✨ Fonctionnalités

### 📊 Tableau de bord
- **Anneau de progression** vers l'objectif de poids, avec pourcentage du chemin parcouru
- Poids actuel, IMC, reste à perdre et **estimation du délai** basée sur le rythme des 30 derniers jours
- **Courbe de poids** sur 90 jours (SVG, courbe lissée, ligne d'objectif)
- **Phrase de motivation** qui change chaque jour
- **Calendrier de la semaine** : séances de sport et événements en un coup d'œil
- Habitudes du jour : verres d'eau, énergie, victoires hors balance
- **Sections réorganisables** (▲▼) — l'ordre est mémorisé

### ⚖️ Poids
- Saisie rapide, historique daté avec notes, graphique complet
- **Point de comparaison épinglable (📍)** : repartir d'une pesée précise (reprise de poids, nouvelle phase) sans perdre l'historique
- **Import CSV Withings** (doublons ignorés automatiquement)

### 📏 Mensurations
- 11 zones prédéfinies : buste, poitrine, bras, taille, hanches, fesses, haut/milieu de cuisse, genou, mollet, cheville
- **Zones personnalisées** ajoutables (cou, poignet…)
- Note individuelle par mesure + note générale par séance
- Carte d'évolution par zone avec mini-graphique et historique détaillé

### 📷 Photos avant / après
- Compression automatique côté client (~100-150 Ko par photo)
- **Comparateur côte à côte** : écart en jours et différence de poids calculés automatiquement
- Deux modes : **stockage privé sur l'appareil** (par défaut) ou synchronisation via ImgBB (optionnel)

### 🔔 Rappels de sport
- Jours de la semaine, heure, délai de préavis (à l'heure, 15/30 min, 1 h, 2 h avant)
- Notifications navigateur (application ouverte dans un onglet)
- **Glisser-déposer sur le calendrier** pour réorganiser *la semaine en cours uniquement* — les rappels habituels ne changent pas
- **Événements** ponctuels ou répétés (quotidien/hebdomadaire, jusqu'à 30 occurrences)

### 🛒 Nutrition
- Liste de courses avec cases à cocher, compteur d'articles restants, nettoyage des articles cochés

### ☁️ Synchronisation cloud (gratuite)
- Sauvegarde multi-appareils via [jsonbin.io](https://jsonbin.io) (plan gratuit)
- Synchro automatique regroupée (5 s après chaque modification) + boutons manuels ↑↓
- Protection contre l'écrasement par des données plus anciennes
- Export / restauration de sauvegarde JSON en local

---

## 🚀 Utilisation

### Option 1 — En local
Télécharger `silhouette.html` et l'ouvrir dans un navigateur. C'est tout.

### Option 2 — GitHub Pages
1. Pousser le dépôt sur GitHub
2. **Settings → Pages → Source : Deploy from a branch** → branche `main`, dossier `/ (root)`
3. Renommer `silhouette.html` en `index.html` (ou y accéder via `…/silhouette.html`)
4. L'app est en ligne — sur mobile, utiliser **« Ajouter à l'écran d'accueil »** pour un usage plein écran

### Option 3 — Cloudflare Pages
Glisser le dossier dans un projet Cloudflare Pages : déploiement en quelques secondes.

> ⚠️ La synchronisation cloud (JSONBin) et l'envoi de photos (ImgBB) nécessitent que le fichier soit ouvert directement dans un navigateur ou hébergé — certains aperçus intégrés bloquent les requêtes externes.

---

## ⚙️ Configuration

### Cloud JSONBin (multi-appareils)
1. Créer un compte gratuit sur [jsonbin.io](https://jsonbin.io)
2. Page **API Keys** → copier la clé `X-Master-Key`
3. Dans l'app : **⚙ → Connexions → Sauvegarde cloud** → coller la clé → **« Créer mon espace »**
4. Sur un autre appareil : coller la même clé + l'identifiant d'espace → **« Récupérer ↓ »**

### Photos ImgBB (optionnel)
1. Compte gratuit sur [imgbb.com](https://imgbb.com), puis aller sur [api.imgbb.com](https://api.imgbb.com) → **Get API key**
2. Dans l'app : **Progression → 📷 Photos → Synchronisation des photos** → coller la clé

> 🔒 **Confidentialité** : sans clé ImgBB, les photos restent uniquement sur l'appareil. Avec ImgBB, les images sont hébergées en ligne et accessibles à quiconque possède le lien — à considérer pour des photos personnelles.

### Import Withings
App Withings : **Profil → Paramètres → Télécharger mes données** → importer le fichier `weight.csv` dans **⚙ → Connexions**.

---

## 🛠️ Technique

- **Un seul fichier** `silhouette.html` : HTML + CSS + JavaScript vanilla, zéro dépendance, zéro build
- Graphiques **SVG générés à la main** (courbes lissées Catmull-Rom, sparklines)
- Stockage : API de stockage de l'hôte quand disponible, sinon `localStorage`, avec repli mémoire
- Icônes intégrées en base64 (favicon SVG + apple-touch-icon) — aucun asset externe requis
- Polices Google Fonts (Outfit / Inter) avec repli système
- Responsive : barre de navigation fixe en bas sur mobile, agenda vertical, anti-zoom iOS (champs 16 px), `safe-area-inset` pour les encoches

## 📁 Contenu du dépôt

| Fichier | Rôle |
|---|---|
| `silhouette.html` | L'application complète |
| `favicon.ico` | Icône navigateur (16/32/48 px) pour l'hébergement |
| `silhouette-icone-512.png` | Icône haute résolution (PWA, écran d'accueil) |
| `recettes.json` | Base des suggestions de recettes — éditable librement |

### Personnaliser les recettes (`recettes.json`)
Placer le fichier **à côté de `index.html`**. S'il est absent (ou en ouverture locale), l'app utilise sa base intégrée. Chaque recette suit ce schéma :

```json
{
  "n": "Nom de la recette",
  "e": "🥘",
  "t": 30,
  "i": ["ingrédient 1", "ingrédient 2"],
  "d": "Préparation en quelques phrases."
}
```

`n` = nom · `e` = emoji (optionnel) · `t` = temps en minutes (optionnel) · `i` = ingrédients (mots simples — la correspondance ignore accents et pluriels) · `d` = préparation. Les recettes ajoutées **dans l'app** (« ＋ Ajouter ma propre recette ») sont, elles, stockées avec les données utilisateur et synchronisées dans le cloud.

## 🗒️ Données

Toutes les données (pesées, mesures, photos locales, rappels, liste de courses, préférences) restent **chez l'utilisateur** : dans le navigateur et, si activé, dans *son* espace JSONBin personnel. Aucune télémétrie, aucun compte requis.

## 📄 Licence

MIT — libre d'utilisation, de modification et de partage.

---

*Projet personnel développé avec l'aide de Claude (Anthropic).*
