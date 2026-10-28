# Vérification bénéficiaire CSU

Mini application web : on saisit un code d'immatriculation (ex. `P6204F8290`) et la fiche du bénéficiaire s'affiche (identité, couverture, validité, adhésion).

L'accès est protégé par un mot de passe partagé entre les agents. Le mot de passe n'est **jamais** écrit dans le code : il est défini dans les réglages de l'hébergeur.

## Contenu

| Fichier | Rôle |
|---|---|
| `index.html` | La page (saisie du code, affichage de la fiche, écran de mot de passe) |
| `api/beneficiaire.js` | Relais qui vérifie le mot de passe puis interroge l'API CSU (évite le blocage CORS) |
| `server.js` | Serveur pour tester sur votre ordinateur (facultatif) |
| `vercel.json` | En-têtes de sécurité (pas d'indexation Google, pas d'intégration dans d'autres sites) |

## Mise en ligne : GitHub + Vercel

### 1. Mettre le code sur GitHub
1. Sur github.com, cliquez **New repository**, nommez-le `verif-csu` et cochez **Private**.
2. Cliquez **uploading an existing file** et glissez tout le contenu du dossier, y compris le dossier `api`.
3. Cliquez **Commit changes**.

### 2. Déployer sur Vercel
1. Allez sur vercel.com et connectez-vous avec votre compte GitHub (l'offre gratuite « Hobby » suffit).
2. Cliquez **Add New… → Project**, puis **Import** à côté de `verif-csu`.
3. Avant de cliquer **Deploy**, ouvrez **Environment Variables** et ajoutez :
   - **Name** : `ACCES_MOT_DE_PASSE`
   - **Value** : le mot de passe des agents (au moins 12 caractères, pas un mot du dictionnaire)
4. Cliquez **Deploy**. Après environ une minute, Vercel donne l'adresse du site, du type `https://verif-csu-xxxx.vercel.app`.

### 3. Utilisation
- Ouvrez l'adresse, saisissez le mot de passe, puis les codes.
- Le mot de passe est oublié à la fermeture de l'onglet.
- Le lien `https://votre-site.vercel.app/?code=P6204F8290` ouvre directement une fiche après connexion.

### Changer le mot de passe
Vercel → votre projet → **Settings → Environment Variables** → modifiez `ACCES_MOT_DE_PASSE`, puis **Deployments → … → Redeploy**. Les agents devront saisir le nouveau mot de passe.

### Mettre à jour l'application
Modifiez les fichiers sur GitHub : Vercel remet automatiquement le site en ligne à chaque modification.

## Tester sur votre ordinateur (facultatif)

Il faut Node.js 18 ou plus récent (nodejs.org). Dans un terminal ouvert dans le dossier :

**Windows (cmd)**
```
set ACCES_MOT_DE_PASSE=monmotdepasse&& node server.js
```

**macOS / Linux**
```
ACCES_MOT_DE_PASSE=monmotdepasse node server.js
```

Puis ouvrez http://localhost:3000. N'ouvrez pas `index.html` en double-cliquant : la page doit passer par le relais.

## Sécurité

- Les fiches contiennent des données personnelles (téléphone, pièce d'identité). Ne partagez le mot de passe qu'avec les agents autorisés et changez-le si un agent quitte la structure.
- Gardez le dépôt GitHub en **Private**.
- Le site n'est pas indexé par les moteurs de recherche.
