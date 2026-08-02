# controle-qualite-code

Un *skill* de contrôle qualité du code à l'ère de l'IA — et une **expérimentation ouverte**.

> Écrire du code ne coûte presque plus rien. Le comprendre, de moins en moins. Ce qui reste cher, c'est le jugement : décider ce qui mérite d'exister, et en répondre.

Ce dépôt accompagne l'article *« Maintenir du code : l'IA nous force-t-elle à désapprendre ? »* (à paraître sur [erictiquet.substack.com](https://erictiquet.substack.com)), écrit à quatre mains dans le prolongement de [*Coder en chef*](https://erictiquet.substack.com/p/coder-en-chef).

**Ce dépôt est la source de vérité du skill.** Il est redistribué en interne à IA-Generative via le dépôt de skills partagées `IA-Generative/skills-claude` — voir [Distribution](#distribution).

## Pourquoi ce skill

Le code généré par IA passe la démo, les tests verdissent — et il peut emporter sa dette en soute : duplication, cas limites négligés, tests taillés pour passer, statut jamais tranché. Le danger n'est pas le code qui échoue (il s'élimine tout seul), c'est **le code qui marche presque**.

Ce skill donne à un agent IA — ou à un humain — une grille pour trouver le *presque* efficacement. Il fait trois choses :

| | |
|---|---|
| **Mode revue** | Trois niveaux de règles appliqués dans l'ordre (entrée, fond, statut) et un **verdict motivé** `NON RÉVISABLE` / `ACCEPTER` / `CORRIGER` / `REFUSER` — pas un avis stylistique. |
| **Mode rituel** | Des passes d'hygiène régulières et outillées : mesurer (duplication, complexité, couverture), refactoriser sous protection de tests, re-mesurer, **tracer la courbe** dans un journal `hygiene-log.md`. |
| **Livraison traçée** | Tout changement produit par le skill part dans une PR qui déclare son intention, son propriétaire humain, sa preuve de comportement, et **identifie l'agent qui l'a produite** (trailer `Assisted-by`). |

## Contenu du dépôt

| Fichier | Rôle |
|---|---|
| [`SKILL.md`](SKILL.md) | **Source de vérité.** La grille complète : principe directeur, les trois niveaux, le verdict, le mode rituel, la règle de livraison. |
| [`references/checklist.md`](references/checklist.md) | La checklist point par point : question à se poser, signaux d'alerte, cotation ✅ / ⚠️ / ❌ / ◻️, modèle de compte rendu. Chargée pour une revue complète. |
| [`references/pr-template.md`](references/pr-template.md) | Le modèle de corps de PR pour les changements produits par le skill. |
| [`references/hygiene-log-format.md`](references/hygiene-log-format.md) | Le format du journal d'hygiène, pour des mesures comparables d'une passe à l'autre. |
| [`references/evaluation-scenarios.md`](references/evaluation-scenarios.md) | Les scénarios qui valident toute évolution du skill lui-même. |

Ce README n'est qu'un résumé : en cas de désaccord, `SKILL.md` fait foi.

## Installation

**Claude Code.** Clonez et liez le dossier dans vos skills utilisateur — disponible dans tous vos projets :

```bash
git clone https://github.com/etiquet/controle-qualite-code ~/code/controle-qualite-code
ln -s ~/code/controle-qualite-code ~/.claude/skills/controle-qualite-code
```

**Claude (application).** Empaquetez le skill et ajoutez-le à vos skills :

```bash
zip -r controle-qualite-code.skill SKILL.md references
```

**Autres agents / usage manuel.** `SKILL.md` est un simple document d'instructions en Markdown : collez-le dans le contexte de votre agent, ou imprimez la checklist et appliquez-la vous-même.

## Utilisation

Quelques prompts types une fois le skill installé :

- « Applique la grille controle-qualite-code à cette pull request. »
- « Revue de ce diff avec le skill : verdict motivé. »
- « Lance la passe d'hygiène hebdomadaire sur ce dépôt et mets à jour `hygiene-log.md`. »

## Distribution

Ce dépôt est **l'amont**. Le skill est vendoré en aval dans `IA-Generative/skills-claude`
(dépôt interne de skills partagées, installées par symlink dans `~/.claude/skills/`).

**Règle :** on édite ici, jamais dans la copie. Toute évolution du skill se fait
dans ce dépôt, puis se propage en aval par une synchronisation à l'identique —
la copie vendorée ne doit jamais diverger. La procédure exacte est décrite dans
`skills/controle-qualite-code/UPSTREAM.md` du dépôt aval.

Une contribution externe arrive donc ici, sur ce dépôt public.

## L'expérimentation

Ce dépôt n'est pas une norme : c'est un protocole à rejouer, et une hypothèse à tester.

**Hypothèse.** Si cette grille tourne *en rituel* — chaque semaine, pas une fois à la fusion — alors la qualité du code généré cesse d'être une fatalité pour devenir une courbe qu'on pilote.

**Protocole.**

1. Mesurer l'état zéro d'une base : duplication, complexité, couverture de tests ;
2. Faire tourner le rituel quelques semaines (une passe hebdomadaire suffit) ;
3. Re-mesurer, et publier la courbe.

**Falsifiable, et c'est voulu.** Si la courbe descend, la « bombe à retardement » du code généré n'était pas une fatalité : c'était l'absence de boucle. Si elle ne descend pas, nous l'écrirons aussi.

## Contribuer

Les contributions sont bienvenues — c'est le but de l'expérimentation. Trois façons d'aider :

1. **Publier vos courbes.** Ouvrez une issue avec vos métriques avant/après (duplication, complexité, couverture, durée du rituel). Les résultats négatifs sont aussi précieux que les positifs ;
2. **Améliorer la grille.** Une règle manque, une règle est trop stricte, un signal d'alerte est daté : proposez la modification ;
3. **Rapporter les erreurs de la grille.** Les cas où le verdict était mauvais (faux REFUSER, faux ACCEPTER) sont les plus instructifs.

**Règle du jeu** — en cohérence avec la grille elle-même : toute pull request arrive avec son *intention déclarée*, un *propriétaire humain* qui l'assume, et une *preuve de comportement*. Niveau 1 oblige : ce dépôt applique son propre skill à ses contributions. Le code ou le texte assisté par IA est bienvenu, déclaré, et l'humain qui soumet en répond. Les PR produites par un agent suivent le modèle [`references/pr-template.md`](references/pr-template.md) : le trailer `Assisted-by: <agent> (<modèle>)` figure dans les commits et en tête de la PR.

Toute modification du skill lui-même se valide contre [`references/evaluation-scenarios.md`](references/evaluation-scenarios.md).

## Statut

Expérimentation — v0.3, juillet 2026. Voir [LICENSE](LICENSE).
