---
title: Rédiger dans WordPress
description: Rédiger dans WordPress
weight: 128
---

## 1/ Introduction
Cette page contient des notes prises durant la refonte d'un site WordPress d'une association dont je fais partie.

## 2/ Règles de base

R1/ Si l'information est effémère ou liée à un évènement ponctuel, l'information doit faire l'objet d'un ARTICLE. Sinon ce sera une PAGE.

R2/ Si une image doit être publiée, il faut :
* accéder à la page *Médias > Médiathèque*
* sélectionner (ou créer) un répertoire correspondant à la PAGE ou à l'ARTICLE dans laquelle la photo doit apparaître


R3/ Si toutes les images d'un répertoire doivent être insérées dans une unique galerie, le bloc *FileBird Galery* est à utiliser de préférence car il permet d'afficher toutes les images d'un répertoire
  * une fois le bloc inséré, sélectionner le répertoire dans la barre latérale à droite
  * sélectionner le nombre de colonnes voulues (1 seule si une seule image, ...)
  * activer/désactiver le *crop* pour vérifier si des images sont dénaturées et sélectionner la meilleure option
  * tester la valeur *Masonry* du paramètre *Layout* pour obtenir, potentiellement, un meilleur rendu

R4/ Si seules quelques images d'un répertoire doivent être insérées, le bloc *Galerie* doit être utilisé.

R5/ Si une unique image doit être insérée et qu'un texte doit y être associée, le bloc *Média & texte* doit être utilisé.

R5/ Avant toute publication, doivent être vérifiés les éléments suivants :
* supprimer les doubles espaces
* supprimer les espaces avant une virgule ou un point
* les siècles sont au format romain avec le *ème* en exposant

R6/ Tous les liens pointant vers un autre site doivent s'ouvrir dans un nouvel onglet.

## 3/ Rédiger un article

Ra1/ Le titre d'un article commence par la date de l'évènement ou, à défaut, par le date de publication.

Ra2/ Si l'information est une copie d'un article de presse, l'article de notre site doit :
* fournir la source : ```Source : https://www.leberry.fr/baugy-18800/travaux-urbanisme/une-premiere-tranche-des-travaux-de-renovation-de-l-eglise-de-baugy-achevee_14562797/```
* mettre le contenu de l'article de presse dans un bloc *Citation*
* ne rien modifier dans le texte de l'article de presse 
* ajouter toute remarque/complément/... en dehors du bloc *Citation*

Ra3/ Avant de publier un article,
* dans la barre latérale droite, dans l'onglet *Article*, doivent être 
  * être sélectionnée, a minima, une catégorie (l'année)
  * vérifiée/modifiée la date de publication présente dans le champs *Publier* 
  * vérifiée la lisibilité de l'article 

## 4/ Rédiger une page

Rp1/ A la création de la page, dans la barre latérale droite, dans l'onglet *Page*, il est préférable de, tout de suite, saisir la page parente dans le paramètre *Parent*.

Rp2/ Toute page parente dont introduire les pages filles. Pour créer une liste de lien vers les pages filles, insérer un bloc *Code court* avec le contenu ```[child_pages list="true" orderby="title" order="ASC"]```


### 4.1/ Les blocs utiles

Pour insérer une image avec du texte dessus (utile en début d'une page), il faut utiliser un bloc *bannière*.

Pour mettre en forme des contenus les uns à côté des autres, existe le bloc *colonnes* (attention, pour une image et un texte, utiliser le bloc *Média & texte*).

Pour mettre en place des onglets, il suffit 
* d'insérer les trois blocs *Code court* suivants :
  * ```[tabby title="Titre du premier onglet"]```
  * ```[tabby title="Titre du second onglet"]```
  * ```[tabbyending]```
* d'ajouter le contenu du premier onglet sous le titre du premier onglet
* d'ajouter le contenu du second onglet sous le titre du second onglet
* d'ajouter, à volontée, un titre et un contenu d'onglet avant le dernier *Code court*

## 5/ Les choses à savoir

Si, après avoir enregistrer des modifications, ces dernières ne sont pas visibles, il suffit de cliquer sur le bouton *Purger le cache" présent tout en haut des pages d'administration de WordPress (hors de l'éditeur de page ou d'article).
