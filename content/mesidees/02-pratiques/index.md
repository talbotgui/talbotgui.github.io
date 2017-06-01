---
title: Liens sur des pratiques utiles
description: Liens sur des pratiques utiles
weight: 102
---

## Pratiques de développement :
* [Refactoring tips - Martin Fowler](http://fr.slideshare.net/redigon/refactoring-1658371) 
  * document publié en 2009 mais encore d'actualité
  * les astuces commencent au slide 14
* [DesignPattern avec explication et code](https://github.com/iluwatar/java-design-patterns)
  * l'un des patterns les plus connus est le *singleton*.
    * Mais il peut être implémenté de [manière simpliste](https://github.com/iluwatar/java-design-patterns/blob/master/singleton/src/main/java/com/iluwatar/singleton/IvoryTower.java)
	* Ou [très complexe](https://github.com/iluwatar/java-design-patterns/blob/master/singleton/src/main/java/com/iluwatar/singleton/ThreadSafeDoubleCheckLocking.java)
	* Cette version est [un bon compromis](https://github.com/iluwatar/java-design-patterns/blob/master/singleton/src/main/java/com/iluwatar/singleton/ThreadSafeLazyLoadedIvoryTower.java)
  * un autre pattern très utile est [le *visiteur*](https://github.com/iluwatar/java-design-patterns/blob/master/visitor/README.md)
  * moins utilisé, [la gestion d'état](https://github.com/iluwatar/java-design-patterns/tree/master/state)
  * le [feature-toggle](https://github.com/iluwatar/java-design-patterns/tree/master/feature-toggle)
  * le [DAO](https://github.com/iluwatar/java-design-patterns/tree/master/data-access-object)
  * ...

## Documentation d'une solution, d'un langage ou d'un projet :
* Frameworks :
  * [Liste très riche de frameworks en tout genre](https://github.com/akullpp/awesome-java)
  * [Mock d'un FileSystem](https://github.com/google/jimfs)
* Outils :
  * [AsciiDoctor - générer la documentation avec Maven](http://asciidoctor.org/docs/asciidoctor-maven-plugin/)
  * [Hugo](https://gohugo.io/overview/introduction/)

## Pratiques triés par méthode/framework :
* Scrum :
  * objectifs de sprint
  * sprint backlog
  * product backlog
  * burn-down chart
  * DefinitionOfDone
* XP :
  * integration continue
  * [10 minute build](http://www.jamesshore.com/Agile-Book/ten_minute_build.html)
  * whole team (toutes les compétences au même endroit)
  * informative workspace (management visuel et partage des informations)
  * test-first programming
  * pair programming
  * incremental design
  * user story
  * slack (se garder du temps pour les impondérables, l'amélioration continue, ...)
  * refactoring
  * simple design
  * planning game
  * single code base (une seule branche, les autres durent moins de quelques heures)
  * shared code (le code n'est le domaine réservé de personne, tout le monde peut contribuer à toutes les parties)
  * code & test
  * root cause analysis (corriger les bugs et faire en sorte de ne plus les reproduire)
  * real customer involvement
  * daily deployment (en prod chaque nuit)
  * pay-per-use (c'est le meilleur feedback et il doit revenir jusqu'à l'équipe)
* LEAN :
  * réduire le gaspillage
  * Intégrer la qualité dans le processus de production
  * Créer et améliorer la connaissance
  * Différer le plus tard possible les décisions irréversibles
  * Produire rapidement une solution utile 
  * Respecter les personnes
  * Optimiser le tout
* UP :
  * itératif et incrémental piloté par les risques
  * gérer la demande par les Use Case
  * architecture par composants
  * concevoir visuellement (vue logique, vue implémentation, vue comportement, vue déploiement et vue utilisateur)
  * tester et vérifier la qualité
  * controler les changement
  * phases IECT
* Agile UP :
  * US plutot que UC
  * storyboard
  * Agile model driven development = Model Storm (réunion de 30mn de conception) + TDD
  * moins de document que UP
