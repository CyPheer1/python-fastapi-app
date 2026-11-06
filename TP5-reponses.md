# TP5 – Réponses

## Q0

Le dépôt contient le code dans `app/` et les tests dans `tests/`. Les dépendances de développement sont listées dans `requirements-dev.txt`. Le dossier `.github/workflows/` existe. La branche de travail est `feature/tp-ci`.

## Q1

Le workflow se lance sur un push vers `main` ou `feature/**` uniquement si un fichier de `app/**` ou `tests/**` est modifié. Pour une PR, il se lance lors de `opened` et `synchronize`.

## Q2

Une modification du seul `README.md` ne déclenche pas le workflow de push, car elle ne correspond pas aux `paths`. Une modification dans `app/` le déclenche, car elle correspond à un chemin filtré.

## Q3

Entre `branches` et `paths`, la logique est **ET** : les deux conditions doivent être satisfaites. Entre plusieurs types d’activité, la logique est **OU** : un seul type correspondant suffit.

## Q4

`tag.yml` est déclenché uniquement par un push de tag correspondant au motif `v*`, par exemple `v1.0.0`.

## Q5

Le workflow `02-ci.yml` contient `lint` et `build` en parallèle. Le job `test` dépend de `lint` et `build` avec `needs: [lint, build]`.

## Q6

Sans `needs`, les jobs peuvent démarrer en parallèle, donc le graphe est plus rapide mais l’ordre n’est plus garanti. Avec `needs`, `test` attend les deux jobs. Si `lint` échoue, `test` est ignoré car une de ses dépendances a échoué.

## Q7

Chaque job GitHub Actions utilise un runner isolé. Le checkout et l’installation de Python doivent donc être refaits dans chaque job.

## Q8

- Nom de l’application : `env.APP_NAME`
- Acteur : `github.actor`
- Événement : `github.event_name`
- Branche : `github.ref_name`
- SHA du commit : `github.sha`
- Système du runner : `runner.os`

## Q9

Sur un push, `github.event_name` vaut `push` et `github.head_ref`/`github.base_ref` sont vides. Sur une PR, `github.event_name` vaut `pull_request`, `github.head_ref` contient la branche source et `github.base_ref` contient la branche cible, ici `main`.

## Q10

`echo '${{ toJson(github) }}'` affiche le contexte GitHub complet. Le message du commit se trouve dans `github.event.head_commit.message` lors d’un événement `push`.

## Run complet

[Run CI](https://github.com/CyPheer1/python-fastapi-app/actions/runs/42298093128)
