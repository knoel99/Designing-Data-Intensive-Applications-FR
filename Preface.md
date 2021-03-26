# Préface

Si vous avez travaillé dans le domaine de l'ingénierie logicielle ces dernières années, en particulier dans les systèmes côté serveur et backend, vous avez probablement été bombardé par une pléthore de mots à la mode relatifs au stockage et au traitement des données: NoSQL ! Big Data ! Web-scale ! Sharding ! Cohérence éventuelle ! ACID ! Théorème CAP ! Services en nuage ! MapReduce ! Temps réel !


Au cours de la dernière décennie, nous avons assisté à de nombreux développements intéressants dans le domaine des bases de données, des systèmes distribués et de la façon dont nous construisons des applications à partir de ceux-ci. Les forces motrices de ces développements sont diverses :

Les sociétés Internet telles que Google, Yahoo !, Amazon, Facebook, LinkedIn, Microsoft et Twitter traitent d'énormes volumes de données et de trafic, ce qui les oblige à créer de nouveaux outils leur permettant de gérer efficacement une telle échelle.

- Les entreprises doivent faire preuve d'agilité, tester des hypothèses à moindre coût et réagir rapidement aux nouvelles connaissances du marché en maintenant des cycles de développement courts et des modèles de données flexibles.

- Les logiciels libres et open source connaissent un grand succès et sont désormais préférés aux logiciels commerciaux ou aux logiciels internes sur mesure dans de nombreux environnements.

- La vitesse d'horloge des processeurs augmente à peine, mais les processeurs multi-cœurs sont devenus la norme et les réseaux sont de plus en plus rapides. Cela signifie que le parallélisme ne peut que s'accroître.

- Même si vous travaillez dans une petite équipe, vous pouvez désormais construire des systèmes répartis sur de nombreuses machines et même sur plusieurs régions géographiques, grâce à l'infrastructure en tant que service (IaaS) telle qu'Amazon Web Services.

- On attend désormais de nombreux services qu'ils soient hautement disponibles. Les temps d'arrêt prolongés dus aux pannes ou à la maintenance sont de plus en plus inacceptables.


Les applications à forte intensité de données repoussent les limites du possible en tirant parti de ces évolutions technologiques. Une application est dite "intensive en données" si les données constituent son principal défi - la quantité de données, leur complexité ou la vitesse à laquelle elles changent - par opposition à l'application intensive en calcul, où les cycles de l'unité centrale sont le goulot d'étranglement.


Les outils et les technologies qui aident les applications à forte intensité de données à stocker et à traiter les données se sont rapidement adaptés à ces changements. De nouveaux types de systèmes de bases de données ("NoSQL") ont fait l'objet d'une grande attention, mais les files d'attente de messages, les caches, les index de recherche, les cadres de traitement par lots et en flux et les technologies connexes sont également très importants. De nombreuses applications utilisent une combinaison de ces technologies. 

Les mots à la mode qui remplissent cet espace sont un signe d'enthousiasme pour les nouvelles possibilités, ce qui est une excellente chose. Cependant, en tant qu'ingénieurs et architectes logiciels, nous devons également avoir une compréhension techniquement exacte et précise des différentes technologies et de leurs compromis si nous voulons créer de bonnes applications. Pour cela, nous devons aller au-delà des mots à la mode. 

Heureusement, derrière les changements rapides de la technologie, il existe des principes durables qui restent vrais, quelle que soit la version d'un outil particulier que vous utilisez. Si vous comprenez ces principes, vous êtes en mesure de voir où se situe chaque outil, comment l'utiliser à bon escient et comment éviter ses pièges. C'est là qu'intervient ce livre. 

L'objectif de ce livre est de vous aider à naviguer dans le paysage diversifié et en rapide évolution des technologies de traitement et de stockage des données. Il ne s'agit pas d'un tutoriel sur un outil particulier, ni d'un manuel rempli de théorie aride. Nous nous pencherons plutôt sur des exemples de systèmes de données performants : des technologies qui constituent la base de nombreuses applications populaires et qui doivent répondre chaque jour aux exigences d'évolutivité, de performance et de fiabilité en production. 

