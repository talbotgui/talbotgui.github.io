---
title: Relecture, revue et audit de code
description: Relecture, revue et audit de code
weight: 106
---

## 1/ Mes définitions

**La relecture de code** est le travail de tout développeur juste avant de commiter son travail. Tous les outils de gestion de version permettent d'afficher le contenu du commit (*git diff* par exemple). 

Une relecture permet de s'assurer
* que les noms des composants, opérations, attributs, paramètres et variables sont explicites,
* que les commentaires sont utiles et pertinents,
* que le contenu du commit correspond bien aux exigences fournies en entrées (ou au ticket/demande)
* qu'aucun fichier ne sera embarqué pour un simple saut de ligne malencontreux, 
* ...

**La revue de code** est un processus outillé qui permet de systématiser une relecture de code par un tiers entre la fin du développement et l'intégration du dit code dans les sources de l'application. 

Le processus peut être le suivant :
  * Dès qu'un développeur pousse son travail sur le repository, une demande de revue est envoyée à un tiers.
  * Ce dernier, en cliquant sur le lien dans le mail, se voit afficher le code à auditer.
  * L'auditeur parcourt le code et y rédige ses remarques.
  * Si le résultat de la revue est satisfaisant, un simple clic permet au commit d'être intégré à la branche principale.
  * Si le nombre de remarques (ou leur sévérité) est trop élevé, une notification est envoyée au développeur pour qu'il corrige son code.

Attention, ce processus n'intègre pas le build de la branche, le déploiement du(es) livrable(s) généré(s) et les tests à réaliser pour valider le développement réalisé.

**L'audit de code** est un processus consistant à faire une relecture de code de tout le code de la branche principale. Ce processus n'est pas lié à un développement précis. Mais il peut être judicieux d'en réaliser un avant ou après un évènement précis (réversibilité de TMA, montée de version majeure d'un framework, ...).

Cette pratique est tout à fait compatible avec les deux autres pratiques précédentes.

## 2/ Le plus important
Attention, on parle ici de la qualité du code. La conception, les librairies/framework utilisés, ... sont une autre affaire !!

Quelques soient les pratiques du projet (revue ou audit ou les deux), l'élément central et primordial est l'échange entre le développeur et le relecteur. Cet échange est le seul garant du bon fonctionnement de ces pratiques !!

Sans lui :
* Le relecteur ne verra pas immédiatement l'utilité de son travail. Il s'en lassera et fera de mauvaises relectures.
* Le développeur recevra une liste de remarque totalement impersonnelle. De plus, certaines remarques peuvent parfois être injustifiées. Un échange verbal peut éviter d'interminables échanges de message.
* le relecteur et le développeur ne pourront apprendre l'un de l'autre que s'ils communiquent directement l'un avec l'autre.

## 3/ Pour se simplifier la vie
Certaines règles de développement sont aisément vérifiables (avec une petite expression régulière par exemple). Pourquoi ne pas utiliser un outil pour contrôler le code ? Il existe quantité d'outil de qualimétrie pour tous les langages.
Ils ne remplacent pas la relecture humaine mais ils peuvent la simplifier en réduisant le nombre de règles et de points d'attention à contrôler.

En plus des contrôles de qualité automatiques, les actions automatiques sur le poste du développeur peuvent simplifier les relectures de code. Tous les IDE proposent de formatter le code, réorganiser les imports, ... à chaque sauvegarde de fichier. 

## 4/ Pour aller plus loin
Pour mettre en oeuvre ces bonnes idées en poussant l'interaction un cran plus loin, il faudrait que le relecteur soit à côté du développeur pour réfléchir avec lui. Le relecteur aurait alors la possibilité de faire des remarques de conception aussi. C'est du **pair programming** !
