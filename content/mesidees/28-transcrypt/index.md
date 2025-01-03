---
title: Git et les mots de passe
description: Git et les mots de passe
weight: 128
---

### Résumé
La (grande?) majorité des développeurs utilisent aujourd'hui GIT pour sauvegarder et partager leurs développements.
Tout y est stocké. Et, parfois, même les mots de passe (clefs, secrets, ...) !!

La solution optimale serait de ne pas les insérer dans le dépôt GIT. Mais cela signifie ne plus les partager avec le reste de l'équipe.

Une autre solution consiste à chiffrer ces informations avant de les partager sur le repository commun sans oublier de les déchiffrer sur le repository local.

Une solution (parmis d'autre) est [Transcrypt](https://github.com/elasticdog/transcrypt).

### Qu'est-ce que Transcrypt ?

Transcrypt est un outil permettant d'initialiser le chiffrement de certains fichiers dans un dépôt Git.

Une fois le dépôt paramétré, Transcrypt n'est plus nécessaire et peut être désinstallé.

La liste des fichiers à chiffrer avant (et déchiffrer après) chaque commit est paramétrée dans le fichier .gitattributes

### Par quoi commencer ?

*La toute première chose à faire est de faire disparaître de l'historique GIT le fichier contenant des mots de passe !!*
Pour cela, il suffit d'exécuter les commandes suivantes :
```
git filter-branch -f --tree-filter 'rm -f projects/coupeDesMaisons/src/environments/environment.ts' HEAD
git push --force
```
	
/!\ Prendre le temps de tester le chemin du fichier permet de ne pas attendre la longue exécution du script sans qu'aucun commit ne soit modifié !

### Pour initialiser un dépot avec Transcrypt
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

