# Boutique Web — Bénéfice Plus

Page web publique (1 seul fichier HTML, sans build) permettant à un client sans compte
de consulter la boutique d'un commerçant et de passer commande. C'est la page qui s'ouvre
quand un client clique sur le lien de boutique partagé depuis l'app Flutter.

## Configuration

1. Ouvrez `index.html`.
2. Remplacez :
   ```js
   const SUPABASE_URL = 'https://VOTRE-PROJET.supabase.co';
   const SUPABASE_ANON_KEY = 'VOTRE_CLE_ANON_SUPABASE';
   ```
   par les mêmes valeurs que dans `lib/main.dart` de l'application Flutter.

## Déploiement sur Vercel

1. Installez la CLI Vercel si besoin : `npm i -g vercel`
2. Depuis le dossier `boutique-web/` :
   ```
   vercel --prod
   ```
3. Vercel vous donne une URL (ex: `https://boutique-beneficeplus.vercel.app`).
4. Ouvrez `lib/models/virtual_shop.dart` dans l'app Flutter et remplacez :
   ```dart
   const String kBoutiqueWebBaseUrl = 'https://VOTRE-BOUTIQUE.vercel.app';
   ```
   par l'URL réelle obtenue à l'étape 3.
5. Recompilez l'app. Le lien de boutique généré (`Ma boutique` dans l'app) fonctionnera
   désormais réellement : n'importe qui peut l'ouvrir dans un navigateur, sans installer l'app.

## Comment ça marche

- L'URL `https://votre-boutique.vercel.app/mon-slug-de-boutique` est réécrite vers
  `index.html` (voir `vercel.json`), qui lit le slug dans l'adresse.
- La page interroge directement Supabase (table `virtual_shops` puis `products`)
  avec la clé publique (anon), autorisée en lecture seule pour les boutiques publiées
  (voir les politiques RLS dans `supabase_schema.sql`).
- Une commande passée sur cette page est insérée dans `shop_orders`, visible ensuite
  par le commerçant dans l'écran "Commandes reçues" de l'app.
