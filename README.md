# 1/ Description du contenu
Ce repository contient les sources utilisées par Hugo pour généré le site talbotgui.github.com (https://gohugo.io/overview/introduction).

# 2/ Guide du développeur
## 2.1/ Cloner de dépôt
Ce dépôt référence un submodule. Pour cloner le dépôt avec le submodule, il faut exécuter la commande
```git clone --recurse-submodules https://github.com/talbotgui/pages-hugo.git```

Si le paramètre ```--recurse-submodules``` a été oublié, il faut exécuter, dans le répertoire du repository, les commandes suivantes :
```
git submodule init
git submodule update
```

## 2.2/ Afficher uniquement le site en local
Pour démarrer Hugo en mode DEV (avec rechargement automatique), il suffit d'exécuter la commande ```npm run serve-win```.

## 2.3/ Pour vérifier qu'aucun lien n'est cassé
L'outil "broken-link-checker" permet de vérifier que tous les liens du site fonctionnent.
Pour l'exécuter sur le site localement, exécuter la commande ```npm run blc```