Nous nous intéresserons à l'intérieur de ces systèmes, nous découperons leurs algorithmes clés, nous discuterons de leurs principes et des compromis qu'ils doivent faire. Au cours de ce voyage, nous essaierons de trouver des façons utiles de penser aux systèmes de données - pas seulement comment ils fonctionnent, mais aussi pourquoi ils fonctionnent de cette façon, et quelles questions nous devons poser. 

Après avoir lu ce livre, vous serez en mesure de décider quel type de technologie est approprié pour quel objectif, et de comprendre comment les outils peuvent être combinés pour former la base d'une bonne architecture d'application. Vous ne serez pas prêt à construire votre propre moteur de stockage de bases de données à partir de rien, mais heureusement, cela est rarement nécessaire. Vous développerez cependant une bonne intuition de ce que vos systèmes font sous le capot, afin de pouvoir raisonner sur leur comportement, prendre de bonnes décisions de conception et dépister tout problème éventuel. 

## Qui devrait lire ce livre ? 

Si vous développez des applications qui ont une sorte de serveur/backend pour stocker ou traiter des données, et que vos applications utilisent Internet (par exemple, des applications Web, des applications mobiles ou des capteurs connectés à Internet), alors ce livre est pour vous. 

Ce livre s'adresse aux ingénieurs logiciels, aux architectes logiciels et aux responsables techniques qui aiment coder. Il est particulièrement pertinent si vous devez prendre des décisions concernant l'architecture des systèmes sur lesquels vous travaillez - par exemple, si vous devez choisir des outils pour résoudre un problème donné et déterminer la meilleure façon de les appliquer. Mais même si vous n'avez pas le choix de vos outils, ce livre vous aidera à mieux comprendre leurs forces et leurs faiblesses. 

 Vous devez avoir une certaine expérience de la création d'applications Web ou de services réseau, et vous devez être familiarisé avec les bases de données relationnelles et SQL. Les bases de données non relationnelles et autres outils liés aux données que vous connaissez sont un plus, mais ne sont pas obligatoires. Une compréhension générale des protocoles réseau courants tels que TCP et HTTP est utile. Votre choix de langage de programmation ou de framework ne fait aucune différence pour ce livre. 

