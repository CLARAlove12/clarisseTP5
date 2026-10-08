Journal TP 05  Git distant avec GitHub

3)Signification de origin/main et de -u

Dans git branch -vv, la ligne de main affiche [origin/main]. Cela veut dire que ma branche locale main suit la branche distante origin/main. Ce lien sert a git push et git pull pour savoir ou envoyer et ou recuperer sans que je precise origin main a chaque fois.

L'option -u (set-upstream) sert a creer ce lien. On la met seulement la premiere fois qu'on pousse une branche, parce qu'apres le lien est enregistre dans la config locale du depot. Le remettre a chaque fois ne change rien.

4)Authentification SSH et HTTPS

J'ai deux remotes vers le meme depot :

origin en SSH : git@github.com:CLARAlove12/clarisseTP5.git
https-origin en HTTPS : https://github.com/CLARAlove12/clarisseTP5.git

SSH demande une paire de cles (cle privee sur ma machine, cle publique sur GitHub). Il n'y a pas de mot de passe a taper a chaque push.

HTTPS demande un token personnel (PAT) ou un gestionnaire d'identifiants. GitHub n'accepte plus le mot de passe du compte depuis 2021.

7) Prediction et resultat du git pull

Prediction : je pense que ca va etre une avance rapide (fast-forward). Car git status me dit "can be fast-forwarded" et mon local n'a aucun commit en avance sur le distant.

Resultat reel : Fast-forward, exactement comme prevu. Pas d'ecart entre prediction et resultat.

8)git fetch seul et git pull

Apres git fetch seul : le nouveau commit est telecharge dans mon depot local, mais il se trouve uniquement dans la branche de suivi origin/main. Ma branche main ne bouge pas, et mes fichiers dans le dossier ne changent pas non plus.

Apres git pull : fetch plus integration. Le commit arrive dans origin/main et aussi dans main, et mes fichiers sont mis a jour.

Donc git pull = git fetch + git merge (ou rebase selon la config).

Preuve avec git log --oneline --all --graph : juste apres fetch on voit main et origin/main sur deux lignes differentes, origin/main en avance. Apres pull tout est aligne sur le meme commit.

10)Message de rejet verbatim

To github.com:CLARAlove12/clarisseTP5.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:CLARAlove12/clarisseTP5.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

11)force-with-lease et force

Ce que force-with-lease verifie en plus de force : force-with-lease regarde si la branche distante est toujours au commit que je connaissais au dernier fetch. Si quelqu'un a pousse entre temps, la commande refuse. Force de son cote ecrase sans rien verifier.

Scenario d'equipe protege : Alice force-push une version corrigee. Bob n'a pas vu la modif et force-push a son tour. Avec force, Bob efface le travail d'Alice sans le savoir. Avec force-with-lease, Bob est bloque parce que son git voit que la branche distante a bouge depuis son dernier fetch. Il doit d'abord recuperer le travail d'Alice.

14)Interet du lien Issue et commit pour une revue trois mois plus tard

Trois mois apres, un commit tout seul ne raconte que le quoi. Il ne dit pas pourquoi la modif a ete faite, ni quel besoin elle couvrait.

Avec le lien Closes #1, on peut remonter du commit vers l'Issue. L'Issue contient la description du besoin et les echanges. Le relecteur comprend l'intention du changement, pas seulement son contenu technique. Ca evite de refaire le meme travail ou de defaire une decision sans connaitre la raison.

## Livrable - URL du depot

https://github.com/CLARAlove12/clarisseTP5
Issue #1 fermee par le commit 0cc4ff2.