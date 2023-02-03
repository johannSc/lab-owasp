# lab-owasp

## Pré-requis

### Outils

Pour installer le lab, nous allons avoir besoin à minima des ressources suivantes:

* Kali linux dans sa version 2022, disponible ici: https://www.kali.org/get-kali/
* Une autre distribution cible, je prends pour ma part une Debian11 disponible ici: https://www.debian.org/distrib/index.fr.html
* VirtualBox 7 pour la machine hôte, disponible ici: https://www.virtualbox.org/wiki/Downloads

-> attention pour les détenteurs des macbook dernière génération, c'est du processeur ARM...

### Installations

Déployer virtualbox sur la machine hôte, j'ai indiqué sous VB les ressources suivantes:

* 2 Go de RAM
* 1 processeur
* 25 Go espace disque


Puis déploiement de Kali Linux (via l'ISO pour choisir les paramètres géographiques, sinon j'ai choisi tous les paramètres par défaut)

### Préparation du TP

Première chose à faire, c'est l'installation des VBoxGuestAdditions, pour ceci, sous VirtualBox (une fois Kali démarrée)
Onglet _Périphériques_, puis _Insérer l'image cd_..

Ensuite dans Kali, ouvrez un terminal:

```
sudo su
cd /media/cdrom0
sh VBoxLinuxAdditions.run
```
Tapez ensuite _yes_, puis redémarrer l'OS. Une fois les outils additionnels installés, le redimensionnement de l'écran, le partage de fichiers...seront disponibles.

On enchaine sur la mise à jour du système et des dépôts associés depuis le terminal fraichement lancé sous Kali:

```
apt update
apt upgrade -y
```

## Juice Shope

Juice shop est une boutique en ligne trouée de vulnérabilités . Elle permet de s’exercer au pentest web et contient l’ensemble du top 10 des vulnérabilités de l’OWASP (https://owasp.org/Top10/fr/)

La dernière version de Kali gère les dépôts de l'outil à partir de sa version 2022, on va donc simplement l'installer:

```
apt install juice-shop
```

L'outil est accessible: http://localhost:3000

### Echauffement

Pour pouvoir utiliser l'outil, il y a des challenges d'échauffement. Les réponses sont apportées dans ce TP, tentez d'abord de chercher :)

Première étape:

* trouver l’interface de scoreboard de juiceshop. Pour cela, regardez du coté du code du site (main.js).

* Nous pouvons également voir une page d’administration, mais nous n’avons pas les droits suffisant pour y accéder 

```
403 Forbidden
```

Seconde étape:

||On peut ensuite se rendre sur l’URL http://ip_juice_shop:3000/score-board/ . Nous pouvons voir la réussite du premier challenge||

Bien nous allons nous échauffer encore un peu avec la réalisation d’une simple XSS Reflected . Le challenge XSS Tier 0 sur l’image ci-dessus demande la réalisation d’une attaque XSS . Pour cela il faut taper <iframe src=”javascript:alert(`xss`)”> dans la barre de recherche de la boutique et nous réalisons une attaque de type XSS Reflected. Ce qui se traduit par : Ouvrir dans la page actuelle un popup avec le texte “xss”. Un pirate mettra bien évidemment un autre code .

Une XSS permet à un attaquant d’injecter un script dans le contenu d’un site Web ou d’une application. Lorsqu’un utilisateur visite la page infectée, le script s’exécute dans le navigateur de la victime.

Une XSS reflected (ou également appelé attaque XSS non persistante) est un type spécifique de XSS. Elle est transmise dans la requête, généralement dans l’URL. Comparé au XSS stocké, le XSS reflected nécessite que le script malveillant soit ajouté à un lien et qu’un utilisateur clique dessus.

Dernier petit échauffement avant d’attaquer les challenges de niveaux 2 et 3.
Nous allons nous inscrire sur la boutique avec l’adresse mail azerty@msn.com et le mot de passe 12345 en ne respectant pas les condition de mot de passe du site . Je me rends sur la page d’inscription et remplis les différents champs demandés.
Lors de l’inscription, si le deuxième mot de passe n’est pas identique au premier mot de passe saisi, nous aurons une erreur.

    « Les mots de passes ne correspondent pas »

Ce qui est logique . Je saisis donc 12345 en deuxième mot de passe . Avant de valider, je remodifie le premier mot de passe entrée par 12349 sans toucher au deuxième mot de passe. Puis je valide l’inscription.

Aucune erreur n’est remontée et le challenge est validé . J’ai volontairement modifié le code pour passer le type des “passwords” en “text” pour que vous puissiez voir les valeurs avant la validation du formulaire.

### Easy Challenge

Un autre défi où nous devons nous connecter en tant qu’administrateurs sur le site juiceshop.

Nous utiliserons l’injection SQL pour réaliser ce défi. L’injection SQL est une méthode d’attaque très connue. C’est un vecteur d’attaque extrêmement puissant quand il est bien exploité. Il consiste à modifier une requête SQL en injectant des morceaux de code non filtrés, généralement par le biais d’un formulaire.

Il faut se rendre dans le formulaire de connexion au site . Nous utiliserons ici en login une instruction basique toujours vraie ‘ or 1=1– et un mot de passe pris au hasard. Cette instruction authentifiera la première personne dans la table Users (car la condition est vraie)qui se trouve être par hasard et par chance l’administrateur.

Au passage nous pouvons maintenant accéder à l’interface d’administration repérer dans la partie échauffement . Nous récupérons beaucoup d’informations comme les adresses mail des utilisateurs inscrits, leurs ID sur le site, etc..

Le dernier défi que j’ai apprécié et une attaque XSS sur une entrée filtrée par le site web .

Étant connecté en tant qu’administrateur, j’ai modifié son profil . En username, j’ai entré la même XSS que tester au-dessus
<script>alert(`computer-security.fr`)</script> . On remarque sous la photo de profil que le nom d’utilisateur est maintenant lert(`computer-security.fr`) ce qui indique que l’entrée username est filtrée. Ici le début de l’entrée est filtré.

Après plusieurs tentatives, j’ai entré l’XSS suivante
<<a|ascript>alert(`computer-security.fr`)</script> pour valider le challenge. Le filtre mis en place va supprimer cette partie <a|a . La popup apparaît et le challenge est validé.







