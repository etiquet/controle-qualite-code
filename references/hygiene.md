# Protocole de passe d'hygiène

Utiliser ce protocole pour obtenir des mesures reproductibles sans confondre
métrique et qualité.

## Sommaire

- [1. Déterminer l'autorisation](#1-déterminer-lautorisation)
- [2. Fixer le contrat de mesure](#2-fixer-le-contrat-de-mesure)
- [3. Sélectionner les outils](#3-sélectionner-les-outils)
- [4. Enregistrer chaque mesure](#4-enregistrer-chaque-mesure)
- [5. Décider si une comparaison est valide](#5-décider-si-une-comparaison-est-valide)
- [6. Recommander sans surinterpréter](#6-recommander-sans-surinterpréter)
- [7. Appliquer seulement sur demande](#7-appliquer-seulement-sur-demande)

## 1. Déterminer l'autorisation

Appliquer le niveau le moins permissif compatible avec la demande :

- `AUDIT` : inspecter et mesurer sans modifier l'arbre de travail ; utiliser ce
  niveau par défaut.
- `JOURNAL` : créer ou compléter uniquement le journal, si cela est
  explicitement demandé.
- `REMÉDIATION` : modifier le code dans le périmètre demandé, puis valider et
  re-mesurer, si cela est explicitement demandé.

Aucun niveau n'autorise implicitement une installation, un auto-fix, un commit,
un push ou une pull request. En mode `AUDIT`, diriger les rapports et caches
vers un répertoire temporaire. Si un outil ne permet pas de garantir l'absence
d'artefact dans l'arbre de travail, ne pas l'exécuter sans autorisation.
Comparer l'état Git avant et après ; signaler toute mutation inattendue sans
effacer les changements préexistants.

## 2. Fixer le contrat de mesure

Relever avant toute commande :

- date ISO 8601 et fuseau ;
- commit complet et état `clean` ou `dirty` ;
- périmètre inclus et exclusions ;
- langage et sous-projets ;
- fichiers générés, vendored, migrations et tests inclus ou non ;
- métriques utiles au problème posé.

Une passe sur un arbre sale peut informer, mais ne constitue pas une référence
temporelle durable.

## 3. Sélectionner les outils

Utiliser dans cet ordre :

1. commande documentée ou scriptée par le dépôt ou sa CI ;
2. outil déclaré dans un manifeste ou verrou et déjà disponible ;
3. binaire local disponible, en enregistrant sa version exacte ;
4. sinon, marquer la métrique `INDISPONIBLE`.

Ne jamais installer un outil, utiliser un mode `--fix` ou remplacer une mesure
absente par zéro sans autorisation. Privilégier duplication, complexité et
couverture lorsque le projet les mesure déjà. Utiliser le volume comme contexte,
jamais comme dette à lui seul.

Traiter les audits de vulnérabilités comme des mesures contextuelles : relever
le lockfile, la source et la date de la base d'avis. Ne pas les intégrer à une
courbe reproductible si cette base n'est pas figée.

## 4. Enregistrer chaque mesure

Utiliser ce schéma pour chaque valeur :

| Champ | Contenu |
|---|---|
| `date` | Date ISO 8601 et fuseau |
| `commit` | SHA complet et état de l'arbre |
| `scope` | Chemins inclus |
| `exclusions` | Chemins et catégories exclus |
| `metric` | Définition précise |
| `value` | Valeur brute ou `indisponible` |
| `unit` | `%`, occurrences, score, lignes, etc. |
| `details` | Numérateur, dénominateur, seuil ou maximum |
| `tool` | Nom et version exacte |
| `command` | Commande et options pertinentes |
| `config` | Fichier/configuration et empreinte si utile |
| `status` | `MESURÉ`, `INDISPONIBLE`, `ERREUR` ou `NON_APPLICABLE` |
| `notes` | Limites, erreurs et environnement |

Conserver les résultats bruts utiles ou leur emplacement. Ne pas convertir une
erreur en zéro.

Exemple de journal :

```markdown
## 2026-07-25T10:30:00+02:00 — commit abc123

- Autorisation : JOURNAL
- Arbre : clean
- Périmètre : `src/`
- Exclusions : code généré et dépendances vendored

| Métrique | Statut | Valeur | Détails | Outil/version | Commande/config |
|---|---|---:|---|---|---|
| duplication.lines.percent | MESURÉ | 4,2 % | 420 / 10 000 lignes | jscpd 4.0.5 | `jscpd src` + `.jscpd.json` |
| coverage.branches.percent | INDISPONIBLE | — | service absent | pytest-cov 7.0.0 | `pytest --cov` |
```

## 5. Décider si une comparaison est valide

Comparer une métrique à la passe antérieure compatible, pas simplement à la
dernière passe. Exiger que ces éléments restent matériellement identiques :

- définition et unité de la métrique ;
- outil et version ;
- commande et configuration ;
- périmètre et exclusions ;
- suite de tests pour la couverture ;
- seuil pour la complexité.

Si l'un change, expliquer l'incompatibilité et créer un nouvel état zéro pour
la seule métrique concernée. Ne produire ni delta ni tendance pour une mesure
indisponible ou non comparable.

Décrire chaque série séparément : `amélioration`, `stable`, `dégradation` ou
`non comparable`. Si les métriques divergent, conclure `tendance mixte`.
Éviter un score composite non justifié.

## 6. Recommander sans surinterpréter

Proposer au plus trois actions. Pour chacune, indiquer :

- preuve observée et fichiers concernés ;
- bénéfice attendu et risque ;
- périmètre explicitement borné ;
- tests de caractérisation ou de protection ;
- critère de fin mesurable ;
- métrique à reprendre à l'identique.

Ne pas recommander une abstraction uniquement pour faire baisser un compteur de
duplication. Ne pas assimiler couverture élevée, faible complexité ou faible
volume à une preuve de justesse. Distinguer une action `recommandée` d'une
action réellement `exécutée`.

## 7. Appliquer seulement sur demande

En mode `REMÉDIATION` :

1. vérifier l'état de travail et isoler la modification ;
2. exécuter les tests de référence ;
3. réaliser une action bornée ;
4. réexécuter les tests ;
5. reprendre les mêmes mesures ;
6. mettre à jour le journal seulement si cette écriture est autorisée ;
7. rapporter aussi toute métrique dégradée.
