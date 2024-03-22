![logo](https://visa.cefim.eu/wp-content/uploads/2023/06/LogoCEFIM-hd.png)

# PROJET INFRA PREPATECH 2023_1

## Préambule / Entreprise
![LOGO Générique](https://store-images.s-microsoft.com/image/apps.27845.30be3330-280f-4e8e-ac1d-35f4f8176e71.bee1f510-c788-4634-97f7-1edb3738e3f1.ce090a7e-7c5d-45fa-91b2-c964e56e10d9)

* Créer un groupe de 4 personnes (shuffle via ploufplouf, affinités).
* Créer un pool de ressources collaboratives Online type GoogleWorkspace,OneDrive/Teams, Notion.
* Choisir une identité visuelle (un nom,logotype,typo,2 couleurs,un motto).
* Créer un descriptif de la société sur une feuille A4.
* Créer un organigramme de la société, vous êtes le service IT dans la DSI.
* Créer un fichier XLSX et CSV de 2000 personnes qui seront réparties dans au moins 5 services.

*Validation par le référent/formateur avant la phase suivante*

## Infra Réseau WAN

![WAN](https://blog.udemy.com/wp-content/uploads/2014/06/shutterstock_91234700-620x465.jpg)

* Choisir une prise RJ45 dans la salle
* Se coordonner avec Roger (alternant Réseau) pour s'interconnecter à la baie
* Demander à Roger une adresse IP Publique
* Tester la prise brassée dans la salle de cours directement sur une poste
* Chercher/Monter un PC avec deux IF, 1 écran et un clavier
* Installer IPFire via une clé usb bootable
* configurer le WAN correctement sur la partie IPFire et vérifier qu'un ping www.perdu.com fonctionne
* configurer le LAN rapidement (sans DHCP) sur une adresse IPv4 privée de classe B en /22

*Validation par le référent/formateur avant la phase suivante*


## Infra Réseau LAN

![LAN](https://1.bp.blogspot.com/-4Om1fcaJZEE/YP7Rtvwxg_I/AAAAAAAADYA/fBPxXW_u44YxZTZtRxI-_nYeESBxOUbkACLcBGAsYHQ/w640-h474/LAN.jpg)

* Choisir et configurer un switch manageable dlink, le configurer et l'adresser correctement
* Choisir et configurer une borne WIFI (la mettre temporairement) en mode serveur DHCP


## Infra Système n°1 (physique)

![SYSTEME](https://p1-ofp.static.pub/medias/bWFzdGVyfHJvb3R8MTE2MDMzfGltYWdlL3BuZ3xoNjEvaGMyLzk0ODkwMjM0MzQ3ODIucG5nfGI3ZWY1MTQ5Mzc1NTY2YmQxNzdkMzhjMzdiNzM5NGIwZWFkNTRhYWNiNGM5MDI4NmRiMTFkM2U0MTNkZTRmN2Y/lenovo-servers-tower-thinksystem-st550-series.png)

* Choisir et configurer un NAS Synology, le configurer et l'adresser correctement
* Choisir et configurer une imprimante réseau, la configurer et l'adresser correctement
* Choisir et configurer une tour avec Linux Debian 12 qui va servir de "Tool"
* Choisir et configurer un serveur cube de virtualisation, le configurer avec Proxmox et l'adresser correctement

Créer un schéma réseau de votre infra à ce moment précis à l'aide de [https://app.diagrams.net/](https://app.diagrams.net/)

*Validation par le réfe
érent/formateur avant la phase suivante*


## Infra Système n°2 (virtualisée)
![VIRTU](https://www.ovhcloud.com/sites/default/files/styles/square_small/public/2020-07/UseCase_What-is-virtualization%402x.png)

### Workgroup
* Choisir un nom de WorkGroup
* Virtualiser sur vos postes clients un Win7 32bits et le mettre à jour au max
* Virtualiser sur vos postes clients un Win8/WIn8.1 ou Win8.1Update1 64 bits et le mettre à jour au max
* Installer un serveur 2012R2 sur le proxmox
* Appliquer sur ces VMs une fiche de recette qui vous sera livrée qui se trouve ci-desosus
* Créer un partage sur le NAS et le monter sur les postes

**Recette/Checklist de votre configuration logicielle du client/serveur windows**

1. Installer les VMWare Tools ou équivalent
1. Faire toutes les MAJ jusqu'à un certain point
1. Couper ensuite les MAJ de windows update (voir le programme sconfig sur server)
1. Couper la sécurité renforcée d'internet explorer pour les admin seulement (pour les server)
1. Changer le nom de la machine pour un nom unique et pertinent
1. Changer le workgroup pour "VOTREWORKGROUP"
1. Mettre l'ordinateur dans un réseau privé (pas public, ni domaine)
1. Couper l'IPv6
1. Mettre une IP fixe à cet ordinateur (genre 192.168.X.Y à déterminer selon votre contexte) avec comme DNS 1.1.1.1 et comme passerelle 192.168.X.Y
1. Autoriser la prise en main à distance par le bureau à distance
1. Installer les logiciels suivant : 7zip, sumatrapdf, notepad++, chrome, bginfo --> ninite peut-être utile
1. Créer des règle entrantes dans le Firewall pour l'ICMP
1. Votre machine doit être pinguable

### Active Directory

![LOGO AD](https://www.outsystems.com/Forge_CW/_image.aspx/Q8LvY--6WakOw9afDCuuGZlRwMsAHVuN679pp-uc0hY=/active-directory-lib-2023-01-04%2000-00-00-2024-02-04%2011-17-32)

1. Installation du Rôle ADFS
1. Ajout des PC CLients en VMs (Win 7 & 8.1) dans le domaine
1. Configuration du Rôle DNS
	2. Mise à jour des roots DNSen IPv4 et v6 (via le site https://root-servers.org/)
	2. Redirecteur sur 1.1.1.1 (et 1.0.0.1)
	2. Capture d'une trame DNS d'un poste client dans les logs du serveurs DNS
1. Création des OU
1. Import des users du CSV dans les OU via un script powershell
1. Création de 5 GPO utiles et appliquées sur les postes clients en VMs 

*Validation par le référent/formateur avant la phase suivante*

### Windows Server
* Installer un Windows Server 2012R2 sur le proxmox qui va héberger le rôle DHCP
* Installer un WIndows Server 2012R2 sur le proxmox qui va héberger le rôle Serveur de Fichiers (avec quotas & filtres
* Créer un partage de fichier sur ce serveur
	* Créer un disque d: 
	* R/W sur un dossier commun pour les utilisateur d'une OU
	* R sur un dossier public pour tous les utilisateurs du domaine
	* R/W sur un dossier perso pour tous les utilisateurs du domaine
	* Interdire les fichiers MP3 pour commun et perso
	* Mettre un quotas de 500 Mo pour les partages perso
	* Mapper les lecteurs des partages par GPO
* Installer un Windows Server 2012R2 sur le proxmox qui va héberger le rôle IIS pour transformer ce serveur en intranet (une simple page web index.html en https (avec HSTS) suffira, elle sera la page par défaut des clients via une GPO)


**Il faut bien évidemment intégrer ces 3 serveurs à l'AD**

### Linux
* Installer un Linux Debian 11 (pas 12) sans GUI sur le proxmox qui va héberger le futur site web front-end de votre entreprise sur www.votredomaine.lan
* Installer un LAMPS sur ce serveur et créer un VHost qui point vers /home/siteweb/www
* Créer une règle HSTS sur ce serveur pour la redirection http-->https
* Créer une règle NAT dans IPFire pour le rendre accessible de l'exterieur
* Créer un deuxième Vhost pour le nom de domaine online avec let's encrypt


* Installer sur un Linux Debian sans GUI le GLPI et le faire pointer sur support.votredomaine.lan (avec certificat local auto-signe , HSTS et HTTPS)

**Il faut également intégrer ces 2 serveurs à l'AD à l'aide du binaire REALM**


## Infra Système n°3 (update)

*TBA*

## Sécurité

*TBA*

## Livrables finaux et restitution en vue du jury

*TBA*



