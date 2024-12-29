---
title: Feature toggle
description: Feature toggle
weight: 125
---

Un tout petit billet pour référencer l'article de Fowler sur ce sujet : https://martinfowler.com/articles/feature-toggles.html

Bonnes pratiques d'implémentation des toggles à retenir :

* découpler l'identifiant du toggle et le code activant (ou pas) la sous-fonctionnalité
  * remplacer ```Festures.isFeatureActive("version-2.0")```
  * par ```Festures.isEnvoiMailConfirmation() { return isFeatureActive("version-2.0"); }```
* limiter au maximum les ```if```. Si plusieurs sont nécessaires, il est préférable de faire des méthodes différentes voire des classes différentes.