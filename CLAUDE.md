# Instructions pour un agent travaillant sur ce dépôt

Ce dépôt **est** un skill : son contenu est une grille de revue, pas une application.
Il n'y a rien à compiler et rien à exécuter.

## Ce qui fait foi

`SKILL.md` est la source de vérité. `README.md` n'en est qu'un résumé destiné aux
humains : si les deux divergent, corriger le README, pas la grille.

## Invariants à ne pas casser

1. **Frontmatter.** `SKILL.md` commence par un frontmatter YAML `name` /
   `description` / `when_to_use` / `argument-hint`. C'est ce qui rend le skill
   découvrable par un agent — ne pas le supprimer, ne pas le déplacer.
2. **Copie à l'identique en aval.** `SKILL.md` et `references/` sont vendorés tels
   quels dans `IA-Generative/skills-claude`. Toute modification de forme du
   frontmatter doit rester compatible avec ce dépôt aval (voir `README.md` →
   *Distribution*).
3. **Le skill s'applique à lui-même.** Aucun commit direct sur `main` : branche
   dédiée, trailer `Assisted-by: <agent> (<identifiant exact du modèle>)` sur
   chaque commit, corps de PR conforme à `references/pr-template.md`, jamais
   d'auto-fusion. C'est la règle 3.2 de la grille (« une porte unique »).
4. **Validation des évolutions.** Toute modification de la grille se rejoue contre
   `references/evaluation-scenarios.md` avant d'être proposée : ordre de priorité
   des verdicts respecté, ◻️ jamais transformé en défaut, une demande d'inspection
   ne modifie jamais le dépôt inspecté.

## Langue

Français pour tout le contenu du dépôt. Le compte rendu produit *par* le skill,
lui, suit la langue de l'utilisateur (voir `SKILL.md` → *Routage et langue*).
