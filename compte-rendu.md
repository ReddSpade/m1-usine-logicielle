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
Il contient également les métadonnées du dépôt.

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

- `git revert`: Permet de créer un nouveau commit qui supprime les
  changements introduits dans le dernier commit, cela permet de conserver
  l'historique, mais de faire pointer la branche sur un commit valide
- `git reset`: Permet de déplacer le HEAD sur un commit antérieur et (selon l'option)
   supprimer ou unstage les changements des commits entre l'ancien et le nouvel emplacement du HEAD

## Partie 3 — Travailler avec les branches

### Question 5

#### Qu'est-ce qu'un fast-forward merge ? Dans quel cas Git effectue-t-il un fast-forward plutôt qu'un merge commit ?

Un **fast-forward merge** est un merge qui va faire pointer la default branch
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

## Partie 5 - Travail collaboratif avec un dépôt distant

### Question 8

#### Quelle est la différence entre git fetch et git pull ? Dans quel cas préférer l'un à l'autre ?

`git fetch` permet de récupérer les inforations des commits effectués sur le dépôt
sans les appliquer.
`git pull` quant à lui récupère directement les changements pour les appliquer dans le working directory.

Il vaut mieux utiliser git fetch quand il est probable qu'on modifie un composant d'un fichier qui est également modifié sur le dépôt distant.
Cela évite un conflit lors du pull.

### Question 9 

####  Quel est l'intérêt d'utiliser des Pull Requests plutôt que de pousser directement sur main ? Quels éléments vérifiez-vous lors d'une code review ?

Une Pull (ou Merge) Request a pour principal intérêt d'éviter des changements
directs sur la branche principale qui est censé porter la production.
Ce faisant, il devient possible de savoir précisemment qui apporte quel changement,
depuis quelle branche vers quelle branche.
Les éléments à vérifier dans la code review sont:

- Ce que la Pull Request modifie
- Qui effectue les changements
- Quel est le but du changement
- Est-ce que le code respècte les standard de l'entreprise
- Est-ce que le code est fiable et testé

## Partie 6 — .gitignore et bonnes pratiques

### Question 10

#### Pourquoi est-il important de ne pas versionner certains fichiers ? Donnez 3 exemples de fichiers à exclure et expliquez pourquoi pour chacun.

Dans tout type de dépôt, il est possible de retrouver des fichiers n'ayant
un intérêt pour le dépôt local, soit car les fichiers contiennent des
éléments sensibles, soit car ils polluent le dépôt.
C'est pour ça qu'il est mieux de les rajouter au .gitignore.
Par exemple:

- env: Contient souvent des données sensibles en clair qui ne doivent pas finir
  sur le dépôt distant
- caches: Les caches tel que `__pycache__` contiennent des traces d'exécution locale
  du code, ce qui n'apporte rien au dépôt, et peut contenir des éléments sensibles
- logs: Les logs sont des retours après exécution du code, comme les caches, ils
  n'apportent rien au code et peuvent contenir des éléments sensibles

## Partie 7 — Pour aller plus loin

### Question 11 

#### Expliquez dans quelles situations git stash, git bisect et git reflog vous seraient utiles dans un projet réel.

- `git stash` est utile pour par exemple mettre ses changements de côté afin de
  pull du code ou rebaser sa branche, cela permet de ne pas perdre ses changements
  ni de les commiter en attendant
- `git bisect` serait utile dans le cas ou on cherche à savoir à quel moment un
  bug aurait été introduit dans un commit, et de retracer ce dernier
- `git reflog` est pertinent lorsqu'on souhaite connaitre l'histoirque ainsi que les actions
  des commits sur la branche courante pour effecter un reset par exemple
