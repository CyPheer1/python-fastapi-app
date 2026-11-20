# Notes de sécurité des actions tierces

## Q3

Il faut préférer une version majeure, comme `@v4`, ou mieux une révision exacte épinglée, plutôt que `@main`. `main` peut recevoir des mises à jour ou des changements sans validation préalable, alors qu'une version publiée fournit un comportement plus stable et reproductible.

## Q4

Le badge « Verified creator » indique que GitHub a vérifié manuellement que le créateur est une organisation partenaire officielle. Il confirme l'identité du créateur, mais il ne garantit pas que chaque version est exempte de bugs, de failles ou de comportement malveillant.

## Q5

Avant d'utiliser une action d'un fournisseur inconnu en production, il faut :

1. Lire et auditer le code source, les scripts exécutés et les dépendances incluses.
2. Vérifier l'activité du dépôt, les mainteneurs, les releases, les tests, les vulnérabilités déclarées et épingler une révision exacte ou une version vérifiée.
