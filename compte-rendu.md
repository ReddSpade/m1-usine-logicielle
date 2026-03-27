# Compte Rendu

## Partie 1 - Premiers pas avec Git

### Question 1

#### Que contient le répertoire .git/ ?

Il contient les hooks, la configuration générale, l'URL de la remote, les historiques
de commit et autres.

```
.git
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   ├── sendemail-validate.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```

#### À quoi sert-il ?

Il sert à versionner ses fichiers localement avant de les pousser sur un dépôt
distant.

### Question 2

#### Quelle est la différence entre un fichier untracked, staged et commited ?

- Untracked: Le fichier n'est pas versionné, Git ne vérifie pas ses changements
- Staged: Le fichier a été ajouté grace à un `git add`, il est en **staging area**
  , donc en attente d'un commit
- Commited: Le fichier a été commité grace à un `git commit`, son changement est
  versionné et trackable

### Question 3

#### Quelle est la différence entre git diff et git diff --staged ? À quel moment utiliseriez-vous chacune ?

- `git diff`: Montre le changement entre le **working directory** et la **staging area**
- `git diff --staged`: Montre le changement entre la **staging area** et le dernier commit

On utilise `git diff` quand on veut voir ce qu'on va `git add`
On utilise `git diff --staged` quand on veut voir ce qui va être commité

## Partie 2 - Annuler et corriger

### Question 4

#### Quelle est la différence entre git revert et git reset ? Dans quel cas utiliser l'un ou l'autre ?

- `git revert`: Permet de créer un nouveau commit conservant supprimant les
   changements effectués dnans l'ancien on garde l'ancien commit dans
   l'historique, mais il n'est plus le latest
- `git reset`: Permet de déplacer le HEAD sur un commit antérieur et (selon l'option)
   supprimer ou unstage les changements des commits entre l'ancien et le nouvel emplacement du HEAD

## Partie 3 — Travailler avec les branches

### Question 5

#### Qu'est-ce qu'un fast-forward merge ? Dans quel cas Git effectue-t-il un fast-forward plutôt qu'un merge commit ?

Un **fast-forard merge** est un merge qui va faire pointer la default branch
(main) sur le dernier commit de la branche qu'on souhaite merger.

C'est le comporement attendu quand la branche ciblée n'a pas de conflit (divergence)
avec la branche source. Dans le cas contraire, il faut créer un commit de merge.

### Question 6

#### Pourquoi est-il recommandé de supprimer les branches une fois fusionnées ? Quelle différence entre -d et -D ?

Une branche fusionnée distante est supprimée si l'option est cochée mais reste
présente en locale, afin de nettoyer son repo local et éviter toute confusion
il vaut mieux les supprimer.

- `git branch -d`: Permet de supprimer une branche complètement merged, si elle
  ne l'est pas, un message d'erreur s'affiche
- `git branch -D`: Supprime une branche peu importe son état (comme un -force)

## Partie 4 — Gérer les conflits

### Question 7

#### Décrivez en vos propres mots ce qu'est un conflit Git, pourquoi il survient, et quelles sont les étapes pour le résoudre.

Un conflit Git est un bloquage lors d'un merge, il se présente lorsqu'on tente
de merge deux branches qui modifient les mêmes choses dans un fichier.

Pour le résoudre, il faut inspecter les éléments conflituels, et arbitrer sur
le changement qui doit rester. Ensuite on crée un nouveau commit contenant ce
changement qui est ensuite push.

