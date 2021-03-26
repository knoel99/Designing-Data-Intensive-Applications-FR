# Table des matières

- Préface

- Qui devrait lire ce livre ?

- Portée de ce livre

- Grandes lignes de ce livre

- Références et lectures complémentaires

- O'Reilly Safari

- Comment nous contacter

- Remerciements

## I. Fondements des systèmes de données

### 1. Applications fiables, évolutives et maintenables

#### Réflexion sur les systèmes de données

#### Fiabilité

- Défauts matériels

- Erreurs logicielles

- Erreurs humaines

Quelle est l'importance de la fiabilité ?

#### Évolutivité

- Description de la charge

- Description des performances

- Approches pour faire face à la charge

#### Maintenabilité

- Opérabilité : Rendre la vie facile aux opérations

- Simplicité : Gérer la complexité

- Évolutivité : Faciliter le changement

#### Résumé

### 2. Modèles de données et langages d'interrogation

#### Modèle relationnel versus modèle documentaire

- La naissance de NoSQL

- L'inadéquation objet-relationnel

- Les relations Many-to-One et Many-to-Many

- Les bases de données documentaires répètent-elles l'histoire ?

- Bases de données relationnelles et bases de données documentaires aujourd'hui

#### Langages de requête pour les données

- Les requêtes déclaratives sur le Web

- Requêtes MapReduce

#### Modèles de données de type graphique

- Graphes de propriétés

- Le langage de requête Cypher

- Requêtes graphiques en SQL

- Triple-Stores et SPARQL

- La base : Datalog

#### Résumé

### 3. Stockage et récupération

#### Les structures de données qui alimentent votre base de données

- Index de hachage

- SSTables et LSM-Trees

- B-Trees

- Comparaison des B-Trees et des LSM-Trees

- Autres structures d'indexation

#### Traitement des transactions ou analyse ?

- Entreposage de données

- Étoiles et flocons de neige : Schémas pour l'analyse

#### Stockage orienté colonnes

- Compression des colonnes

- Ordre de tri dans le stockage en colonnes

- Écriture dans un stockage orienté colonnes

- Agrégation : Cubes de données et vues matérialisées

#### Résumé

### 4. Encodage et évolution

#### Formats de codage des données

- Formats spécifiques au langage

- JSON, XML et variantes binaires

- Thrift et tampons de protocole

- Avro

- Les mérites des schémas

#### Modes de flux de données

- Flux de données à travers les bases de données

- Flux de données à travers les services : REST et RPC

- Flux de données avec passage de messages

#### Résumé

## II. Données distribuées

### 5. Réplication

#### Leaders et suiveurs

- Réplication synchrone et réplication asynchrone

- Mise en place de nouveaux suiveurs

- Gérer les pannes de nœuds

- Mise en œuvre des journaux de réplication

#### Problèmes de retard de réplication

- Lire ses propres écritures

- Lectures monotones

- Lectures de préfixes cohérents

- Solutions pour le retard de réplication

#### Réplication multi-chefs

- Cas d'utilisation de la réplication multi-leaders

- Gestion des conflits d'écriture

- Topologies de réplication multi-leaders

#### Réplication sans leader

- Écriture dans la base de données lorsqu'un nœud est hors service

- Limites de la cohérence du quorum

- Quorums Sloppy et Hinted Handoff (en anglais)

- Détection des écritures simultanées

#### Résumé

### 6. Partitionnement

#### Partitionnement et réplication

#### Partitionnement de données clé-valeur

- Partitionnement par plage de clés

- Partitionnement par hachage de clé

- Charges de travail asymétriques et atténuation des points chauds

#### Partitionnement et index secondaires

- Partitionnement des index secondaires par document

- Partitionnement des index secondaires par terme

#### Rééquilibrage des partitions

- Stratégies de rééquilibrage

- Opérations : Rééquilibrage automatique ou manuel

#### Acheminement des requêtes

- Exécution de requêtes en parallèle

#### Résumé

### 7. Transactions

#### Le concept glissant de transaction

- La signification d'ACID

- Opérations à objet unique et à objets multiples

#### Niveaux d'isolation faibles


Lecture engagée

- Isolation des instantanés et lecture répétée

- Prévention des mises à jour perdues

- Skew d'écriture et fantômes

#### Sérialisabilité

- Exécution en série réelle

- Verrouillage biphasé (2PL)

- Isolation d'instantanés sérialisables (SSI)

#### Résumé

### 8. Le problème des systèmes distribués

#### Défauts et défaillances partielles

- Cloud Computing et supercalculateurs

#### Réseaux non fiables

- Les pannes de réseau dans la pratique

- Détection des pannes

- Délais d'attente et délais non bornés

- Réseaux synchrones et asynchrones

#### Horloges non fiables

- Horloges monotones et horodateurs

- Synchronisation et précision des horloges

- Se fier à des horloges synchronisées

- Pauses dans les processus

#### Connaissance, vérité et mensonges

- La vérité est définie par la majorité

- Défauts byzantins

- Modèle de système et réalité

#### Résumé

### 9. Cohérence et consensus

#### Garanties de consistance

#### Linéarisabilité

- Qu'est- ce qui rend un système linéarisable ?

- S'appuyer sur la linéarisabilité

- Mise en œuvre de systèmes linéarisables

- Le coût de la linéarisabilité

#### Garanties d'ordonnancement

- Ordonnancement et causalité

- Ordre des numéros de séquence

- Diffusion de l'ordre total

#### Transactions distribuées et consensus

- Atomic Commit et Two- Phase Commit (2PC)

- Transactions distribuées en pratique

- Consensus tolérant aux pannes

- Services d'adhésion et de coordination

#### Résumé

## III. Données dérivées

### 10. Traitement par lots

#### Traitement par lots avec les outils Unix

- Analyse simple des journaux

- La philosophie Unix

#### MapReduce et les systèmes de fichiers distribués

- Exécution des tâches MapReduce

- Joints et regroupements côté réduction

- Joints côté carte

- Le résultat des flux de travail par lots

- Comparaison entre Hadoop et les bases de données distribuées

#### Au- delà de MapReduce

- Matérialisation de l'état intermédiaire

- Graphes et traitement itératif

- API et langages de haut niveau

#### Résumé

### 11. Traitement des flux

#### Transmettre des flux d'événements

- Systèmes de messagerie

- Journaux partitionnés

#### Bases de données et flux

- Synchronisation des systèmes

- Capture des données de changement

- Recherche d'événements

- État, flux et immuabilité

#### Traitement des flux

- Utilisations du traitement en continu

- Raisonnement temporel

- Jonction de flux

- Tolérance aux fautes

#### Résumé

### 12. L'avenir des systèmes de données

#### Intégration des données

- Combiner des outils spécialisés en dérivant des données

- Traitement par lots et en flux

#### Dégroupage des bases de données

- Composer les technologies de stockage des données

- Concevoir des applications autour du flux de données

- Observer l'état dérivé

#### Viser la correction

- L'argument de bout en bout en faveur des bases de données

- Appliquer les contraintes

- Actualité et intégrité

- Faire confiance, mais vérifier

#### Faire ce qu'il faut

- Analyse prédictive

- Vie privée et suivi

#### Résumé

- Glossaire

- Index
