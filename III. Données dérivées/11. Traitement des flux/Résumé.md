# Résumé

Dans ce chapitre, nous avons abordé les flux d'événements, leurs objectifs et la manière de les traiter. D'une certaine manière, le traitement de flux ressemble beaucoup au traitement par lots dont nous avons parlé au Chapitre 10, mais il est effectué en continu sur des flux non limités (sans fin) plutôt que sur une entrée de taille fixe. De ce point de vue, les courtiers en messages et les journaux d'événements sont l'équivalent en streaming d'un système de fichiers.

Nous avons passé un certain temps à comparer deux types de courtiers de messages :

Le courtier de messages de type AMQP/JMS

Le courtier attribue des messages individuels aux consommateurs, et ces derniers accusent réception des messages individuels lorsqu'ils ont été traités avec succès. Les messages sont supprimés du courtier une fois qu'ils ont été acquittés. Cette approche est appropriée en tant que forme asynchrone de RPC (voir également "Flux de données avec passage de messages"), par exemple dans une file d'attente de tâches, où l'ordre exact de traitement des messages n'est pas important et où il n'est pas nécessaire de revenir en arrière et de relire les anciens messages après leur traitement.

Courtier en messages basé sur les journaux

Le courtier attribue tous les messages d'une partition au même nœud consommateur, et délivre toujours les messages dans le même ordre. Le parallélisme est obtenu grâce au partitionnement, et les consommateurs suivent leur progression en vérifiant le décalage du dernier message qu'ils ont traité. Le courtier conserve les messages sur le disque, il est donc possible de revenir en arrière et de relire les anciens messages si nécessaire.

L'approche basée sur les journaux présente des similitudes avec les journaux de réplication que l'on trouve dans les bases de données (voir le chapitre 5) et les moteurs de stockage structurés par journaux (voir le chapitre 3). Nous avons vu que cette approche est particulièrement appropriée pour les systèmes de traitement de flux qui consomment des flux d'entrée et génèrent des états dérivés ou des flux de sortie dérivés.

En ce qui concerne l'origine des flux, nous avons discuté de plusieurs possibilités : les événements liés à l'activité de l'utilisateur, les capteurs fournissant des relevés périodiques et les flux de données (par exemple, les données de marché en finance) sont naturellement représentés sous forme de flux. Nous avons vu qu'il peut également être utile de considérer les écritures dans une base de données comme un flux : nous pouvons capturer le journal des modifications, c'est-à-dire l'historique de toutes les modifications apportées à une base de données, soit implicitement par la capture des données de modification, soit explicitement par le sourçage des événements. Le compactage du journal permet au flux de conserver une copie complète du contenu d'une base de données.

La représentation des bases de données sous forme de flux offre de puissantes possibilités d'intégration des systèmes. Vous pouvez maintenir les systèmes de données dérivés tels que les index de recherche, les caches et les systèmes d'analyse continuellement à jour en consommant le journal des modifications et en les appliquant au système dérivé. Vous pouvez même créer de nouvelles vues sur des données existantes en partant de zéro et en consommant le journal des modifications depuis le début jusqu'à aujourd'hui.

Les possibilités de maintenir l'état sous forme de flux et de rejouer les messages sont également à la base des techniques qui permettent les jointures de flux et la tolérance aux pannes dans divers cadres de traitement de flux. Nous avons abordé plusieurs objectifs du traitement de flux, notamment la recherche de modèles d'événements (traitement d'événements complexes), le calcul d'agrégations fenêtrées (analyse de flux) et la mise à jour de systèmes de données dérivés (vues matérialisées).

Nous avons ensuite abordé les difficultés liées au raisonnement temporel dans un processeur de flux, notamment la distinction entre le temps de traitement et l'horodatage des événements, ainsi que le problème du traitement des événements traînants qui arrivent après que vous pensiez que votre fenêtre était terminée.

Nous avons distingué trois types de jointures qui peuvent apparaître dans les processus de flux :

Les jointures flux-flux

Les deux flux d'entrée sont constitués d'événements d'activité, et l'opérateur de jointure recherche des événements connexes qui se produisent dans une certaine fenêtre de temps. Par exemple, il peut faire correspondre deux actions effectuées par le même utilisateur à 30 minutes d'intervalle.  Les deux entrées de jointure peuvent en fait être le même flux (une auto-jointure) si vous voulez trouver des événements liés dans ce seul flux.

Jointures de flux et de tableaux

Un flux d'entrée est constitué d'événements d'activité, tandis que l'autre est un journal des modifications de la base de données. Le journal des modifications permet de maintenir à jour une copie locale de la base de données. Pour chaque événement d'activité, l'opérateur de jointure interroge la base de données et produit un événement d'activité enrichi.

Jointures table-table

Les deux flux d'entrée sont des journaux de modifications de la base de données. Dans ce cas, chaque changement d'un côté est joint au dernier état de l'autre côté. Le résultat est un flux de modifications de la vue matérialisée de la jointure entre les deux tables.

Enfin, nous avons abordé les techniques permettant d'atteindre la tolérance aux pannes et la sémantique "exactly-once" dans un processeur de flux. Comme pour le traitement par lots, nous devons rejeter la sortie partielle de toute tâche qui échoue. Cependant, étant donné qu'un processus de flux est long et produit des résultats en continu, nous ne pouvons pas simplement éliminer tous les résultats. Au lieu de cela, un mécanisme de récupération à grain plus fin peut être utilisé, basé sur le microbatching, le checkpointing, les transactions ou les écritures idempotentes. 


