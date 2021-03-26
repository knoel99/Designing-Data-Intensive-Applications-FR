# Résumé

Dans ce chapitre, nous avons exploré différentes manières de partitionner un grand ensemble de données en sous-ensembles plus petits. Le partitionnement est nécessaire lorsque vous avez tellement de données que leur stockage et leur traitement sur une seule machine ne sont plus possibles.

L'objectif du partitionnement est de répartir les données et la charge des requêtes de manière homogène sur plusieurs machines, en évitant les points chauds (nœuds avec une charge disproportionnée). Il faut donc choisir un schéma de partitionnement adapté à vos données et rééquilibrer les partitions lorsque des nœuds sont ajoutés ou retirés du cluster.

Nous avons abordé deux approches principales du partitionnement :

- *Le partitionnement par plage de clés*, où les clés sont triées et où une partition possède toutes les clés d'un minimum à un maximum. Le tri présente l'avantage de permettre des requêtes de plage efficaces, mais il existe un risque de points chauds si l'application accède souvent à des clés proches dans l'ordre de tri.

Dans cette approche, les partitions sont généralement rééquilibrées dynamiquement en divisant la plage en deux sous-plages lorsqu'une partition devient trop grande.

- *Le partitionnement par hachage*, où une fonction de hachage est appliquée à chaque clé, et une partition possède une gamme de hachages. Cette méthode détruit l'ordre des clés, ce qui rend les requêtes de plage inefficaces, mais peut répartir la charge de manière plus uniforme.

Lors du partitionnement par hachage, il est courant de créer un nombre fixe de partitions à l'avance, d'attribuer plusieurs partitions à chaque nœud et de déplacer des partitions entières d'un nœud à l'autre lorsque des nœuds sont ajoutés ou supprimés. Le partitionnement dynamique peut également être utilisé.

Des approches hybrides sont également possibles, par exemple avec une clé composée : utiliser une partie de la clé pour identifier la partition et une autre partie pour l'ordre de tri.

  Nous avons également discuté de l'interaction entre le partitionnement et les index secondaires. Un index secondaire doit également être partitionné, et il existe deux méthodes :

- *Les index partitionnés par document* (index locaux), où les index secondaires sont stockés dans la même partition que la clé et la valeur primaires. Cela signifie qu'une seule partition doit être mise à jour en écriture, mais qu'une lecture de l'index secondaire nécessite une diffusion/un rassemblement sur toutes les partitions.

- *Les index partitionnés par termes* (index globaux), où les index secondaires sont partitionnés séparément, en utilisant les valeurs indexées. Une entrée dans l'index secondaire peut inclure des enregistrements de toutes les partitions de la clé primaire. Lorsqu'un document est écrit, plusieurs partitions de l'index secondaire doivent être mises à jour ; cependant, une lecture peut être servie à partir d'une seule partition.

Enfin, nous avons abordé les techniques d'acheminement des requêtes vers la partition appropriée, qui vont du simple équilibrage de charge tenant compte des partitions aux moteurs d'exécution de requêtes parallèles sophistiqués.

Par conception, chaque partition fonctionne de manière indépendante, ce qui permet à une base de données partitionnée de s'adapter à plusieurs machines. Cependant, les opérations qui nécessitent d'écrire sur plusieurs partitions peuvent être difficiles à appréhender : par exemple, que se passe-t-il si l'écriture sur une partition réussit, mais qu'une autre échoue ? Nous aborderons cette question dans les chapitres suivants. 