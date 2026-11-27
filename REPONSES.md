# Réponses au TP7

## Q1

`actions/checkout` n'a pas besoin de `with` pour cette utilisation, car ses paramètres utiles sont optionnels et ses valeurs par défaut permettent déjà de récupérer le dépôt. `actions/setup-node` utilise `with` pour fournir explicitement le paramètre `node-version: 20`.

## Q2

Les jobs GitHub Actions utilisent des environnements isolés sur des runners différents. Les fichiers générés dans `build` ne sont pas automatiquement visibles dans `verify`. Il faut les transférer avec `actions/upload-artifact@v4`, puis les récupérer dans `verify` avec `actions/download-artifact@v4`.

## Q6

`actions/checkout` doit s'exécuter avant une action locale afin que les fichiers du dépôt soient présents sur le runner. Une action locale située sous `.github/actions/` est lue depuis le workspace récupéré par checkout.

## Q7

Dans une action composite, chaque step `run` doit déclarer son shell avec `shell: bash`, car GitHub ne peut pas déduire automatiquement quel interpréteur utiliser pour cette step locale.

## Informations relevées sur actions/setup-node

- Badge Marketplace : « Verified » — créateur vérifié comme organisation partenaire officielle.
- Nombre d'étoiles du dépôt : environ 5 000 étoiles.
- Code source : principalement dans le dossier `src/`, avec le code compilé distribué dans `dist/`.

# Réponses au TP8

## Q1

La matrice de base génère 6 jobs :

- `ubuntu-latest` avec Python 3.11, 3.12 et 3.13 ;
- `windows-latest` avec Python 3.11, 3.12 et 3.13.

## Q2

Il y a 6 jobs au total. `exclude` retire `windows-latest` avec Python 3.11 des 6 combinaisons initiales, puis `include` ajoute `ubuntu-latest` avec Python 3.14.

## Q3

Exclure Python 3.14 ne retire pas la combinaison ajoutée par `include`. `exclude` s'applique aux combinaisons de la matrice de base, avant l'ajout de cette combinaison par `include`.

## Q4

Avec `fail-fast: true`, le test sur `windows-latest` et Python 3.12 échoue, puis le job `windows-latest` et Python 3.13 est annulé ; les quatre autres jobs réussissent. Avec `fail-fast: false`, les six jobs s'exécutent jusqu'au bout : les tests Windows en Python 3.12 et 3.13 échouent, et les quatre autres réussissent.

Je choisirais `true` pour arrêter rapidement une matrice dès qu'un échec rend les autres résultats moins utiles. Je choisirais `false` pour obtenir les résultats de toutes les combinaisons, notamment pour diagnostiquer une incompatibilité limitée à certains environnements.

## Q5

`max-parallel` permet de limiter les coûts et la consommation de runners en évitant de lancer trop de jobs simultanément. Il permet aussi de respecter les limites de concurrence de l'organisation et de réduire la charge simultanée sur des ressources partagées.
