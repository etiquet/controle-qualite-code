# controle-qualite-code

Un *skill* de contrôle qualité du code à l'ère de l'IA — et une **expérimentation ouverte**.

> Écrire du code ne coûte presque plus rien. Le comprendre, de moins en moins. Ce qui reste cher, c'est le jugement : décider ce qui mérite d'exister, et en répondre.

Ce dépôt accompagne l'article *« Maintenir du code : l'IA nous force-t-elle à désapprendre ? »* (à paraître sur [erictiquet.substack.com](https://erictiquet.substack.com)), écrit à quatre mains dans le prolongement de [*Coder en chef*](https://erictiquet.substack.com/p/coder-en-chef).

## Pourquoi ce skill

Le code généré par IA passe la démo, les tests verdissent — et il peut emporter sa dette en soute : duplication, cas limites négligés, tests taillés pour passer, statut jamais tranché. Le danger n'est pas le code qui échoue (il s'élimine tout seul), c'est **le code qui marche presque**.

Ce skill donne à un agent IA — ou à un humain — une grille pour trouver le *presque* efficacement. Il fait deux choses :

**Mode revue** — appliquer trois niveaux de règles à toute contribution, dans l'ordre :

1. *Règles d'entrée* (avant même de lire le code) : intention déclarée, propriétaire humain, preuve de comportement, coût de revue proportionné ;
2. *Règles de fond* (dans le code) : lisibilité, duplication, justesse vérifiée (≠ « les tests passent »), API réelles, cas limites, sécurité, surface de maintenance ;
3. *Règles de statut* : marquer le jetable comme jetable, une porte unique (la règle du noyau Linux), tracer l'origine IA.

Et rendre un **verdict motivé** : `ACCEPTER` / `CORRIGER` / `REFUSER` — pas un avis stylistique.

**Mode rituel** — dégonfler la dette existante par passes d'hygiène régulières : mesurer (duplication, complexité, couverture), recommander 1 à 3 refactorisations bornées avec tests de protection, re-mesurer, **tracer la courbe** dans un journal `hygiene-log.md`.

## L'expérimentation

Ce dépôt n'est pas une norme : c'est un protocole à rejouer, et une hypothèse à tester.

**Hypothèse.** Si cette grille tourne *en rituel* — chaque semaine, pas une fois à la fusion — alors la qualité du code généré cesse d'être une fatalité pour devenir une courbe qu'on pilote.

**Protocole.**

1. Mesurer l'état zéro d'une base : duplication, complexité, couverture de tests ;
2. Faire tourner le rituel quelques semaines (une passe hebdomadaire suffit) ;
3. Re-mesurer, et publier la courbe.

**Falsifiable, et c'est voulu.** Si la courbe descend, la « bombe à retardement » du code généré n'était pas une fatalité : c'était l'absence de boucle. Si elle ne descend pas, nous l'écrirons aussi.

## Contenu

- [`SKILL.md`](SKILL.md) — la grille complète : principe directeur, les trois niveaux, le verdict, le mode rituel ;
- [`references/checklist.md`](references/checklist.md) — la checklist détaillée, point par point : question à se poser, signaux d'alerte, cotation ✅ / ⚠️ / ❌, et modèle de compte rendu.

## Installation

**Claude (application).** Empaquetez le skill et ajoutez-le à vos skills :

```bash
zip -r controle-qualite-code.skill SKILL.md references
```

**Claude Code.** Copiez le dossier dans vos skills locaux :

```bash
git clone https://github.com/etiquet/controle-qualite-code ~/.claude/skills/controle-qualite-code
```

**Autres agents / usage manuel.** `SKILL.md` est un simple document d'instructions en Markdown : collez-le dans le contexte de votre agent, ou imprimez la checklist et appliquez-la vous-même.

## Utilisation

Quelques prompts types une fois le skill installé :

- « Applique la grille controle-qualite-code à cette pull request. »
- « Revue de ce diff avec le skill : verdict motivé. »
- « Lance la passe d'hygiène hebdomadaire sur ce dépôt et mets à jour hygiene-log.md. »

## Contribuer

Les contributions sont bienvenues — c'est le but de l'expérimentation. Trois façons d'aider :

1. **Publier vos courbes.** Ouvrez une issue avec vos métriques avant/après (duplication, complexité, couverture, durée du rituel). Les résultats négatifs sont aussi précieux que les positifs ;
2. **Améliorer la grille.** Une règle manque, une règle est trop stricte, un signal d'alerte est daté : proposez la modification ;
3. **Rapporter les erreurs de la grille.** Les cas où le verdict était mauvais (faux REFUSER, faux ACCEPTER) sont les plus instructifs.

**Règle du jeu** — en cohérence avec la grille elle-même : toute pull request arrive avec son *intention déclarée*, un *propriétaire humain* qui l'assume, et une *preuve de comportement*. Niveau 1 oblige : ce dépôt applique son propre skill à ses contributions. Le code ou le texte assisté par IA est bienvenu, déclaré (à la manière du tag `Assisted-by` du noyau Linux), et l'humain qui soumet en répond.

## Statut

Expérimentation — v0.2, juillet 2026. Voir [LICENSE](LICENSE).
