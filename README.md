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

Première chose à faire, c'est la mise à jour du système et des dépôts associés depuis le terminal fraichement lancé sous Kali:

```
sudo su
apt update
apt upgrade -y




