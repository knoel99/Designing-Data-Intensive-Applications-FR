# Résumé

Dans ce chapitre, nous avons exploré le sujet du traitement par lots. Nous avons commencé par étudier des outils Unix tels que awk, grep et sort, et nous avons vu comment la philosophie de conception de ces outils est reprise dans MapReduce et dans des moteurs de flux de données plus récents. Certains de ces principes de conception sont que les entrées sont immuables, les sorties sont destinées à devenir l'entrée d'un autre programme (encore inconnu), et les problèmes complexes sont résolus en composant de petits outils qui "font bien une chose".

Dans le monde Unix, l'interface uniforme qui permet à un programme d'être composé avec un autre est constituée de fichiers et de tuyaux ; dans MapReduce, cette interface est un système de fichiers distribué. Nous avons vu que les moteurs de flux de données ajoutent leurs propres mécanismes de transport de données de type pipe pour éviter de matérialiser l'état intermédiaire dans le système de fichiers distribué, mais l'entrée initiale et la sortie finale d'un travail sont encore généralement HDFS.

 Les deux principaux problèmes que les cadres de traitement par lots distribués doivent résoudre sont les suivants :

Partitionnement

Dans MapReduce, les mappeurs sont partitionnés en fonction des blocs de fichiers d'entrée. La sortie des mappeurs est repartitionnée, triée et fusionnée dans un nombre configurable de partitions de réducteurs. L'objectif de ce processus est de rassembler toutes les données liées, par exemple tous les enregistrements ayant la même clé, au même endroit.

 Les moteurs de flux de données post-MapReduce essaient d'éviter le tri à moins qu'il ne soit nécessaire, mais ils adoptent par ailleurs une approche largement similaire du partitionnement.

Tolérance aux pannes

MapReduce écrit fréquemment sur le disque, ce qui facilite la récupération d'une tâche individuelle défaillante sans avoir à redémarrer l'ensemble du travail, mais ralentit l'exécution dans le cas sans défaillance. Les moteurs de flux de données matérialisent moins l'état intermédiaire et en conservent davantage en mémoire, ce qui signifie qu'ils doivent recalculer davantage de données en cas de défaillance d'un nœud. Les opérateurs déterministes réduisent la quantité de données qui doivent être recalculées.

Nous avons discuté de plusieurs algorithmes de jointure pour MapReduce, dont la plupart sont également utilisés en interne dans les bases de données MPP et les moteurs de flux de données. Ils fournissent également une bonne illustration du fonctionnement des algorithmes de jointure :

Les jointures de tri-fusion

Chacune des entrées à joindre passe par un mappeur qui extrait la clé de jointure. En partitionnant, triant et fusionnant, tous les enregistrements ayant la même clé finissent par être envoyés au même appel du réducteur. Cette fonction peut alors produire les enregistrements joints.

Jointures de hachage de diffusion

L'une des deux entrées de jointure est petite, elle n'est donc pas partitionnée et peut être entièrement chargée dans une table de hachage. Ainsi, vous pouvez démarrer un mappeur pour chaque partition de la grande entrée de jointure, charger la table de hachage de la petite entrée dans chaque mappeur, puis parcourir la grande entrée un enregistrement à la fois, en interrogeant la table de hachage pour chaque enregistrement.

Jointures de hachage partitionnées

Si les deux entrées de jointure sont partitionnées de la même manière (en utilisant la même clé, la même fonction de hachage et le même nombre de partitions), l'approche de la table de hachage peut être utilisée indépendamment pour chaque partition.

Les moteurs de traitement par lots distribués ont un modèle de programmation délibérément restreint : les fonctions de rappel (telles que les mappeurs et les réducteurs) sont supposées être apatrides et n'avoir aucun effet secondaire visible de l'extérieur en dehors de leur sortie désignée. Cette restriction permet au cadre de dissimuler certains des problèmes difficiles des systèmes distribués derrière son abstraction : face aux pannes et aux problèmes de réseau, les tâches peuvent être réessayées en toute sécurité, et les résultats de toutes les tâches qui ont échoué sont rejetés. Si plusieurs tâches pour une partition réussissent, seule l'une d'entre elles rend son résultat visible.

Grâce au framework, votre code dans une tâche de traitement par lots n'a pas à se soucier de la mise en œuvre de mécanismes de tolérance aux pannes : le framework peut garantir que la sortie finale d'une tâche est la même que si aucune erreur ne s'était produite, même si, en réalité, plusieurs tâches ont peut-être dû être relancées. Cette sémantique fiable est beaucoup plus solide que celle des services en ligne qui traitent les demandes des utilisateurs et qui écrivent dans les bases de données comme effet secondaire du traitement d'une demande.

 La caractéristique distinctive d'une tâche de traitement par lots est qu'elle lit des données d'entrée et produit des données de sortie, sans modifier l'entrée - en d'autres termes, la sortie est dérivée de l'entrée. Il est essentiel que les données d'entrée soient limitées : elles ont une taille connue et fixe (par exemple, elles consistent en un ensemble de fichiers journaux à un moment donné ou en un instantané du contenu d'une base de données). Parce qu'il est borné, un travail sait quand il a fini de lire la totalité de l'entrée, et donc un travail se termine finalement quand il a terminé.

Dans le chapitre suivant, nous aborderons le traitement de flux, dans lequel l'entrée est non limitée, c'est-à-dire que vous avez toujours un travail, mais ses entrées sont des flux de données sans fin. Dans ce cas, une tâche n'est jamais terminée, car à tout moment, d'autres travaux peuvent encore arriver. Nous verrons que le traitement par flux et le traitement par lots sont similaires à certains égards, mais l'hypothèse de flux non limités change également beaucoup la façon dont nous construisons les systèmes. 



