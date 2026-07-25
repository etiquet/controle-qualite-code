# Checklist de revue

Utiliser cette grille pour évaluer une contribution. Ne coter que ce qui entre
dans le périmètre de la demande.

## Sommaire

- [Échelle commune](#échelle-commune)
- [Niveau 1 — Entrée et révisabilité](#niveau-1--entrée-et-révisabilité)
- [Niveau 2 — Fond technique](#niveau-2--fond-technique)
- [Niveau 3 — Statut et traçabilité](#niveau-3--statut-et-traçabilité)
- [Format du compte rendu](#format-du-compte-rendu)

## Échelle commune

| Statut | Critère |
|---|---|
| ✅ Conforme | Une preuve accessible et suffisante soutient le critère. |
| ⚠️ Réserve | Une déficience réelle mais non bloquante est établie ; la revue peut continuer. |
| ❌ Bloquant | Un défaut matériel confirmé empêche une fusion sûre. |
| ◻️ Non évalué | La preuve, l'accès, l'environnement ou le contexte manque. Ce statut n'est pas un défaut. |
| N/A | Le critère ne s'applique pas, avec justification. |

Qualifier chaque constat par une sévérité :

- **Critique** : compromission, perte ou corruption de données, contrôle
  d'accès cassé, comportement central faux ou opération irréversible non
  maîtrisée.
- **Majeure** : comportement requis incomplet, régression probable, test
  essentiel absent, API inexistante ou dette importante et immédiate.
- **Mineure** : amélioration locale qui ne bloque pas le comportement attendu.

Indiquer une confiance `haute`, `moyenne` ou `faible` pour un risque inféré.
Une hypothèse à confiance faible appelle une vérification, pas une accusation.
Ne pas employer ⚠️ pour masquer une absence de preuve : utiliser ◻️.

## Niveau 1 — Entrée et révisabilité

### 1.1 Intention

- Rechercher : problème, effet attendu, comportement explicitement hors
  périmètre et critères d'acceptation.
- ✅ si l'effet recherché et les limites permettent de juger le diff.
- ⚠️ si l'effet est clair mais une limite secondaire reste implicite.
- ❌ si les métadonnées accessibles ne contiennent aucun objectif identifiable
  ou présentent des objectifs contradictoires.
- ◻️ si la description, l'issue ou l'ADR n'est pas accessible.

### 1.2 Propriétaire humain

- Rechercher : auteur ou mainteneur humain prêt à répondre du changement et de
  sa reprise.
- ✅ si une personne responsable est explicitement identifiée.
- ⚠️ si un auteur humain est connu mais que sa responsabilité doit être
  confirmée avant fusion.
- ❌ si la contribution déclare qu'aucun humain ne l'assume.
- ◻️ si les métadonnées de contribution ne sont pas accessibles.

Un agent ou un bot ne remplace pas le propriétaire humain.

### 1.3 Preuve de comportement proportionnée

- Rechercher : reproduction rouge → vert pour un bug, test ou démonstration
  exécutable pour une fonctionnalité, non-régression pour une refactorisation,
  rendu ou build pour la documentation, validation adaptée à la configuration.
- ✅ si la preuve couvre le comportement central et les risques du changement.
- ⚠️ si un cas secondaire non critique est réellement omis.
- ❌ si une preuve indispensable est connue comme absente, ou si la preuve
  contredit l'effet annoncé.
- ◻️ si les résultats, l'environnement ou la spécification sont inaccessibles.

Ne pas exiger mécaniquement un nouveau test pour un changement qui ne modifie
aucun comportement exécutable.

### 1.4 Périmètre révisable

- Rechercher : diff cohérent, changements générés isolés, absence de
  reformatage sans rapport, dépendances entre commits ou PR expliquées.
- ✅ si le périmètre peut être compris et testé comme une unité.
- ⚠️ si du bruit identifié ralentit la revue sans masquer l'intention.
- ❌ si le mélange des changements empêche une revue fiable.
- ◻️ si le diff complet ou ses dépendances ne sont pas accessibles.

Une taille élevée est un signal, pas une preuve suffisante de mauvaise qualité.

### Décision après le niveau 1

Si un élément indispensable est ❌ ou ◻️ et empêche une décision sûre, arrêter
la revue approfondie et effectuer le balayage critique décrit dans `SKILL.md`.
Marquer les autres contrôles ◻️, rendre `NON RÉVISABLE — FUSION BLOQUÉE`, puis
lister exactement les informations ou découpages nécessaires pour reprendre.

Si le balayage prouve que l'approche entière est dangereuse et qu'une correction
locale ne suffit pas, appliquer `REFUSER` selon la matrice.

## Niveau 2 — Fond technique

### 2.1 Comportement et justesse

- Vérifier le chemin nominal, les invariants, les sorties, les effets de bord
  et la compatibilité avec l'intention.
- ✅ si les chemins importants sont soutenus par le code et des preuves.
- ⚠️ si un cas secondaire confirmé reste insuffisamment protégé.
- ❌ si un comportement requis est faux, absent ou contredit par les tests.
- ◻️ si la spécification ou l'exécution nécessaire est inaccessible.

### 2.2 Tests et résistance aux faux positifs

- Vérifier que les assertions portent sur les effets attendus, que les cas
  négatifs pertinents existent et que les mocks ne remplacent pas la logique
  testée.
- ✅ si les tests auraient échoué face aux défauts qu'ils prétendent prévenir.
- ⚠️ si un cas non critique est effectivement manquant.
- ❌ si les tests verdissent sans exercer l'intention centrale, sont désactivés
  ou masquent une régression confirmée.
- ◻️ si les tests ne peuvent pas être lus ou exécutés.

### 2.3 Interfaces, API et dépendances réelles

- Vérifier les signatures et options contre le code, le verrou de dépendances
  ou la documentation de la version utilisée.
- ✅ si chaque interface nouvelle ou modifiée est vérifiée.
- ⚠️ si une compatibilité secondaire est prouvée partiellement mais n'empêche
  pas le comportement central.
- ❌ si un appel, paquet, paramètre ou comportement d'API est confirmé
  inexistant ou incompatible.
- ◻️ si la version ou la source d'autorité est inaccessible.

Ne pas qualifier d'hallucination un paquet simplement inconnu ou privé.

### 2.4 Erreurs, limites et concurrence

- Vérifier les entrées vides ou extrêmes, timeouts, annulations, erreurs
  réseau, reprises partielles, idempotence, concurrence et nettoyage.
- ✅ si les cas pertinents au risque sont gérés ou explicitement exclus.
- ⚠️ si un cas plausible non critique est réellement non couvert.
- ❌ si un cas réaliste provoque perte de données, incohérence ou échec du
  comportement central.
- ◻️ si l'environnement nécessaire n'est pas disponible.

### 2.5 Sécurité, secrets et chaîne d'approvisionnement

- Examiner les contrôles d'accès, validations d'entrée, injections, secrets,
  permissions, journaux sensibles, nouvelles dépendances et migrations.
- ✅ si aucun risque pertinent n'est trouvé dans le périmètre inspecté et que
  les protections attendues sont visibles.
- ⚠️ si une faiblesse confirmée de défense en profondeur a un impact borné.
- ❌ si une vulnérabilité exploitable, un secret, une permission dangereuse ou
  une dépendance trompeuse est confirmé.
- ◻️ si le modèle de menace ou les données nécessaires sont inaccessibles.

Ne pas affirmer qu'une dépendance est vulnérable sans source ou résultat
vérifiable.

### 2.6 Lisibilité et duplication

- Vérifier nommage, cohésion, flux de contrôle, commentaires de décision,
  duplication de connaissance et abstractions déjà disponibles.
- ✅ si l'intention locale reste compréhensible et les répétitions justifiées.
- ⚠️ si la complexité ou duplication est localisée et réparable.
- ❌ si elle masque un comportement divergent, multiplie un correctif critique
  ou empêche d'établir la justesse.
- ◻️ si le contexte voisin nécessaire n'est pas accessible.

### 2.7 Surface de maintenance

- Évaluer dépendances, configuration, observabilité, compatibilité, chemins de
  reprise et coût durable par rapport au bénéfice annoncé.
- ✅ si la surface ajoutée est proportionnée et possédée.
- ⚠️ si une simplification bornée réduirait sensiblement le coût.
- ❌ si le changement impose une charge durable majeure sans bénéfice ni
  propriétaire identifiable.
- ◻️ si les contraintes d'exploitation sont inconnues.

## Niveau 3 — Statut et traçabilité

### 3.1 Durable ou jetable

- ✅ si le statut est clair ; pour un prototype, inclure limites, propriétaire
  et condition ou date de retrait.
- ⚠️ si une échéance ou condition de sortie secondaire reste à préciser.
- ❌ si un prototype connu comme dangereux est destiné à devenir une fondation
  implicite.
- ◻️ si la destination du code est inconnue.

### 3.2 Processus de revue

- ✅ si la contribution suit les protections normales du projet.
- ⚠️ si une exception limitée est documentée et compensée.
- ❌ si une protection obligatoire est sciemment contournée.
- ◻️ si les règles ou paramètres du dépôt sont inaccessibles.

### 3.3 Assistance d'un agent

- ✅ si l'assistance connue ou exigée est tracée avec l'agent réel, son rôle,
  son périmètre et ses validations ; conserver l'humain responsable séparé.
- ⚠️ si une assistance connue est déclarée mais son rôle ou périmètre reste
  incomplet.
- ❌ si une politique explicite exige la déclaration et que l'assistance connue
  est sciemment dissimulée ou faussement attribuée.
- ◻️ si l'usage de l'IA ou la politique ne peut pas être établi.
- N/A si aucune assistance n'est connue et qu'aucune politique ne l'exige.

Suivre la convention du projet. `Assisted-by:` est notamment documenté pour les
contributions au noyau Linux, mais n'est pas une norme universelle :
<https://www.kernel.org/doc/html/next/process/coding-assistants.html>.

## Format du compte rendu

```markdown
VERDICT : NON RÉVISABLE | ACCEPTER | CORRIGER | REFUSER

Résumé : intention, périmètre examiné et décision.

Preuves consultées :
- ...

Balayage critique :
- résultat, y compris lorsque la revue est non révisable

Bloquants :
- [sévérité] source:emplacement — observation — impact — action attendue

Réserves et risques :
- [sévérité, confiance] source:emplacement — risque — vérification attendue

Non évalué / non applicable :
- élément — statut — raison — information nécessaire

Statut et traçabilité :
- durable/jetable — propriétaire humain — assistance connue

Validations exécutées :
- commande — résultat, ou « non exécutée » avec raison
```
