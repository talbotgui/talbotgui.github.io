---
title: Installation d'un serveur pour une Integration Continue
description: Installation d'un serveur pour une Integration Continue
weight: 117
---


#### Création de la machine :

Créer une machine de type *small* chez Amazon EC2 avec l'OS Ubuntu xx.xx de votre choix.

Dans le groupe de sécurité, ouvrir les ports HTTP HTTPS SSH (ouverts à tous)

#### Mise à jour de la plateforme :
Se connecter en SSH et exécuter
```
sudo apt-get update
sudo apt-get -y upgrade
```

#### Installation de Java et Jenkins :
```
sudo apt-get -y install default-jdk
	
@see https://www.jenkins.io/doc/book/installing/linux/#weekly-release
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get -y install jenkins
```
 
Pour définir le rootContext de Jenkins, exécuter ```sudo vi /etc/default/jenkins``` et modifier la dernière ligne pour ajouter "--prefix=$PREFIX" (attention au double tirets)

Pour prendre en compte la modification, redémarrer Jenkins

#### Installation apache2
```
sudo apt-get -y install apache2
sudo a2enmod proxy proxy_http
sudo a2enmod headers
sudo a2enmod ssl
```

Créer le fichier ```sudo vi /etc/apache2/sites-available/monHttp.conf```
```
<VirtualHost *:80>
 ServerAdmin webmaster@localhost
 ServerName me.guillaumetalbot.com
 ServerAlias ci
</VirtualHost>
```

```
sudo a2ensite monHttp
sudo /etc/init.d/apache2 restart
```

#### Installation LetsEncrypt
*@see https://certbot.eff.org/instructions et sélectionner Apache et le bon OS*

**Penser à configurer le nom de domaine pour pointer sur la machine auprès du DNS avant**
```
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --apache
```

#### Limiter les informations fournies par le serveur
Modifier le fichier ```sudo vi /etc/apache2/conf-available/security.conf``` et changer les valeurs :

* ServerTokens Prod
* ServerSignature Off

#### Activer les accès sécurisés aux outils :
Modifier le fichier ```sudo vi /etc/apache2/sites-available/monHttp-le-ssl.conf``` et y ajouter
```
  DocumentRoot /var/www/html
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
  ProxyRequests Off
  ProxyVia Off
  <Proxy *>
   Order deny,allow
   Allow from all
  </Proxy>
  ProxyPreserveHost on
  ProxyPass /jenkins http://localhost:8080/jenkins nocanon
  ProxyPassReverse /jenkins http://locahost:8080/jenkins
  ProxyPassReverse /jenkins  http://me.guillaumetalbot.com/jenkins
  RequestHeader set X-Forwarded-Proto "https"
  RequestHeader set X-Forwarded-Port "443"
  AllowEncodedSlashes NoDecode

  <IfModule mod_headers.c>
   Header set X-Content-Type-Options nosniff
   Header set X-Permitted-Cross-Domain-Policies "none"
   # pour ne pas bloquer Jenkins
   # Header set Content-Security-Policy "child-src 'none'; object-src 'none'"
   # Header set X-Frame-Options DENY
   Header set X-XSS-Protection "1; mode=block;"
   Header set Strict-Transport-Security "max-age=300; includeSubDomains; preload; always;"
   Header set Public-Key-Pins "pin-sha256=\"base64+primary==\"; pin-sha256=\"base64+backup==\"; max-age=5184000; includeSubDomains"
  </IfModule>
```

Puis exécuter :
```
sudo /etc/init.d/apache2 restart
sudo rm /var/www/html/index.html
```

Créer le fichier ```sudo vi /var/www/html/index.html``` avec le contenu suivant :
```
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8" />
  <meta http-equiv="refresh" content="3; url=https://talbotgui.github.io" />
 </head>
 <body>
  Redirection en cours...
 </body>
</html>
```

#### Configuration Jenkins :
Aller à l'adresse https://...../jenkins et suivre les instructions

#### Installation email : 
```
sudo apt-get install postfix
  me.guillaumetalbot.com
sudo vi /etc/postfix/main.cf
  myhostname = me.guillaumetalbot.com
sudo service postfix restart
```

#### Installation Maven :
```
sudo mkdir /usr/share/m2repo
sudo chmod a+rw /usr/share/m2repo
sudo apt-get -y install maven
```
Modifier le fichier ```sudo vi /usr/share/maven/conf/settings.xml``` pour définir le repo local 

