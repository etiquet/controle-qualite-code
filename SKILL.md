---
name: controle-qualite-code
description: >-
  Réviser une contribution de code avec des preuves et un verdict explicite,
  ou mesurer l'évolution de la dette technique avec un rituel reproductible.
  Utiliser ce skill pour relire ou auditer un diff, une branche ou une pull
  request, décider si une contribution est révisable et acceptable, contrôler
  du code assisté par IA sans lui appliquer un standard différent, ou lancer
  une passe d'hygiène sur une base existante. Produire selon le cas un verdict
  NON RÉVISABLE / ACCEPTER / CORRIGER / REFUSER, ou un relevé de métriques
  comparable accompagné de 1 à 3 recommandations bornées.
---

# Contrôler la qualité du code

## Appliquer le contrat commun

Sélectionner un seul mode à partir de la demande :

- **Revue** : évaluer une contribution, un diff, une branche ou une pull
  request.
- **Hygiène** : mesurer une base existante et proposer une réduction de dette.

Ne pas mélanger les modes sans demande explicite. Rester en lecture seule par
défaut : inspecter et recommander n'autorise ni modification, ni installation,
ni commit, ni publication. Réutiliser les outils déjà configurés dans le
projet. Demander une autorisation avant toute action supplémentaire nécessaire.

Fonder chaque conclusion sur une preuve accessible. Distinguer :

- **Observation** : directement établie par le code, les métadonnées ou une
  commande exécutée.
- **Risque** : conséquence plausible à confirmer, avec niveau de confiance.
- **Non évalué** : information inaccessible ou impossible à vérifier.
- **Non applicable** : critère sans rapport avec la contribution, avec
  justification.

Ne jamais transformer une absence d'accès en preuve d'absence. Signaler
séparément qu'un élément obligatoire est connu comme absent ou qu'il n'a pas pu
être vérifié.

Appliquer les mêmes critères techniques quelle que soit l'origine du code.
Tracer l'assistance d'un agent lorsqu'elle est connue ou exigée par le projet,
sans inventer son identité, son modèle ou son rôle.

## Conduire une revue

Lire [`references/checklist.md`](references/checklist.md) pour toute revue.

1. **Cadrer.** Identifier l'intention, le périmètre, le propriétaire humain,
   les preuves de comportement, les contraintes et les artefacts accessibles.
2. **Évaluer l'entrée.** Déterminer si la contribution est suffisamment
   expliquée, attribuée, prouvée et bornée pour permettre une revue responsable.
3. **Maintenir un filet de sécurité.** Même si l'entrée est insuffisante,
   examiner le diff accessible à la recherche de risques critiques évidents :
   secret exposé, contrôle d'accès affaibli, injection, destruction ou
   corruption de données, migration irréversible et dépendance suspecte. Ne pas
   présenter ce balayage comme une revue complète.
4. **Évaluer le fond si l'entrée est révisable.** Contrôler le comportement,
   les tests, les interfaces réelles, les erreurs, la sécurité, la lisibilité,
   la duplication et la surface de maintenance. Sinon, marquer ces contrôles
   non évalués et passer au verdict après le balayage critique.
5. **Évaluer le statut accessible.** Vérifier le caractère durable ou jetable,
   la responsabilité humaine, le processus de revue et la traçabilité connue.
6. **Rendre un verdict.** Appliquer la première condition satisfaite :

   | Priorité | Condition | Verdict |
   |---|---|---|
   | 1 | Des preuves suffisantes montrent que l'intention ou l'approche est fondamentalement dangereuse ou incompatible avec les exigences, et qu'une correction locale ne suffit pas | **REFUSER** |
   | 2 | Une information ou un artefact indispensable manque ou reste inaccessible et empêche une décision sûre | **NON RÉVISABLE** |
   | 3 | Au moins un défaut bloquant confirmé est réparable avant fusion | **CORRIGER** |
   | 4 | Les contrôles nécessaires sont évalués, ou justifiés non applicables, et aucun défaut bloquant n'est confirmé | **ACCEPTER** |

Ne pas utiliser `REFUSER` pour punir une information simplement inaccessible.
Ne pas utiliser `ACCEPTER` si un contrôle indispensable reste non évalué.

Pour chaque constat, citer `source:emplacement` — fichier et ligne, description
de PR, issue, journal CI ou configuration —, indiquer la sévérité, expliquer
l'impact et demander une action vérifiable. Prioriser les défauts de
comportement, de sécurité et de maintenance ; éviter d'ériger une préférence
stylistique en bloquant.

## Conduire une passe d'hygiène

Lire [`references/hygiene.md`](references/hygiene.md) avant de mesurer ou de
comparer la dette.

1. Déterminer l'autorisation : `AUDIT` en lecture seule, `JOURNAL` si l'écriture
   du relevé est demandée, ou `REMÉDIATION` si le code doit être modifié.
2. Définir le commit, l'état de l'arbre, le périmètre, les exclusions et les
   métriques utiles.
3. Découvrir les outils déjà déclarés par le projet et relever leurs versions,
   commandes et configurations.
4. Mesurer sans modification non autorisée. Conserver les erreurs et valeurs
   indisponibles au lieu de les remplacer par des estimations ou par zéro.
5. Comparer uniquement des mesures compatibles. Si la méthode, l'outil, la
   configuration ou le périmètre change matériellement, créer un nouvel état
   zéro pour la métrique et ne pas annoncer de tendance.
6. Proposer 1 à 3 actions bornées, ordonnées par impact et risque, avec une
   preuve de fin et des tests de protection.
7. Après une refactorisation autorisée, réexécuter les tests et exactement les
   mêmes mesures.

Décrire la tendance métrique par métrique. Ne pas réduire la qualité globale à
un score composite arbitraire.

## Tracer l'assistance d'un agent

Lorsque l'assistance est connue ou demandée, consigner au minimum :

- l'humain responsable de la contribution ;
- le nom réel de l'agent ou de l'outil ;
- son modèle ou sa version uniquement si l'environnement les expose ;
- son rôle et le périmètre produit ou modifié ;
- les validations qu'il a réellement exécutées.

Employer la valeur `non exposé par l'environnement` plutôt que deviner. Ne
jamais présenter l'agent comme propriétaire humain. Respecter d'abord la
convention du projet ; à défaut, proposer une section de pull request
`Traçabilité de l'agent` et un trailer `Assisted-by:`.

## Valider une évolution du skill

Pour modifier ce skill, utiliser les cas de
[`references/evaluation-scenarios.md`](references/evaluation-scenarios.md).
Vérifier que la sortie reste fondée sur les preuves, que les verdicts suivent
la matrice et qu'une demande de revue ne provoque aucune mutation implicite.
