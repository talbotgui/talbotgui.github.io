---
title: Créer une console Hibernate
description: Créer une console Hibernate
weight: 104
---

## 1/ Qu'est-ce ?

Une console Hibernate est un environnement de développement (IDE) permettant de requêter en HQL (ou JPQL) une base de données.

L'intérêt de cette console est de tester une requête HQL sans avoir à déclencher du code Java dans l'application ou dans un test de développement.

## 2/ Les problèmes 
Où se trouve la difficulté ? Un peu partout en fait :

* Il faut trouver comment installer le plugin Eclipse.
* La console Hibernate de JBoss Tools utilise encore un fichier *hibernate.cfg.xml* dont on a plus très souvent l'habitude.
* La recherche d'entités par présence d'annotation n'est pas disponible (c'est Spring qui le fait d'habitude).
* il faut paramétrer correctement la connexion à une base de données.

## 3/ Une solution

## 3.1/ étape 1
Pour installer le plugin, démarrer Eclipse puis
* accéder au MarketPlace via le menu *Help* > *Eclipse MarketPlace*
* faire une recherche avec le mot *jboss*
* sur la ligne *JBoss Tools xxx*, cliquer sur le bouton *install*
* parmis la liste des fonctionnalités du plugin, ne sélectionner que les packages marqués *required* et *Hibernate Tools*
* valider toutes les étapes suivantes jusqu'au redémarrage d'Eclipse (nécessaire pour prendre en compte le nouveau plugin)

## 3.2/ étape 2
Pour paramétrer la connexion à la base de données, depuis Eclipse
* ouvrir la perspective *Hibernate*
* afficher la vue *Data Source Explorer*
* faire un clic droit sur *Database connexion*
* cliquer sur *new*
* sélectionner le bon type de base de données et lui fournir un JAR contenant un driver (dans le repository Maven, *org.postgresql:postgresql* par exemple)
* renseigner les paramètres de connexion
* tester et sauvegarder la connexion

## 3.3/ étape 3
Pour rassembler la configuration dans un même endroit, il faut créer un projet Java dans Eclipse utilisant le même JDK que celui utilisé pour démarrer Eclipse.

La configuration de la console Hibernate  demande la création de deux fichiers. Donc, dans ce projet, créer les deux fichiers :
* *hibernate.properties* qui va rester vide
* *hibernate.cfg.xml* qui va contenir un de ces deux fichiers XML

Pour une BDD HSQL :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
  <session-factory name="sessionFactory">
    <property name="hibernate.connection.driver_class">org.hsqldb.jdbcDriver</property>
    <property name="hibernate.connection.password">login</property>
    <property name="hibernate.connection.url">jdbc:hsqldb:file:C:/monCheminVersMaBaseDeDonnees/idDeMaBase;readonly=true;files_readonly=true;hsqldb.lock_file=false</property>
    <property name="hibernate.connection.username">username</property>
    <property name="hibernate.dialect">org.hibernate.dialect.HSQLDialect</property>
    <mapping class="mon.package.MonEntite" />
    <mapping class="mon.package.EtToutesMesAutresEntitesSansEnOublier" />
  </session-factory>
</hibernate-configuration>
```

Pour une BDD PostgreSQL :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory name="sessionFactory">
		<property name="hibernate.connection.driver_class">org.postgresql.Driver</property>
		<property name="hibernate.connection.password">pwd</property>
		<property name="hibernate.connection.url">jdbc:postgresql://localhost:5432/mybdd</property>
		<property name="hibernate.connection.username">login</property>
		<property name="hibernate.dialect">org.hibernate.dialect.PostgreSQL82Dialect</property>
		<mapping class="mon.package.MonEntite" />
		<mapping class="mon.package.EtToutesMesAutresEntitesSansEnOublier" />
	</session-factory>
</hibernate-configuration>
```

Toujours dans ce projet, il faut ajouter les répertoires des entités :
* faire un clic droit sur le projet
* cliquer sur *Properties* puis *Java Build Path*
* dans l'onglet *source*, ajouter des *Link sources* sur les répertoires contenant les classes des entités
* dans l'onglet *Libraries*, ajouter des *external jars* permettant de faire compiler les sources (hibernate-core, javax-persistence, ... en fonction des besoins)

## 3.4/ étape 4
Pour paramétrer la console Hibernate, il reste maintenant à
* dans la vue *Hibernate Configurations*, faire un clic-droit et un *add Configuration...*
* renseigner le formulaire avec :
  * Nom : ce que vous voulez
  * Type : *Core*
  * Hibernate version : votre version d'Hibernate
  * Project : le projet précédemment créé
  * Database Connection : *Hibernate configured connection*
  * Property file : le fichier précédemment créé
  * Configuration file : le fichier précédemment créé
  * Database dialect : HSQL ou PostgreSQL

Après un petit clic sur la flèche à coté de votre configuration pour l'étendre, puis un clic sur *Session Factory*, vous devriez voir toutes vos classes persistantes.

Il est maintenant possible d'ouvrir un *HQL Editor* depuis un clic droit sur la configuration.

A partir du même endroit, il est possible de créer un schéma de votre mapping.