Si l'une des situations suivantes s'applique à vous, ce livre vous sera utile : 
- Vous voulez apprendre à rendre les systèmes de données évolutifs, par exemple pour prendre en charge des applications Web ou mobiles avec des millions d'utilisateurs. 
- Vous devez rendre les applications hautement disponibles (en minimisant les temps d'arrêt) et robustes sur le plan opérationnel. 
- Vous cherchez des moyens de faciliter la maintenance des systèmes à long terme, même s'ils se développent et que les exigences et les technologies évoluent. 
- Vous avez une curiosité naturelle pour le fonctionnement des choses et vous voulez savoir ce qui se passe à l'intérieur des principaux sites Web et services en ligne. Ce livre présente les mécanismes internes de plusieurs bases de données et systèmes de traitement des données, et il est très amusant d'explorer les idées brillantes qui ont présidé à leur conception. 

Parfois, lorsque l'on discute de systèmes de données évolutifs, les gens font des commentaires du type : "Vous n'êtes pas Google ou Amazon. Arrêtez de vous soucier de l'échelle et utilisez simplement une base de données relationnelle". Il y a du vrai dans cette affirmation : construire pour une échelle dont vous n'avez pas besoin est un gaspillage d'efforts et peut vous enfermer dans une conception inflexible. En fait, il s'agit d'une forme d'optimisation prématurée. Cependant, il est également important de choisir le bon outil pour le travail, et les différentes technologies ont chacune leurs propres forces et faiblesses. Comme nous le verrons, les bases de données relationnelles sont importantes mais ne constituent pas le dernier mot en matière de traitement des données. 

## Portée de ce livre 

Ce livre n'a pas pour but de donner des instructions détaillées sur l'installation ou l'utilisation de progiciels ou d'API spécifiques, car il existe déjà une documentation abondante à ce sujet. Au lieu de cela, nous discutons des différents principes et compromis qui sont fondamentaux pour les systèmes de données, et nous explorons les différentes décisions de conception prises par les différents produits. 

Dans les éditions ebook, nous avons inclus des liens vers le texte intégral de ressources en ligne. Tous les liens ont été vérifiés au moment de la publication, mais malheureusement les liens ont tendance à se rompre fréquemment en raison de la nature du web. Si vous rencontrez un lien cassé, ou si vous lisez une version imprimée de ce livre, vous pouvez rechercher les références en utilisant un moteur de recherche. Pour les articles universitaires, vous pouvez rechercher le titre dans Google Scholar pour trouver des fichiers PDF en accès libre. Vous pouvez également trouver toutes les références sur https://github.com/ept/ddia-references, où nous maintenons des liens à jour.



Nous nous intéressons principalement à l'architecture des systèmes de données et à la façon dont ils sont intégrés dans des applications à forte intensité de données. Ce livre n'a pas la place de couvrir le déploiement, les opérations, la sécurité, la gestion et d'autres domaines - ce sont des sujets complexes et importants, et nous ne leur rendrions pas justice en en faisant des notes secondaires superficielles dans ce livre. Ils méritent des livres à part entière.

La plupart des technologies décrites dans ce livre relèvent de l'expression à la mode "Big Data". Cependant, le terme "Big Data" est tellement sur-utilisé et sous-défini qu'il n'est pas utile dans une discussion sérieuse sur l'ingénierie. Ce livre utilise des termes moins ambigus, tels que systèmes à nœud unique ou systèmes distribués, ou systèmes en ligne/interactifs ou systèmes hors ligne/traitement par lots.

Ce livre a un penchant pour les logiciels libres et ouverts (FOSS), car lire, modifier et exécuter le code source est un excellent moyen de comprendre comment quelque chose fonctionne en détail. Les plates-formes ouvertes réduisent également le risque de verrouillage des fournisseurs. Toutefois, lorsque cela est approprié, nous abordons également les logiciels propriétaires (logiciels à source fermée, logiciels en tant que service ou logiciels internes des entreprises qui ne sont décrits que dans la littérature mais ne sont pas publiés).



## Grandes lignes de ce livre

Ce livre est organisé en trois parties :

Dans la première partie, nous abordons les idées fondamentales qui sous-tendent la conception d'applications à forte intensité de données. Dans le chapitre 1, nous commençons par discuter de ce que nous essayons réellement d'atteindre : la fiabilité, l'évolutivité et la maintenabilité ; comment nous devons y réfléchir ; et comment nous pouvons les atteindre. Dans le chapitre 2, nous comparons plusieurs modèles de données et langages de requête différents, et nous voyons comment ils sont adaptés à différentes situations. Le chapitre 3 traite des moteurs de stockage : comment les bases de données organisent les données sur le disque de manière à pouvoir les retrouver efficacement. Le chapitre 4 aborde les formats d'encodage des données (sérialisation) et l'évolution des schémas dans le temps.

Dans la deuxième partie, nous passons des données stockées sur une seule machine aux données distribuées sur plusieurs machines. Cette évolution est souvent nécessaire pour des raisons d'évolutivité, mais elle s'accompagne d'une série de défis uniques. Nous abordons d'abord la réplication (chapitre 5), le partitionnement/sharding (chapitre 6) et les transactions (chapitre 7). Nous détaillons ensuite les problèmes liés aux systèmes distribués (chapitre 8) et ce que signifie l'obtention de la cohérence et du consensus dans un système distribué (chapitre 9).

Dans la troisième partie, nous abordons les systèmes qui dérivent certains ensembles de données à partir d'autres ensembles de données. Les données dérivées se retrouvent souvent dans des systèmes hétérogènes : lorsqu'il n'existe pas de base de données capable de tout faire correctement, les applications doivent intégrer plusieurs bases de données, caches, index, etc. différents. Dans le chapitre 10, nous commençons par une approche du traitement par lots des données dérivées, puis nous la complétons par le traitement en continu dans le chapitre 11. Enfin, au chapitre 12, nous regroupons tout et discutons des approches permettant de créer des applications fiables, évolutives et faciles à maintenir à l'avenir. 