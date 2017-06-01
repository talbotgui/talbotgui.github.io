---
title: Hugo et GitHub:Pages
description: Hugo et GitHub:Pages
weight: 107
---

## 1/ Introduction
*[GitHub:Pages](https://pages.github.com/)* est une fonctionnalité très utile de GitHub permettant d'héberger un site WEB en le déposant, tout simplement, dans un dépôt Git.

*[Hugo](http://gohugo.io/)* est un outil de génération de site WEB à partir de template et de Markdown.

Réunis, ces deux outils permettent de réaliser un site très simplement. Les usages sont nombreux. Mais le premier qui me vient est la documentation d'un projet : installation du poste, règles de conception/développement/test, pratiques du projet, ... bref, un [guide du développeur](/mesidees/10-guidedudev/).

## 2/ Objectifs
Les objectifs de cette page sont :
* de disposer d'un repository contenant les sources du site avec des pages en markdown ;
* d'utiliser [Hugo](http://gohugo.io/) pour réaliser la mise en page et générer le site statique ;
* de déployer ce site statique dans un repository [GitHub:Pages](https://pages.github.com/) ;
* de sipmplifier au maximum la génération et la publication du site.

## 2/ La mise en place de Hugo
Hugo est un exécutable. Il faut donc le télécharger (depuis la [page officielle](https://github.com/gohugoio/hugo/releases/)) et le déposer dans un répertoire.

Attention, cet exécutable est dépendant de l'OS. Donc, pour construire le site sur Windows et sur Unix, il faut déposer les deux exécutables.

La première étape est donc de 
* créer un répertoire
* y placer l'exécutable de Hugo (en conservant ou non le numéro de version dans le nom du fichier)

## 3/ Créer le site HUGO avec un thème
Pour créer un site avec le thème LEARN (celui utilisé par ce site), exécuter les commandes suivantes (en adaptant le numéro de version si vous souhaitez le conserver dans le nom de fichier) :
```shell
hugo-0.140.1 new site monProjet
cd monProjet
git init
git submodule add https://github.com/McShelby/hugo-theme-relearn.git themes/hugo-theme-relearn
echo "theme = 'hugo-theme-relearn'" >> hugo.toml
mv ../hugo-0.140.1 ./
hugo-0.140.1 server
```

Le site disponible [en local](http://localhost:1313) n'affiche qu'une page d'accueil vide et aucun contenu.

## 4/ Paramétrer HUGO

### 4.1/ Paramétrage essentiel
Le fichier *hugo.toml* peut être très vide ou très riche selon les besoins. A mon avis, sont obligatoires les informations suivantes :
```toml
# L'URL de base du site
baseURL = 'https://mon.serveur.de.doc/'

# Le nom du thème
theme = 'hugo-theme-relearn'

# La langue du site
languageCode = "fr-fr"

# Le titre du site
title = "Mon super site"
```

### 4.2/ Paramétrage propre au thème
De plus, chaque thème apporte des fonctionnalités. Voici la [documentation du thème relearn](https://mcshelby.github.io/hugo-theme-relearn/introduction/quickstart/index.html)

Par exemple, il est utile/possible de :
```toml
[params]

  # Thème bleu par défaut mais autres thèmes disponibles
  themeVariant = ['blue', 'relearn-light', 'relearn-dark']

  # Désactiver le lien 'Home' dans le menu
  disableLandingPageButton = true

  # Définir l'auteur
  [params.author]
    email = 'talbotgui@gmail.com'
    name = 'Guillaume TALBOT'

# Activer l'impression
[outputs]
  home = ['html', 'print']
  page = ['html', 'print']
  section = ['html', 'print']
```

## 5/ Créer un premier contenu
Pour créer une page dans Hugo, il suffit de créer une arboresence dans le répertoire *./content*.

Chaque répertoire doit contenir un fichier *index.md* dont le contenu commence par :
```md
---
title: Titre de ma page
description: La description de ma page
weight: 101
---

## Voici le premier chapitre de ma page
```

## 6/ Mettre en place un build

Hugo est un outil de génération et pas un outil de build.

Pour ne pas avoir à retenir les paramètres de ligne de commande à utiliser pour démarrer un serveur local ou générer l'application, il est préférable de stocker les scripts quelque part. Comme un outil de build (Maven ou NPM) est toujours utile, autant en utiliser un. Donc il faut créer un *package.json* minimal :
```json
{
	"name": "monProjet",
	"version": "1.0",
	"scripts": {
		"start": "hugo-0.140.1 serve --bind 0.0.0.0 ",
		"build": "hugo-0.140.1.exe"
	},
	"dependencies": {
	}
}
```

Pour démarrer le serveur, il suffit donc d'exécuter la commande ```npm run start```.

Pour générer le site, il suffit donc d'exécuter la commande ```npm run build```.

## 7/ Publier dans GitHub
La plus simple des solutions de publication dans Gitbug-Pages est de créer un répertoire *./docs* dans la branche principale du dépôt avec le contenu du site.

Or ce répertoire est utilisé, par défaut, par Hugo, pour stocker les pages HTML générés par le serveur local. Il est donc préférable de séparer ce répertoire de génération temporaire (*hugo serve*) du répertoire de génération réel (*hugo*).

De plus, la génération ajoute ou modifie les fichiers du répertoire *./docs* mais ne supprime rien. Donc il faut que le répertoire soit vidé avant de faire générer les pages HTML à Hugo.

Donc le *package.json* doit être enrichi :
```json
{
	"name": "monProjet",
	"version": "1.0",
	"scripts": {
		"start": "hugo-0.140.1 serve --bind 0.0.0.0 -d docsServe",
		"build": "rm -rf ./docs/* && hugo-0.140.1.exe"
	},
	"dependencies": {
	}
}
```