#### Installation NPM & NodeJS
Vérifier la dernière version disponible du setup et remplacer les YY (version 14 en aout 2021).
```
curl -sL https://deb.nodesource.com/setup_YY.x | sudo -E bash -
sudo apt-get install -y nodejs
```

#### Pour paramétrer Jenkins avec une configuration de base
Premier accès et paramétrage de base
* accéder au portail pour la première fois
* suivre les instructions pour se connecter en tant qu'administrateur
* sélectionner l'installation des plugins courants

Installer les plugins nécessaires
* accéder à la partie "administrer Jenkins" puis "gestion des plugins"
* dans la liste des plugins disponibles, sélectionner "blue ocean" et cliquer sur "installer sans redémarrer"
* dans la liste des plugins disponibles, sélectionner "Pipeline Utility Steps" et cliquer sur "installer sans redémarrer"
* une fois les plugins installés, cliquer sur la case "redémarrer Jenkins" et patienter que Jenkins redémarre

Déclarer les outils
* accéder à la partie "administrer Jenkins" puis "Configuration globale des outils"
* cliquer sur "Ajouter JDK", décocher "installation automatique", saisir "java local" et "/usr/lib/jvm/java-11-openjdk-amd64/"
* cliquer sur "Ajouter Maven", décocher "installation automatique", saisir "M3" et "/usr/share/maven/"

Déclarer les clefs
* faire le tour de tous vos dépôts et jenkinsFile pour lister les clefs nécessaires à leur bon fonctionnement
* accéder à la partie "administrer Jenkins" puis "Manage credentials"
* cliquer sur "Jenkins" puis "identifiants globaux"
* cliquer sur "Ajouter identifiant" et saisir les élements nécessaires

#### Publication d'application
Si votre serveur héberge aussi des applications déployées automatiquement depuis votre Jenkins
* faire le tour de tous vos dépôts et jenkinsFile pour lister les clefs nécessaires à leur bon fonctionnement
* exécuter autant de fois que nécessaire les commandes suivantes
```
sudo mkdir /var/www/html/xxx
sudo chmod a+rw /var/www/html/xxx
```

#### Si la machine est de type *tiny*
La machine va manquer de RAM. Pour ajouter une partition de SWAP (dans un fichier) (http://tecadmin.net/add-swap-partition-on-ec2-linux-instance/)
```
sudo dd if=/dev/zero of=/var/myswap bs=1M count=2048
sudo mkswap /var/myswap
sudo swapon /var/myswap
sudo chmod 0644 /var/myswap
```

#### Petits outils à installer
Parmis les outils utiles dans les scripts SHELL, il faut bien souvent installer
```
sudo apt-get -y install zip
```

#### Les répertoires à vider pour récupérer de l'espace :
/var/lib/jenkins/.npm/_logs/

#### Notes sur l'installation d'un Archiva
Ressources :

* documentation : https://archiva.apache.org/docs/2.2.3/quick-start.html
* installation : http://apache.mirrors.nublue.co.uk/archiva/2.2.3/binaries/apache-archiva-2.2.3-bin.zip

Installation :

* extraire le contenu de l'archive
* si Java9 est le JDK par défaut, modifier le fichier conf/wrapper.conf : ``` wrapper.java.command=C:/Program Files/Java/jdk1.8.0_151/bin/java ```
* sous Windows, dans un interpréteur de commande en mode "Admin", exécuter ``` archiva install ``` puis ``` archiva start ```
* Archiva sera disponible à l'adresse http://localhost:8080/

#### Petites commandes pratiques pour entretenir le serveur :

Pour rechercher les répertoires prenant de la place : ```sudo du -cha --max-depth=1 / | grep -E "[0-9]M|[0-9]G"```

Et un petit script permettant de faire le ménage régulièrement :
```
apt clean all
rm /var/log/**/*.gz
rm /var/log/journal/*/*-00000*.journal
rm -rf /var/lib/jenkins/workspace/*
rm -rf /var/lib/jenkins/.sonar/*
rm -rf /var/lib/jenkins/.m2/*
rm -rf /var/lib/jenkins/.npm/*
```

#### Installation du Cloud SDK

* documentation : https://cloud.google.com/sdk/docs/downloads-apt-get

Installation :
```
export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"
echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

Connexion :

* créer une clef depuis la console GCloud (IAM / comptes de service)
* activer les APIs "Cloud Resource Manager API" et "App Engine Admin API" depuis la console GCloud (API / Bibliotheque)
* déposer le fichier JSON sur le serveur
* se connecter une première fois avec ```gcloud auth activate-service-account --key-file=LE_FICHIER_JSON```
* exécuter la commande ```gcloud init``` et renseigner toutes les informations
