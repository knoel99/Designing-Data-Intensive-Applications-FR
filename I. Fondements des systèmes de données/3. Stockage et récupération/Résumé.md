# Résumé

Dans ce chapitre, nous avons essayé de comprendre comment les bases de données gèrent le stockage et la récupération des données. Que se passe-t-il lorsque vous stockez des données dans une base de données, et que fait la base de données lorsque vous interrogez à nouveau les données plus tard ?

À un haut niveau, nous avons vu que les moteurs de stockage se répartissent en deux grandes catégories : ceux qui sont optimisés pour le traitement des transactions (OLTP) et ceux qui sont optimisés pour l'analyse (OLAP). Il existe de grandes différences entre les modèles d'accès dans ces cas d'utilisation :

- Les systèmes OLTP sont généralement orientés vers l'utilisateur, ce qui signifie qu'ils peuvent recevoir un énorme volume de demandes. Afin de gérer la charge, les applications ne touchent généralement qu'un petit nombre d'enregistrements dans chaque requête. L'application demande des enregistrements en utilisant une sorte de clé et le moteur de stockage utilise un index pour trouver les données correspondant à la clé demandée. Le temps de recherche sur le disque est souvent le goulot d'étranglement dans ce cas.
- Les entrepôts de données et les systèmes analytiques similaires sont moins connus, car ils sont principalement utilisés par les analystes commerciaux, et non par les utilisateurs finaux. Ils traitent un volume de requêtes beaucoup plus faible que les systèmes OLTP, mais chaque requête est généralement très exigeante, nécessitant l'analyse de plusieurs millions d'enregistrements en peu de temps. La bande passante du disque (et non le temps de recherche) est souvent le goulot d'étranglement dans ce cas, et le stockage orienté colonne est une solution de plus en plus populaire pour ce type de charge de travail.

Du côté OLTP, nous avons vu des moteurs de stockage issus de deux grandes écoles de pensée :

- L'école log-structurée, qui permet uniquement d'ajouter des fichiers et de supprimer des fichiers obsolètes, mais ne met jamais à jour un fichier qui a été écrit. Bitcask, SSTables, LSM-trees, LevelDB, Cassandra, HBase, Lucene, et d'autres appartiennent à ce groupe.
- L'école de mise à jour sur place, qui traite le disque comme un ensemble de pages de taille fixe qui peuvent être écrasées. Les arbres B sont le plus grand exemple de cette philosophie, étant utilisés dans toutes les bases de données relationnelles majeures et également dans de nombreuses bases de données non relationnelles.

Les moteurs de stockage structurés en logs sont un développement relativement récent. Leur idée principale est de transformer systématiquement les écritures à accès aléatoire en écritures séquentielles sur le disque, ce qui permet un débit d'écriture plus élevé en raison des caractéristiques de performance des disques durs et des disques SSD.

Pour terminer l'aspect OLTP, nous avons fait un bref tour des structures d'indexation plus compliquées et des bases de données optimisées pour garder toutes les données en mémoire.

Nous avons ensuite fait un détour par l'intérieur des moteurs de stockage pour examiner l'architecture de haut niveau d'un entrepôt de données typique. Ce contexte illustre pourquoi les charges de travail analytiques sont si différentes de l'OLTP : lorsque vos requêtes nécessitent un balayage séquentiel d'un grand nombre de lignes, les index sont beaucoup moins pertinents. Il devient alors important de coder les données de manière très compacte, afin de minimiser la quantité de données que la requête doit lire sur le disque. Nous avons vu comment le stockage orienté colonnes permet d'atteindre cet objectif.

En tant que développeur d'applications, si vous êtes armé de ces connaissances sur les aspects internes des moteurs de stockage, vous êtes bien mieux placé pour savoir quel outil est le mieux adapté à votre application particulière. Si vous devez ajuster les paramètres de réglage d'une base de données, cette compréhension vous permet d'imaginer l'effet d'une valeur supérieure ou inférieure.

Bien que ce chapitre n'ait pas pu faire de vous un expert du réglage d'un moteur de stockage particulier, il vous a doté d'un vocabulaire et d'idées suffisants pour vous permettre de comprendre la documentation de la base de données de votre choix. 

**Notes de bas de page**

i Si toutes les clés et valeurs avaient une taille fixe, vous pourriez utiliser la recherche binaire sur un fichier de segments et éviter complètement l'index en mémoire. Cependant, dans la pratique, ils sont généralement de longueur variable, ce qui rend difficile de savoir où se termine un enregistrement et où commence le suivant si vous n'avez pas d'index.

ii L'insertion d'une nouvelle clé dans un B-tree est raisonnablement intuitive, mais la suppression d'une clé (tout en maintenant l'équilibre de l'arbre) est un peu plus complexe [2].

iii Cette variante est parfois connue sous le nom d'arbre B+, bien que l'optimisation soit si courante qu'elle n'est souvent pas distinguée des autres variantes d'arbre B.

iv La signification de online dans OLAP n'est pas claire ; elle fait probablement référence au fait que les requêtes ne sont pas seulement destinées à des rapports prédéfinis, mais que les analystes utilisent le système OLAP de manière interactive pour des requêtes exploratoires.
