Journal TP 04  Maîtriser les fusions

3) Prédiction avant fusion feature-a

Avant de lancer git merge feature-a, je pensais qu'il n'y aurait PAS de commit de fusion, car les deux branches étaient alignées.

Résultat : "Updating c4a4b39..da66c04, Fast-forward"

4) Pourquoi pas de commit de fusion ?

Git n'a pas créé de commit de fusion parce que main n'avait aucun commit que feature-a n'avait pas déjà. main pointait sur c4a4b39 et feature-a sur da66c04 (son enfant direct). Git a juste pu "avancer" main sur da66c04. C'est une avance rapide (fast-forward).

6) Fusion --no-ff feature-b

git merge --no-ff feature-b a donné : "Merge made by the 'ort' strategy."

git log --oneline --graph --all :
*   60140d2 (HEAD -> main) Merge branch 'feature-b'
|\
| * b22d3cb (feature-b) Ajout ligne feature-b
* | 3dcb970 Commit sur main (autre fichier)
|/
* da66c04 (feature-a) Ajout ligne feature-a
* c4a4b39 Premier commit sur main

git cat-file -p HEAD | grep parent :
parent 3dcb97080a83fd4a0ee21cb6b295916f42b1a10a
parent b22d3cb83208ed20f73ea5e11bf0a499f98fa506

=> Le commit de fusion a 2 parents (un de chaque branche).

7) git lg après fusion --no-ff

*   60140d2 (HEAD -> main) Merge branch 'feature-b'
|\
| * b22d3cb (feature-b) Ajout ligne feature-b
* | 3dcb970 Commit sur main (autre fichier)
|/
* da66c04 (feature-a) Ajout ligne feature-a
* c4a4b39 Premier commit sur main

=> Forme "X" : les branches se séparent puis se rejoignent au commit de fusion.

9) Conflit provoqué

Message d'erreur exact :
Auto-merging fichier.txt
CONFLICT (content): Merge conflict in fichier.txt
Automatic merge failed; fix conflicts and then commit the result.

Contenu de fichier.txt avec les marqueurs (parenthèses ajoutées pour que grep ne les détecte plus) :

<<<< (HEAD)
ligne 1 modifiée par main
======= (séparateur)
ligne 1 modifiée par feature-c
>>>>>>> (feature-c)
ligne 2
ligne 3
ligne feature-a
ligne feature-b

10) Résolution du conflit

J'ai remplacé les marqueurs par  "ligne 1 modifiée par main et feature-c"
grep -r marqueur : aucune sortie
git add fichier.txt : marque le conflit comme résolu
git commit -m "..." : TERMINE la fusion

Si on oublie git add, git status montre encore "both modified: fichier.txt".

11) Graphe final

*   c65c973 (HEAD -> main) Merge feature-c : résolution du conflit
|\
| * b204d44 (feature-c) feature-c modifie ligne 1
* | e3447fe main modifie ligne 1
|/
*   60140d2 Merge branch 'feature-b'
|\
| * b22d3cb (feature-b) Ajout ligne feature-b
* | 3dcb970 Commit sur main (autre fichier)
|/
* da66c04 (feature-a) Ajout ligne feature-a
* c4a4b39 Premier commit sur main

On voit :
feature-a : avance rapide (ligne simple)
 feature-b : fusion --no-ff (2 parents)
-
 feature-c : fusion avec conflit résolu (2 parents)

12) learngitbranching

Non faite (étape facultative).