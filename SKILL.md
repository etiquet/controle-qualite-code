---
name: controle-qualite-code
description: >-
  Grille de contrôle qualité pour réviser du code — en particulier du code
  généré ou co-écrit par une IA — avant de l'accepter dans une base maintenue,
  et rituel d'hygiène récurrent pour dégonfler la dette existante. À utiliser
  quand quelqu'un demande de « relire », « auditer », « évaluer la qualité »,
  « décider d'accepter une PR », « faire une revue de code », « vérifier du
  code IA », ou de lancer une « passe d'hygiène » / un « rituel qualité » sur
  une base. Mode revue : trois niveaux de règles (entrée, fond, statut) et un
  verdict motivé ACCEPTER / CORRIGER / REFUSER. Mode rituel : mesurer la dette
  (duplication, complexité, couverture), recommander des passes de
  refactorisation ciblées, re-mesurer, tracer la courbe.
---

# Contrôle qualité de code (à l'ère de l'IA)

## Principe directeur

Produire du code ne coûte presque plus rien ; le **comprendre, le maintenir et
l'assumer** coûte toujours aussi cher. Une revue de qualité ne sert donc plus à
« écrire mieux » mais à **décider quoi laisser entrer** dans une base qu'un
humain devra porter pendant des années. La question centrale de toute revue
n'est pas « ce code est-il joli ? » mais :

> **Quelqu'un pourra-t-il comprendre, faire évoluer et assumer ce code dans
> trois ans — et le coût de le maintenir est-il justifié par ce qu'il
> apporte ?**

Le code généré par IA est typiquement *syntaxiquement correct et sémantiquement
opaque* : il compile, les tests passent, mais l'intention est absente, les cas
limites sont négligés, des fonctions inexistantes sont parfois appelées, et la
duplication remplace la conception. Cette grille cible précisément ces défauts.

## Comment utiliser cette grille

Appliquer les trois niveaux **dans l'ordre**. Un échec au Niveau 1 stoppe la
revue : inutile de lire en détail un code qu'on ne peut pas assumer.

1. **Niveau 1 — Règles d'entrée** : vérifier *avant de lire le code* que la
   contribution est recevable et révisable. (Rétablit l'asymétrie coût.)
2. **Niveau 2 — Règles de fond** : vérifier la qualité *dans* le code.
3. **Niveau 3 — Règles de statut** : vérifier le *statut* du code (jetable vs
   durable) et sa traçabilité.

Pour chaque règle, attribuer : ✅ conforme · ⚠️ à corriger · ❌ bloquant.
Puis rendre un **verdict global motivé** (voir plus bas). Toujours citer des
lignes/fichiers précis — jamais un jugement vague.

Le détail exécutable de chaque point (questions à se poser, signaux d'alerte,
exemples) est dans `references/checklist.md`. Charger ce fichier lorsqu'une
revue complète et systématique est demandée.

---

## Niveau 1 — Règles d'entrée (avant de lire le code)

Un échec ici est en principe **bloquant** : on renvoie sans réviser.

1. **Intention déclarée** — La contribution énonce l'*effet recherché* (le
   « pourquoi »), pas seulement le diff. Pas d'intention lisible → refus.
2. **Propriétaire humain responsable** — Un humain nommé assume la PR. L'IA
   n'est jamais redevable ; un humain l'est toujours.
3. **Preuve de comportement** — Reproduction du bug, ou test qui échouait
   avant / passe après. On évalue un *comportement*, pas une esthétique.
4. **Coût de revue proportionné** — Une PR massive générée en minutes n'a pas
   droit à des heures de revue par défaut. Elle doit être rendue révisable :
   petite, ciblée, découpée, expliquée.

## Niveau 2 — Règles de fond (dans le code)

1. **Lisibilité / intention** — Le code se lit-il comme une explication de ce
   qu'il fait et pourquoi ? Nommage, structure, absence de complexité gratuite.
