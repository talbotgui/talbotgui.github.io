---
title: Git et les mots de passe
description: Git et les mots de passe
weight: 128
---

### 1/ Résumé
La (grande?) majorité des développeurs utilisent aujourd'hui GIT pour sauvegarder et partager leurs développements.
Tout y est stocké. Et, parfois, même les mots de passe (clefs, secrets, ...) !!

La solution optimale serait de ne pas les insérer dans le dépôt GIT. Mais cela signifie ne plus les partager avec le reste de l'équipe.

Une autre solution consiste à chiffrer ces informations avant de les partager sur le repository commun sans oublier de les déchiffrer sur le repository local.

Une solution (parmis d'autre) est [Transcrypt](https://github.com/elasticdog/transcrypt).

### 2/ Qu'est-ce que Transcrypt ?

Transcrypt est un outil permettant d'initialiser le chiffrement de certains fichiers dans un dépôt Git.

Une fois le dépôt paramétré, Transcrypt n'est plus nécessaire et peut être désinstallé.

La liste des fichiers à chiffrer avant (et déchiffrer après) chaque commit est paramétrée dans le fichier .gitattributes

### 3/ Par quoi commencer ?

* La toute première chose à faire est d'**identifier les fichiers contenant des secrets dans l'historique GIT**. Pour cela, l'outil [GitLeaks](https://github.com/gitleaks/gitleaks?tab=readme-ov-file) est bien utile.
  * Télécharger une version compatible avec le poste de développement depuis la [page de téléchargement](https://github.com/gitleaks/gitleaks/releases).
  * Extraire le contenu de l'archive dans un répertoire dédié en dehors du clone du dépôt.
  * Ouvrir une invite de commande dans le clone à analyser
  * Exécuter la commande ``````xxx/gitleaks_xxxx/gitleaks.exe git --report-path gitleaks-report.json```
  * Le fichier **gitleaks-report.json** contient alors tous les secrets détectés
  * S'assurer que le fichier ne contient aucun caractère # (sinon la commande SED est KO)
  * Extraire la liste des fichiers et secrets avec le pattern ```^.*"(File|Secret)".*$```
  * Mettre sur une même ligne le secret et le nom de fichier
  * Eliminer les doublons
  * Faire des chemins de fichier des chemins absolus
  * Mettre en forme chaque ligne pour obtenir ```sed -i "s#LE_SECRET_ISSU_DE_GITLEAK#xxxxxxxx#g" "LE_NOM_DE_FICHIER_ISSU_DE_GITLEAK"```
  * Sauvegarder l'ensemble des commandes dans un script shell déposé à la racine du dépôt
  
* La seconde étape est de faire disparaître de l'historique GIT le(s) secret(s). Pour cela, il suffit 
  * télécharger l'outil BFG depuis la [page officielle](https://rtyley.github.io/bfg-repo-cleaner/#usage)
  * créer un fichier contenant, sur une ligne différente, chaque secret venant du fichier généré par GitLeaks
    * après avoir retiré le caractère = en fin de secret
    * suffixé par ```==>xxxxxxxx```
  * exécuter les commandes
```
java -jar /d/xxxx/bfg.jar --replace-text /d/xxx/password.txt "/d/cheminVersDepot"
cd /d/cheminVersDepot
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

* Une fois les secrets supprimés, relancer l'analyse avec la commande précédete

### 4/ Pour initialiser un dépot avec Transcrypt
* Installer Transcrypt :
  * Dans un répertoire d'outillage, exécuter la commande suivante pour récupérer la dernière version de l'outil :
```git clone https://github.com/elasticdog/transcrypt.git``` 
  * Ajouter le répertoire nouvellement créé dans la variable d'environnement PATH
  * Ouvrir une nouvelle invite de commande dans tout autre répertoire (pour être certain que la dernière valeur de PATH est utilisée)
  * Y exécuter la commande pour vérifier que le script est bien accessible partout (l'aide de Transcrypt doit s'afficher)
```transcrypt --help``` 
* Initialiser le chiffrement dans le dépôt :
  * Depuis une invite de commande, se positionner dans le répertoire du dépôt à traiter
  * Y exécuter la commande
```transcrypt```
  * Répondre aux questions posées
  * Conserver le mot de passe généré (ou saisi) dans votre [Keepass](https://keepass.fr/)
* Ajouter un fichier et déclarer qu'il doit être chiffré :
  * Ajouter le fichier normalement :
```git add le/chemin/du/fichier.extension```
  * Ajouter la ligne suivante dans le fichier .gitattributes
 ```le/chemin/du/fichier.extension filter=crypt diff=crypt merge=crypt```

Voici un exemple pour un fichier environment d'Angular : 
```projects/coupeDesMaisons/src/environments/environment.prod.ts filter=crypt diff=crypt merge=crypt```

