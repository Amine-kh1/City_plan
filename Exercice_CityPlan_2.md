# Exercice CityPlan 2

## Corrigé de l'exemple

La grille fait 10 × 14 cases et la distance maximale est de 6.
Les utilities 5 et 6 sont du même type. L'utility 7 est d'un autre type.

Voici les distances corrigées :

| Résidence | Utility 5 | Utility 6 | Utility 7 |
| --- | ---: | ---: | ---: |
| R1 | 10 | 1 | 4 |
| R2 | 6 | 2 | 9 |
| R3 | 4 | 2 | 4 |
| R4 | 1 | 9 | 8 |

Je garde les utilities à une distance de 6 ou moins. Pour chaque résidence, je compte chaque type une seule fois.

- **R1**, capacité 10 : accès à 6 et 7, donc deux types → **20 points**.
- **R2**, capacité 12 : accès à 5 et 6, mais un seul type → **12 points**.
- **R3**, capacité 15 : accès à 5, 6 et 7, donc deux types → **30 points**.
- **R4**, capacité 20 : accès à 5 seulement → **20 points**.

Cela donne **82 points avec R4**. Mais R4 contient un trou fermé : elle est invalide. En l'ignorant, le score est donc **62 points**.

## Ma première idée de méthode gloutonne

Je commence par placer une résidence et une utility proches, car un bâtiment seul ne rapporte rien sur une grille vide.

Ensuite, j'essaie les placements possibles et je garde celui qui ajoute le plus de points :

- Une nouvelle résidence rapporte selon sa capacité et les types proches.
- Une nouvelle utility rapporte si elle apporte un type qui manque aux résidences proches.
- Si aucun bâtiment seul ne rapporte, j'essaie une nouvelle paire résidence–utility.

Pour chaque placement, je vérifie que :

- Le plan reste dans la grille.
- Les cases `#` ne se chevauchent pas. Les `.` restent libres et peuvent accueillir un autre bâtiment.
- La distance est prise entre les deux `#` les plus proches, en comptant les déplacements horizontaux et verticaux.

Je garde les formes fournies, sans les tourner. Je peux réutiliser un même plan plusieurs fois selon les règles du problème.

### Pseudo-code simple

```text
Commencer avec une grille vide.
Placer la paire résidence–utility qui rapporte le plus de points,
si elle tient dans la grille, sans chevauchement, à distance ≤ 6.

Répéter :
    Chercher le bâtiment qui ajoute le plus de points
    parmi les placements autorisés.

    Si aucun bâtiment seul ne rapporte :
        Chercher une paire résidence–utility autorisée
        et à distance ≤ 6 qui ajoute des points.

    Si aucun ajout ne rapporte, arrêter.
    Sinon, placer le ou les bâtiments choisis et actualiser le score.
```

Cette méthode me donne une première solution, mais pas forcément la meilleure : un bâtiment placé trop tôt peut bloquer de meilleures possibilités.
