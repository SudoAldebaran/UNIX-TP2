# UNIX-TP2
Deuxieme TP en cours d'UNIX

# 1 - Secure Shell : SSH

## 1.1 Exercice : connexion ssh root

Dans `sshd_config`, l'élément à configurer est PermitRootLogin

Les différentes options sont : 

> no

Les connexions SSH pour l'utilisateur root sont complètement interdites

Avantages :

- améliorer la sécurité en empechant les attaques par force brute sur le compte root
- permettre d'utiliser des comptes non privilégiés, ce qui est une bonne pratique de sécurité

Inconvénients :

- nécessite une connexion via un autre utilisateur, ce qui peut ajouter des étapes supplémentaires pour les admin qui doivent exécuter des commandes root.

Utilisation : Utilisé dans les environnements de production pour réduire le risque d'accès non autorisé

> yes

Les connexions ssh root sont autorisées avec un mot de passe

Avantages :

Pratique pour l'administration à distance, car les admin peuvent se connecter directement en tant que root

Inconvénients :

- augmente le risque de piratage, un attaquant peut essayer de deviner le mot de passe root
- expose le système à des attaques par force brute

Utilisation : Utilisé dans des environnements de test ou de développement où l'accès rapide à root est nécessaire, mais pas recommandé en production

> prohibit-password

Les connexions ssh root sont interdites via un mot de passe, mais autorisées avec des clefs ssh

Avantages :

- permet un accès sécurisé via des clefs, réduit donc le risque d'infiltration tout en étant flexible via l'authentification avec clefs

Inconvénients :

- nécessite la gestion des clefs ssh, ce qui peut être compliqué pour certains utilisateurs

Utilisation : Recommandé pour les environnements de production où la sécurité est primordiale, mais où un accès root est nécessaire

## 1.2 Exercice : authentification par clef / génération de clefs


### Génération des clefs publique et privée

>> ssh-keygen 

Les clefs ont bien été créee :

Emplacement de la clef privée :

Your identification has been saved in /Users/rayan/.ssh/id_ed25519
Your public key has been saved in /Users/rayan/.ssh/id_ed25519.pub

### Copie de la clef publique sur le serveur distant

>> ssh-copy-id root@172.16.233.130

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/rayan/.ssh/id_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@172.16.233.130's password: 

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'root@172.16.233.130'"
and check to make sure that only the key(s) you wanted were added.

Après la saisie du mot de passe, la clef a bien été transferée dans /root/.ssh/authorized_keys sur le serveur distant

Desormais, je suis le seul (ordinateur) à pouvoir me connecter au serveur, étant le seul à détenir la clef privée

Lors de la tentative de connexion, ssh recherche et essaie toutes les clefs présentes sur la machine locale :


debug1: Offering public key: /Users/rayan/.ssh/id_ed25519 ED25519 SHA256:Zk1jY8AoCwjdlBeKw30lkRpFSH0EDeC7M27ZqgjrR1I
debug1: Server accepts key: /Users/rayan/.ssh/id_ed25519 ED25519 SHA256:Zk1jY8AoCwjdlBeKw30lkRpFSH0EDeC7M27ZqgjrR1I
Authenticated to 172.16.233.130 ([172.16.233.130]:22) using "publickey".

Ci-dessus, le serveur a accepté la clef correspondante lorsque celle-ci a été envoyée.


