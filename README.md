# TP5 - Correction GitHub Actions

## Q0 - Préparation du dépôt

Le code de l'application se trouve dans `app/` et les tests se trouvent dans `tests/`. Le fichier qui contient les dépendances de développement est `requirements-dev.txt`. Le dépôt contient aussi le dossier `.github/workflows/`. J'ai créé la branche `feature/tp-ci` pour réaliser le TP.

## Q1 - Filtres du workflow

Le workflow se déclenche sur un push vers `main` ou `feature/**`, seulement si un fichier de `app/**` ou `tests/**` est modifié. Pour une pull request, il se déclenche avec les événements `opened` et `synchronize`.

## Q2 - Modification du README et de app

Si je modifie seulement `README.md`, le workflow ne se déclenche pas car ce fichier n'est pas compris dans les chemins filtrés. Si je modifie un fichier dans `app/`, le workflow se déclenche car le chemin correspond au filtre.

## Q3 - Logique des filtres

Entre `branches` et `paths`, la logique est **ET** : les deux conditions doivent être respectées. Entre plusieurs types d'activité, la logique est **OU** : un seul type correspondant suffit.

## Q4 - Déclenchement par un tag

Le fichier `tag.yml` se déclenche uniquement lorsqu'un tag qui commence par `v` est envoyé, par exemple `v1.0.0`.

## Q5 - Les jobs du workflow CI

Le workflow contient trois jobs. Les jobs `lint` et `build` s'exécutent en parallèle. Le job `test` attend la fin des deux jobs grâce à `needs: [lint, build]`.

## Q6 - Rôle de needs

Sans `needs`, les jobs peuvent démarrer en même temps et le workflow peut être plus rapide, mais l'ordre n'est pas garanti. Avec `needs`, le job `test` attend `lint` et `build`. Si le lint échoue, le job `test` ne s'exécute pas.

## Q7 - Installation dans chaque job

Chaque job utilise un runner isolé. C'est pour cela qu'il faut refaire le checkout du code et l'installation de Python dans chaque job.

## Q8 - Contextes utilisés

- Nom de l'application : `env.APP_NAME`
- Acteur : `github.actor`
- Nom de l'événement : `github.event_name`
- Branche : `github.ref_name`
- SHA du commit : `github.sha`
- Système d'exploitation du runner : `runner.os`

## Q9 - Push et pull request

Pour un push, `github.event_name` vaut `push` et `github.head_ref` ainsi que `github.base_ref` sont vides. Pour une pull request, `github.event_name` vaut `pull_request`, `github.head_ref` contient la branche source et `github.base_ref` contient la branche cible, ici `main`.

## Q10 - Contexte GitHub complet

La commande `echo '${{ toJson(github) }}'` affiche toutes les informations du contexte GitHub. Avec un push, le message du commit est disponible dans `github.event.head_commit.message`.

## Résultat du workflow

Le workflow CI est passé avec succès : [voir le run complet](https://github.com/CyPheer1/python-fastapi-app/actions/runs/42298450428).