2. **Pas de duplication** — Cible n°1 du code IA (blocs clonés en forte hausse
   depuis l'arrivée des assistants). Est-ce refactorisé, ou copié-collé ?
3. **Justesse vérifiée ≠ « les tests passent »** — Les tests testent-ils le bon
   comportement, ou ont-ils été écrits pour verdir ? Se méfier de la « triche »
   (optimiser la mesure, pas l'intention).
4. **API et fonctions réelles** — Le code appelle-t-il uniquement ce qui existe
   vraiment ? Traquer les hallucinations (appels, méthodes, options inventés).
5. **Erreurs et cas limites** — Chemin nominal soigné mais bords négligés :
   entrées vides/nulles, concurrence, échecs réseau, valeurs extrêmes.
6. **Sécurité, secrets, dépendances** — Secrets en dur, dépendances hallucinées
   ou vulnérables, entrées non validées, injection.
7. **Surface de maintenance** — Ce code crée-t-il une dette qu'un humain devra
   porter longtemps ? Le gain justifie-t-il le poids ajouté ?

## Niveau 3 — Règles de statut (méta)

1. **Marquer le jetable comme jetable** — Tout prototype naît avec une « date
   de mort » ou une étiquette *non maintenu*. Le sacrificiel non étiqueté qui
   devient fondation par accident est la source réelle de dette.
2. **Une porte unique** — Le code d'un dirigeant ou d'une IA passe par la même
   revue que celui de n'importe qui. Aucun passe-droit hiérarchique ou
   technologique. (C'est la règle du noyau Linux.)
3. **Tracer l'origine IA** — Marquer le code généré (à la manière du tag
   `Assisted-by` du noyau Linux), non pour l'interdire, mais pour mesurer son
   taux de défaut réel dans le temps.

---

## Verdict global

Conclure toute revue par l'un des trois verdicts, **motivé et actionnable** :

- **ACCEPTER** — Niveaux 1 et 3 conformes, Niveau 2 sans ❌. Réserves ⚠️
  mineures listées mais non bloquantes.
- **CORRIGER** — Recevable sur le fond mais points ⚠️/❌ précis à traiter avant
  fusion. Lister chaque point avec fichier:ligne et l'action attendue.
- **REFUSER** — Échec d'une règle d'entrée (N1) ou défaut de fond majeur
  (justesse fausse, hallucination d'API, faille) sans propriétaire prêt à
  assumer la reprise. Expliquer pourquoi, brièvement, avec respect.

Toujours : citer des emplacements précis, distinguer le bloquant du cosmétique,
et se rappeler que **refuser proprement fait partie du travail de qualité** —
la gentillesse est de dire non clairement plutôt que de laisser entrer une dette
que quelqu'un paiera en silence.

---

## Mode rituel — la passe d'hygiène (dégonfler la dette)

La revue décide de ce qui entre ; le rituel s'occupe de ce qui est déjà entré.
À lancer **à intervalle régulier** (hebdomadaire recommandé), pas seulement à
la fusion. Déroulé en quatre temps :

1. **Mesurer l'état de la base.** Produire les métriques de dette :
   - duplication (blocs clonés, copier-coller) — outils : `jscpd`, `pylint`
     (duplicate-code), `PMD CPD`, ou équivalent du langage ;
   - complexité (cyclomatique / cognitive) — `radon`, `lizard`, `eslint
     complexity`, ou équivalent ;
   - couverture de tests — l'outil du projet ;
   - volume : nombre de fichiers, de fonctions/méthodes, lignes de code.

2. **Comparer à la passe précédente.** Chercher le relevé antérieur (fichier
   `hygiene-log.md` ou équivalent à la racine du projet). Première passe →
   c'est l'état zéro ; le créer.

3. **Recommander les passes de refactorisation ciblées.** À partir des
   mesures, proposer 1 à 3 actions *bornées et vérifiables*, les plus
   rentables d'abord — typiquement : « refactorise toutes les instanciations
   de X en une seule implémentation », « extrais l'utilitaire dupliqué dans
   N fichiers », « ajoute des tests sur le module le moins couvert avant de
   le toucher ». Chaque action recommandée doit être accompagnée de tests qui
   protègent le comportement pendant la refactorisation.

4. **Re-mesurer et tracer la courbe.** Après application, refaire les mesures,
   consigner dans le journal (date, métriques, actions menées), et dire si la
   courbe descend, stagne ou monte. C'est la courbe — pas l'impression — qui
   dit si la dette dégonfle.

Règle du rituel : petit et régulier bat gros et rare. Une passe d'une heure
par semaine qui fait baisser la duplication de 1 % vaut mieux qu'un grand
nettoyage annuel que personne n'ose lancer.
