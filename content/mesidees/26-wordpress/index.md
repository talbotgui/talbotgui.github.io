---
title: Installer une instance WordPress sur Ubuntu
description: Installer une instance WordPress sur Ubuntu
weight: 126
---

## 1/ Introduction
Cette procédure a été mise à jour en décembre 2024 lors de la recréation des instances de mes serveurs.

## 2/ Installation 

### 2.1/ Créer l'infrastructure
Depuis le portail EC2 d'Amazon, 
* créer un groupe de sécurité avec les caractéristiques suivantes :
  * TODO
* créer une machine *t2.micro* (une t2.nano ne permet pas d'installer MySQL par manque de mémoire)
* s'y connecter via le navigateur
* mettre à jour la machine avec les commandes 
```
sudo apt-get update
sudo apt-get -y upgrade
```

Pour obtenir, à coup sûr, l'IP publique de la machine, exécuter la commande :
```
curl ifconfig.me
```

### 2.2/ Faire pointer le DNS
Depuis votre fournisseur de DNS, créer l'entrée suivante :
* de type *A*, faire pointer *me.guillaumetalbot.com* vers l'IP publique de la machine EC2


### 2.3/ Installer apache2
Pour installer *apache2*, exécuter les commandes :
```
sudo apt-get -y install apache2
sudo a2enmod proxy proxy_http headers ssl rewrite
sudo systemctl restart apache2
```

Pour vérifier que le service est démarré et le serveur accessible, ouvrir un navigateur avec l'adresse *http://xxx.xxx.xxx.xxx* avec *xxx.xxx.xxx.xxx* l'IP obtenue de la commande précédente.

Pour déclarer un premier vhost HTTP écoutant sur le port 80 (temporaire évidemment), créer le fichier ```sudo vi /etc/apache2/sites-available/vHostHttp.conf```
```
<VirtualHost *:80>
 ServerAdmin webmaster@localhost
 ServerName me.guillaumetalbot.com
 ServerAlias ci
</VirtualHost>
```

Pour activer le nouveau vhost et désactiver le vhost par défaut
```
sudo a2ensite vHostHttp
sudo a2dissite 000-default.conf
sudo a2disconf localized-error-pages.conf charset.conf serve-cgi-bin.conf
sudo systemctl reload apache2
```

Pour limiter les informations fournies par le serveur dans les entêtes de réponse, modifier la configuration de sécurité via la commande 
```
sudo vi /etc/apache2/conf-available/security.conf
```
Puis modifier (en commentant, décommantant ou modifiant les lignes) pour obtenir
* la clef *ServerTokens* avec la valeur *Prod*
* la clef *ServerSignature*  avec la valeur *Off*
* la clef *TraceEnable* avec la valeur *Off*
* la configuration de l'entête *Header set X-Content-Type-Options: "nosniff"*
* la configuration de l'entête *Header set Content-Security-Policy "frame-ancestors 'self';"*
Pour prendre en compte les modifications, exécuter la commande
```
sudo systemctl reload apache2
```

### 2.4/ Installer CertBot
** Il faut que la configuration DNS soit active et propagée pour installer CertBot. **

Pour tester la configuration DNS, tenter d'accéder au site *http://me.guillaumetalbot.com* et vérifier que la page par défaut d'Apache2 s'affiche.

Puis suivre les instructions du site https://certbot.eff.org/instructions en y sélectionnant Apache et le bon OS.

Si la procédure n'a pas changée, cela se résume aux commandes suivantes (à vérifier tout de même) :
```
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --apache
```

Pour tester le bon fonctionnement du site en HTTPs, tenter d'accéder au site *https://me.guillaumetalbot.com* et vérifier que la page par défaut d'Apache2 s'affiche.

Pour désactiver l'accès HTTP, réaliser les actions :
* modifier le fichier ```sudo vi /etc/apache2/ports.conf``` pour en supprimer la ligne *Listen 80*
* exécuter les commandes :
```
sudo a2dissite vHostHttp
sudo systemctl reload apache2
```

### 2.5/ Installer les prérequis à WordPress
** Une bonne procédure de référence est disponible sur le site [ubuntu-fr.org](https://doc.ubuntu-fr.org/wordpress)*

Pour installer des utilitaires précieux, exécuter la commande :
```
sudo apt-get install unzip
```

Pour installer *MySQL*, exécuter la commande :
```
sudo apt install mysql-server
```

Pour  paramétrer *MySQL*, exécuter les commandes :
```
sudo mysql
> CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
> CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'un_mot_de_passe_a_choisir_et_stocker_dans_keepass';
> GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost';
> FLUSH PRIVILEGES;
> EXIT;
```

Pour installer *PHP*, exécuter la commande :
```
sudo apt install php php-mysql php-curl php-gd php-intl php-json php-mbstring php-xml php-zip libapache2-mod-php
```


### 2.6/ Installer WordPress

Pour paramétrer *Apache2*,
* modifier le fichier ```sudo vi /etc/apache2/sites-enabled/vHostHttp-le-ssl.conf``` pour y ajouter, sous *ServerAlias*, les lignes suivantes :
```
 DocumentRoot /var/www/wordpress
 <Directory /var/www/wordpress>
  AllowOverride all
  Require all granted
 </Directory>
```
* puis exécuter la commande 
```
sudo systemctl reload apache2
```

Pour installer *WordPress*, exécuter les commandes :
```
cd 
pwd
wget https://fr.wordpress.org/wordpress-latest-fr_FR.zip
sudo unzip wordpress-latest-fr_FR.zip -d /var/www
sudo chown www-data:www-data /var/www/wordpress -R
sudo chmod -R -wx,u+rwX,g+rX,o+rX /var/www/wordpress
```

Pour paramétrer *WordPress*, accéder à la page https://me.guillaumetalbot.com/ et laisser WordPress guider via son interface de paramétrage.

Une fois l'interface d'administration affichée, voici quelques étapes incontournables :
* dans le menu *Extensions > Ajouter une extension*
  * installer puis activer *WP Super Cache*
  * installer puis activer *Yoast SEO*
  * installer puis activer *FileBird*
  * installer puis activer *UpdraftPlus*
  * installer puis activer *CC Child Pages*
* dans le menu *Réglages > WP Super Cache*
  * sélectionner l'option *Mise en cache activée (Recommandé)* et cliquer sur *Mettre à jour l'état*
* dans le menu *UpdaftPlus* puis l'onglet *Réglages*
  * sélectionner les deux planifications *mensuelles* avec une rétention de *1*
  * mettre en place la sauvegarde distante (non détaillée ici)
* dans le menu *Yoast SEO*
  * dérouler la procédure de configuration minimale
  * dans le sous-menu *Réglages*, 
    * activer *Suivi d'utilisation*
	* dans *Réglages essentiels*, renseigner le formulaire
	* enregistrer les modifications
* dans le menu *Extensions > Extensions installées*
  * supprimer *Hello Dolly*
  * supprimer *Akismet Anti-spam: Spam Protection*
  * activer la mise à jour automatique pour toutes les extensions
* dans le menu *Comptes > Ajouter un compte*
  * créer un compte pour chaque personne en ayant besoin
* dans le menu *Réglages > Lecture*
  * définir la page par défaut du site (pas celle des articles)
* dans le menu *Apparence > Thèmes*
  * installer le thème *NewsBlogger*


