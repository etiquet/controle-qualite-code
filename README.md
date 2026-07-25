# controle-qualite-code

Un *skill* de contrôle qualité du code à l'ère de l'IA — et une **expérimentation ouverte**.

> Écrire du code ne coûte presque plus rien. Le comprendre, de moins en moins. Ce qui reste cher, c'est le jugement : décider ce qui mérite d'exister, et en répondre.

Ce dépôt accompagne l'article *« Maintenir du code : l'IA nous force-t-elle à désapprendre ? »* (à paraître sur [erictiquet.substack.com](https://erictiquet.substack.com)), écrit à quatre mains dans le prolongement de [*Coder en chef*](https://erictiquet.substack.com/p/coder-en-chef).

## Pourquoi ce skill

Le code généré par IA passe la démo, les tests verdissent — et il peut emporter sa dette en soute : duplication, cas limites négligés, tests taillés pour passer, statut jamais tranché. Le danger n'est pas le code qui échoue (il s'élimine tout seul), c'est **le code qui marche presque**.

Ce skill donne à un agent IA — ou à un humain — une grille pour trouver le *presque* efficacement. Il fait deux choses :

**Mode revue** — appliquer trois niveaux de règles à toute contribution :

1. *Règles d'entrée* : intention déclarée, propriétaire humain, preuve de comportement proportionnée, périmètre révisable ;
2. *Règles de fond* (dans le code) : lisibilité, duplication, justesse vérifiée (≠ « les tests passent »), API réelles, cas limites, sécurité, surface de maintenance ;
3. *Règles de statut* : marquer le jetable comme jetable, appliquer les mêmes protections à toute contribution, tracer l'origine IA.

Et rendre un **verdict motivé** : `NON RÉVISABLE` / `ACCEPTER` / `CORRIGER` / `REFUSER` — pas un avis stylistique. Une information inaccessible est marquée comme non évaluée, jamais transformée en faute. Même si une contribution n'est pas révisable, un balayage minimal cherche les risques critiques évidents.

**Mode rituel** — dégonfler la dette existante par passes d'hygiène régulières : mesurer (duplication, complexité, couverture), recommander 1 à 3 refactorisations bornées avec tests de protection, re-mesurer, **tracer des séries comparables**. Le mode reste en lecture seule par défaut ; le journal et le code ne changent que sur demande explicite.

## L'expérimentation

Ce dépôt n'est pas une norme : c'est un protocole à rejouer, et une hypothèse à tester.

**Hypothèse.** Si cette grille tourne *en rituel* — chaque semaine, pas une fois à la fusion — alors la qualité du code généré cesse d'être une fatalité pour devenir une courbe qu'on pilote.

**Protocole.**

1. Mesurer l'état zéro d'une base avec un périmètre, des outils et des versions enregistrés ;
2. Faire tourner le rituel quelques semaines (une passe hebdomadaire suffit) ;
3. Reprendre la même méthode, vérifier la comparabilité, et publier les courbes.

**Falsifiable, et c'est voulu.** Si la courbe descend, la « bombe à retardement » du code généré n'était pas une fatalité : c'était l'absence de boucle. Si elle ne descend pas, nous l'écrirons aussi.

## Contenu

- [`SKILL.md`](SKILL.md) — le contrat d'exécution, les deux modes et la matrice de verdict ;
- [`references/checklist.md`](references/checklist.md) — la grille de revue avec critères ✅ / ⚠️ / ❌ / non évalué / non applicable ;
- [`references/hygiene.md`](references/hygiene.md) — le protocole reproductible de mesure et de comparaison ;
- [`references/evaluation-scenarios.md`](references/evaluation-scenarios.md) — dix scénarios pour vérifier les décisions du skill ;
- [`agents/openai.yaml`](agents/openai.yaml) — les métadonnées d'interface Codex.

## Installation

**Claude (application).** Empaquetez le skill et ajoutez-le à vos skills :

```bash
zip -r controle-qualite-code.skill SKILL.md references
```

**Claude Code.** Copiez le dossier dans vos skills locaux :

```bash
git clone https://github.com/etiquet/controle-qualite-code ~/.claude/skills/controle-qualite-code
```

**Codex.** Clonez le dépôt dans le répertoire des skills :

```bash
git clone https://github.com/etiquet/controle-qualite-code ~/.codex/skills/controle-qualite-code
```

**Autres agents / usage manuel.** `SKILL.md` est un simple document d'instructions en Markdown : collez-le dans le contexte de votre agent, ou imprimez la checklist et appliquez-la vous-même.

## Utilisation

Quelques prompts types une fois le skill installé :

- « Applique la grille controle-qualite-code à cette pull request. »
- « Revue de ce diff avec le skill : verdict motivé. »
- « Audite ce dépôt en lecture seule et signale ce que tu ne peux pas évaluer. »
- « Lance la passe d'hygiène hebdomadaire sur ce dépôt et mets à jour hygiene-log.md. »

## Contribuer

Les contributions sont bienvenues — c'est le but de l'expérimentation. Trois façons d'aider :

1. **Publier vos courbes.** Ouvrez une issue avec vos métriques avant/après (duplication, complexité, couverture, durée du rituel). Les résultats négatifs sont aussi précieux que les positifs ;
2. **Améliorer la grille.** Une règle manque, une règle est trop stricte, un signal d'alerte est daté : proposez la modification ;
3. **Rapporter les erreurs de la grille.** Les cas où le verdict était mauvais (faux REFUSER, faux ACCEPTER) sont les plus instructifs.

**Règle du jeu** — en cohérence avec la grille elle-même : toute pull request arrive avec son *intention déclarée*, un *propriétaire humain* qui l'assume, et une *preuve de comportement proportionnée*. Ce dépôt applique son propre skill à ses contributions. Le code ou le texte assisté par IA est bienvenu et déclaré ; l'identité réelle de l'agent, son rôle et son périmètre sont distingués de l'humain qui soumet et en répond. Le trailer `Assisted-by:` suit une convention également [documentée pour le noyau Linux](https://www.kernel.org/doc/html/next/process/coding-assistants.html), sans prétendre qu'elle soit universelle.

## Statut

Expérimentation — v0.3, juillet 2026. Voir [LICENSE](LICENSE).
