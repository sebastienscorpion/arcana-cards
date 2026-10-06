# Arcana Cards — application Vue 3 (frontend)

Boutique de cartes à jouer anciennes et nouvelles, avec animation de mélange et de distribution sur la page d'accueil.

## Installation

1. Installer Node.js LTS (18 ou plus) : https://nodejs.org  (vérifier : `node -v` et `npm -v`)
2. Ouvrir un terminal dans le dossier du projet : `cd arcana-cards`
3. Installer les dépendances : `npm install`
4. Lancer en développement : `npm run dev`  puis ouvrir http://localhost:5173
5. Version de production : `npm run build` (dossier `dist/`), test local : `npm run preview`

## Structure

    src/
      main.js                point d'entrée
      App.vue                page d'accueil
      assets/main.css        styles globaux (thème clair/sombre)
      data/products.js       cartes en vente
      components/
        AppHeader.vue        barre de navigation (boutons non fonctionnels)
        AppFooter.vue        pied de page (boutons non fonctionnels)
        ShuffleStage.vue     animation mélange + distribution (2 ou 3 joueurs)
        HandSvg.vue          main en SVG
        PlayingCard.vue      carte à jouer (face et dos)
        ProductCard.vue      fiche produit
