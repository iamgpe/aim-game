# Aim Game

Un mini jeu réalisé sous demande d'une entreprise pour un test.

> ### Consigne
> ------
> 
> Construisez une page web avec Vuejs (et toute autre bibliothèque), l'utilisateur voit 5 cercles apparaître aléatoirement sur l'écran. Les règles sont très simples :
> 
> - Le joueur a 10 secondes au premier tour pour cliquer sur tous les cercles
> - Un tableau de bord est affiché en bas de page
> - Chaque cercle cliqué récompense 1 point au joueur 🏆
> - Au prochain tour :
>   - le temps et la taille des cercles seront réduits de 25%.
>   - le tableau d'affichage restera en place jusqu'à ce que le joueur perde
>   - si le joueur perd, réinitialiser le jeu
> 
> __Bonus :__
> Les cercles se réduisent au néant pendant le tour

## Project  Build Setup

``` bash
# installation des dépendances
npm install

# lancement du serveur avec hot reload disponible à l'adresse http://localhost:8080
npm run dev

# montage de l'application pour la production
npm run build
```