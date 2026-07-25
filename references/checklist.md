# Checklist détaillée — contrôle qualité de code

Grille exécutable, point par point. Pour chaque item : la **question à se
poser**, les **signaux d'alerte** typiques (souvent propres au code IA), et la
**cotation** ✅ / ⚠️ / ❌.

---

## NIVEAU 1 — Règles d'entrée

### 1.1 Intention déclarée
- Question : quel *effet* la contribution vise-t-elle ? Est-il énoncé, ou
  faut-il le deviner à partir du diff ?
- Alerte : titre générique (« fix », « update »), description absente ou
  visiblement générée, PR qui « améliore » sans problème identifié.
- Cotation : ❌ si aucune intention lisible → renvoyer sans lire le code.

### 1.2 Propriétaire humain responsable
- Question : quel humain nommé assume ce code et sa reprise ?
- Alerte : « généré par l'agent », aucun mainteneur prêt à répondre aux
  questions, contributeur incapable d'expliquer son propre diff.
- Cotation : ❌ si personne n'assume.

### 1.3 Preuve de comportement
- Question : y a-t-il une reproduction, ou un test rouge → vert ?
- Alerte : « les tests passent » sans nouveau test ; correctif sans cas
  reproduisant le bug ; capture d'écran au lieu d'une preuve exécutable.
- Cotation : ⚠️ si preuve faible, ❌ si aucune preuve pour un correctif de bug.

### 1.4 Coût de revue proportionné
- Question : la taille et la découpe rendent-elles la revue humainement
  possible dans un temps raisonnable ?
- Alerte : méga-PR (milliers de lignes) générée en minutes, mélange de
  refactor + feature + reformatage dans un seul diff.
- Cotation : ⚠️/❌ → demander un découpage avant toute revue de fond.

---

## NIVEAU 2 — Règles de fond

### 2.1 Lisibilité / intention du code
- Question : puis-je lire une fonction et dire *ce qu'elle fait et pourquoi*
  sans exécuter mentalement chaque ligne ?
- Alerte : noms vagues (`data`, `tmp`, `handle`), fonctions trop longues,
  abstractions inutiles, commentaires qui paraphrasent le code au lieu de
  justifier les choix.
- Cotation : ⚠️ si des noms ou structures obscurcissent l'intention par
  endroits ; ❌ si une fonction centrale du diff reste incompréhensible sans
  exécution mentale ligne à ligne.

### 2.2 Absence de duplication
- Question : ce bloc existe-t-il déjà ailleurs ? Aurait-il dû être factorisé ?
- Alerte : copier-coller avec variations minimes, logique répétée dans
  plusieurs fichiers, réimplémentation d'un utilitaire existant. (Défaut n°1
  documenté du code assisté par IA.)
- Cotation : ⚠️ duplication locale facilement factorisable ; ❌ bloc de logique
  métier cloné ou réimplémentation d'un utilitaire déjà présent dans le dépôt.

### 2.3 Justesse vérifiée ≠ tests qui passent
- Question : les tests vérifient-ils le *bon* comportement, ou ont-ils été
  taillés pour passer ?
- Alerte : assertions triviales, valeurs attendues codées en dur qui reflètent
  la sortie observée plutôt que la spécification, tests sans cas négatifs,
  mocks qui masquent la vraie logique. (Le piège « optimiser la mesure ».)
- Cotation : ⚠️ tests présents mais faibles (pas de cas négatifs, assertions
  minces) ; ❌ test taillé pour verdir (assertion recopiant la sortie observée)
  ou comportement central du diff non testé.

### 2.4 API et fonctions réelles
- Question : chaque appel externe existe-t-il réellement dans cette version ?
- Alerte : méthodes/options/paramètres inventés, signatures fausses, imports
  de paquets inexistants, exemples plausibles mais faux (cas curl). Vérifier
  contre la doc/le code réel, pas contre la vraisemblance.
- Cotation : ❌ dès qu'un appel, une option ou un import inexistant est
  confirmé (vérifié contre la doc ou le code, pas l'intuition) ; ⚠️ si l'usage
  est réel mais déprécié ou fragile.

### 2.5 Erreurs et cas limites
- Question : que se passe-t-il hors du chemin heureux ?
- Alerte : pas de gestion d'erreur, entrées nulles/vides ignorées, hypothèses
  implicites (liste non vide, réseau fiable), absence de timeout/retry,
  conditions de course.
- Cotation : ⚠️ bords secondaires négligés ; ❌ chemin d'erreur du flux
  principal absent (entrée nulle, échec réseau) là où le contexte l'exige.

### 2.6 Sécurité, secrets, dépendances
- Question : introduit-on une faille, un secret, une dépendance douteuse ?
- Alerte : secrets/clés en dur, entrées non validées, SQL/commande concaténée,
  dépendance hallucinée (nom inexistant → risque de *slopsquatting*), paquet
  non maintenu ou vulnérable, permissions trop larges.
- Cotation : ❌ secret en dur, injection possible ou dépendance
  inexistante/vulnérable ; ⚠️ validation d'entrée incomplète sans exposition
  directe.

### 2.7 Surface de maintenance
- Question : quel poids de long terme ce code ajoute-t-il ?
- Alerte : nouvelle dépendance lourde pour un petit gain, couplage fort,
  config supplémentaire, chemin de code que personne ne comprendra dans 3 ans.
- Cotation : ⚠️ poids ajouté réel mais assumé et documenté ; ❌ dépendance
  lourde ou couplage fort dont le gain ne justifie pas le coût de long terme.

---

## NIVEAU 3 — Règles de statut

### 3.1 Marquer le jetable comme jetable
- Question : ce code est-il un prototype ? Si oui, est-ce explicite ?
- Alerte : prototype qui glisse en production sans étiquette, « démo » branchée
  sur des données réelles, absence de date de mort ou de plan de reprise.
- Cotation : ⚠️ statut ambigu à clarifier avant fusion ; ❌ prototype branché
  sur de la production sans étiquette ni plan de reprise.

### 3.2 Une porte unique
- Question : ce code a-t-il subi la même revue que n'importe quel autre ?
- Alerte : contournement au motif que « c'est le chef qui l'a écrit » ou « c'est
  l'IA, c'est propre », auto-merge sans relecture.
- Cotation : ❌ si un contournement de revue est constaté (auto-fusion,
  passe-droit) ; ✅ sinon.

### 3.3 Tracer l'origine IA
- Question : sait-on quelle part est générée, pour en suivre le taux de défaut ?
- Alerte : aucune traçabilité, impossibilité de mesurer si le code IA casse
  plus souvent. (Mesurer, pas interdire.)
- Cotation : dépôt sans convention de traçage → consultative, ⚠️ au maximum
  (recommander d'instaurer la convention) ; convention en place → ❌ si du code
  généré n'est pas déclaré. (Voir la calibration dans SKILL.md.)

---

## Modèle de compte rendu de revue

```
VERDICT : ACCEPTER | CORRIGER | REFUSER

Résumé (2-3 lignes) : intention de la contribution + décision.

Bloquants (❌) :
- fichier:ligne — problème — action attendue

À corriger (⚠️) :
- fichier:ligne — problème — suggestion

Réserves mineures / cosmétique :
- ...

Statut : prototype étiqueté ? origine IA tracée ? propriétaire identifié ?
```

Règle d'or : citer des emplacements précis, séparer le bloquant du cosmétique,
et refuser proprement quand il le faut — laisser entrer une dette que quelqu'un
paiera en silence n'est pas un service rendu.
