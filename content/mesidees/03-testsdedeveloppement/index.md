---
title: Tests de développement
description: Tests de développement
weight: 103
---

## 1/ Coup de gueule en introduction
J'en ai ras le bol de ce débat stérile sur les *tests unitaires*. Chacun a sa petite définition et, en fonction du contexte, utilise mal l'adjectif unitaire.

## 2/ Discutons
D'un côté, les tests sont unitaires quand le SUT (System Under Test) se limite à une classe voire une méthode. L'avantage de ces tests est qu'ils sont assez simples à rédiger car le code à tester se limite à celui de la classe (inutile de connaître le comportement de tous les autres composants du système qui peuvent être mis en jeu). Le problème est qu'il faut justement arriver à isoler le SUT et donc utiliser des techniques de bouchonnage (encore un framework que les collaborateurs du projet doivent apprendre à maîtriser).

De l'autre côté, les tests sont unitaires du moment qu'ils participent à la construction de l'application durant les travaux du développeur (j'aime les chefs de projet définissant un nombre de jours de "programmation et tests unitaires"). Dans cette définition, n'est pas pris en compte la granularité du SUT.

La troisième définition (la pire à mon sens) est le test basé sur JUnit.

Et si, pour se simplifier la vie, on parlait de tests de développement utilisant des TU, TI et TA ?

Et là, on me pose la question “Et les tests de non-régression ?”. Ma réponse est simple “Un test de non-régression a été un test de développement mais le développement est fini et le test est resté.”
