# Chapitre 1. Applications fiables, évolutives et maintenables

Internet a été si bien réalisé que la plupart des gens le considèrent comme une ressource naturelle comme l'océan Pacifique, plutôt que comme quelque chose qui a été créé par l'homme. À quand remonte la dernière fois où une technologie d'une telle ampleur a été aussi exempte d'erreurs ?

Alan Kay, dans un entretien avec le Dr Dobb's Journal (2012)

 

Aujourd'hui, de nombreuses applications sont gourmandes en données, par opposition aux applications informatiques. La puissance brute du processeur est rarement un facteur limitant pour ces applications. Les problèmes les plus importants sont généralement la quantité de données, leur complexité et la vitesse à laquelle elles évoluent.

Une application à forte intensité de données est généralement construite à partir de blocs de construction standard qui fournissent des fonctionnalités courantes. Par exemple, de nombreuses applications doivent

- Stocker des données afin qu'elles, ou une autre application, puissent les retrouver ultérieurement (bases de données).
- Se souvenir du résultat d'une opération coûteuse, pour accélérer la lecture (caches).
- Permettre aux utilisateurs de rechercher des données par mot-clé ou de les filtrer de différentes manières (index de recherche).
- Envoyer un message à un autre processus, pour qu'il soit traité de manière asynchrone (traitement en flux).
- Croquer périodiquement une grande quantité de données accumulées (traitement par lots).

Si cela semble douloureusement évident, c'est simplement parce que ces systèmes de données sont une abstraction si réussie : nous les utilisons tout le temps sans trop y penser. Lors de la création d'une application, la plupart des ingénieurs ne songeraient pas à écrire un nouveau moteur de stockage de données à partir de zéro, car les bases de données sont un outil parfaitement adapté à cette tâche.

Mais la réalité n'est pas aussi simple. Il existe de nombreux systèmes de bases de données présentant des caractéristiques différentes, car chaque application a des exigences différentes. Il existe plusieurs approches de la mise en cache, plusieurs façons de construire des index de recherche, etc. Lors de la création d'une application, nous devons toujours déterminer quels outils et quelles approches sont les plus appropriés pour la tâche à accomplir. Et il peut être difficile de combiner les outils lorsque vous devez faire quelque chose qu'un seul outil ne peut pas faire seul.

Ce livre est un voyage à travers les principes et les aspects pratiques des systèmes de données, et la façon dont vous pouvez les utiliser pour créer des applications à forte intensité de données. Nous explorerons ce que les différents outils ont en commun, ce qui les distingue et comment ils obtiennent leurs caractéristiques.

Dans ce chapitre, nous commencerons par explorer les principes fondamentaux de ce que nous essayons d'atteindre : des systèmes de données fiables, évolutifs et faciles à maintenir. Nous clarifierons la signification de ces éléments, décrirons certaines façons d'y réfléchir et passerons en revue les éléments de base dont nous aurons besoin dans les chapitres suivants. Dans les chapitres suivants, nous continuerons couche par couche, en examinant les différentes décisions de conception à prendre en compte lorsque nous travaillons sur une application à forte intensité de données.


## Fiabilité

Tout le monde a une idée intuitive de ce que signifie la fiabilité ou le manque de fiabilité d'un objet. Pour les logiciels, les attentes typiques sont les suivantes :

- L'application remplit la fonction que l'utilisateur attendait.
- Elle peut tolérer que l'utilisateur fasse des erreurs ou utilise le logiciel de manière inattendue.
- Ses performances sont suffisamment bonnes pour le cas d'utilisation requis, sous la charge et le volume de données prévus.
- Le système empêche tout accès non autorisé et tout abus.

Si l'ensemble de ces éléments signifie "fonctionner correctement", alors nous pouvons comprendre la fiabilité comme signifiant, en gros, "continuer à fonctionner correctement, même lorsque les choses tournent mal".

Les choses qui peuvent mal tourner sont appelées des pannes, et les systèmes qui anticipent les pannes et peuvent y faire face sont dits tolérants aux pannes ou résilients. Le premier terme est légèrement trompeur : il suggère que nous pourrions rendre un système tolérant à tous les types de défaillances possibles, ce qui n'est en réalité pas réalisable.  Si la planète Terre entière (et tous les serveurs qu'elle contient) était avalée par un trou noir, la tolérance à cette défaillance nécessiterait un hébergement Web dans l'espace - bonne chance pour faire approuver ce poste budgétaire. Il est donc logique de parler de la tolérance à certains types de défaillances.

Notez qu'une défaillance n'est pas la même chose qu'une panne [2]. Une défaillance est généralement définie comme un composant du système qui s'écarte de sa spécification, alors qu'une panne survient lorsque le système dans son ensemble cesse de fournir le service requis à l'utilisateur. Il est impossible de réduire la probabilité d'une défaillance à zéro ; par conséquent, il est généralement préférable de concevoir des mécanismes de tolérance aux défaillances qui empêchent les défaillances de provoquer des pannes. Dans ce livre, nous abordons plusieurs techniques permettant de construire des systèmes fiables à partir de parties non fiables.

De manière contre-intuitive, dans de tels systèmes tolérants aux pannes, il peut être judicieux d'augmenter le taux de pannes en les déclenchant délibérément, par exemple en tuant aléatoirement des processus individuels sans avertissement. De nombreux bogues critiques sont en fait dus à une mauvaise gestion des erreurs [3] ; en provoquant délibérément des fautes, vous vous assurez que le mécanisme de tolérance aux fautes est continuellement exercé et testé, ce qui peut augmenter votre confiance dans la gestion correcte des fautes lorsqu'elles se produisent naturellement. Le Chaos Monkey de Netflix [4] est un exemple de cette approche.

Bien que nous préférions généralement tolérer les défaillances plutôt que de les prévenir, il existe des cas où il vaut mieux prévenir que guérir (par exemple, parce qu'il n'existe aucun remède). C'est le cas en matière de sécurité, par exemple : si un attaquant a compromis un système et accédé à des données sensibles, cet événement ne peut être annulé. Cependant, ce livre traite principalement des types de fautes qui peuvent être corrigées, comme décrit dans les sections suivantes.

### Défaillances matérielles

Lorsque l'on pense aux causes de défaillance d'un système, les défaillances matérielles viennent rapidement à l'esprit. Les disques durs tombent en panne, la mémoire vive devient défectueuse, le réseau électrique subit une panne générale, quelqu'un débranche le mauvais câble réseau. Quiconque a travaillé dans de grands centres de données peut vous dire que ces choses arrivent tout le temps lorsque vous avez beaucoup de machines.

Les disques durs ont un temps moyen de défaillance (MTTF) d'environ 10 à 50 ans [5, 6]. Ainsi, sur un cluster de stockage comportant 10 000 disques, il faut s'attendre à ce qu'un disque meure en moyenne par jour.

  Notre première réaction consiste généralement à ajouter de la redondance aux composants matériels individuels afin de réduire le taux de défaillance du système. Les disques peuvent être configurés en RAID, les serveurs peuvent être équipés d'une double alimentation et de processeurs remplaçables à chaud, et les centres de données peuvent être équipés de batteries et de générateurs diesel pour l'alimentation de secours. Lorsqu'un composant tombe en panne, le composant redondant peut prendre sa place pendant que le composant défectueux est remplacé. Cette approche ne peut pas empêcher complètement les problèmes matériels de provoquer des pannes, mais elle est bien comprise et permet souvent à une machine de fonctionner sans interruption pendant des années.

  Jusqu'à récemment, la redondance des composants matériels était suffisante pour la plupart des applications, car elle rend la défaillance totale d'une seule machine assez rare. Tant que vous pouvez restaurer une sauvegarde sur une nouvelle machine assez rapidement, le temps d'arrêt en cas de panne n'est pas catastrophique dans la plupart des applications. Ainsi, la redondance multi-machine n'était requise que par un petit nombre d'applications pour lesquelles la haute disponibilité était absolument essentielle.

   Cependant, avec l'augmentation des volumes de données et des demandes de calcul des applications, de plus en plus d'applications ont commencé à utiliser un plus grand nombre de machines, ce qui augmente proportionnellement le taux de défaillances matérielles. De plus, dans certaines plates-formes en nuage comme Amazon Web Services (AWS), il est assez courant que des instances de machines virtuelles deviennent indisponibles sans avertissement [7], car les plates-formes sont conçues pour privilégier la flexibilité et l'élasticitéi plutôt que la fiabilité d'une seule machine.

 On assiste donc à une évolution vers des systèmes qui peuvent tolérer la perte de machines entières, en utilisant des techniques de tolérance aux pannes logicielles de préférence ou en plus de la redondance matérielle. Ces systèmes présentent également des avantages opérationnels : un système à serveur unique nécessite un temps d'arrêt planifié si vous devez redémarrer la machine (pour appliquer les correctifs de sécurité du système d'exploitation, par exemple), alors qu'un système qui peut tolérer la défaillance d'une machine peut être corrigé un nœud à la fois, sans temps d'arrêt de l'ensemble du système (une mise à niveau continue ; voir le chapitre 4).

### Erreurs logicielles

Nous considérons généralement les erreurs matérielles comme étant aléatoires et indépendantes les unes des autres : la défaillance du disque d'une machine n'implique pas que le disque d'une autre machine va tomber en panne. Il peut y avoir de faibles corrélations (par exemple en raison d'une cause commune, telle que la température du rack de serveurs), mais il est peu probable qu'un grand nombre de composants matériels tombent en panne en même temps.

Une autre catégorie de défaillance est une erreur systématique au sein du système [8]. Ces erreurs sont plus difficiles à anticiper et, comme elles sont corrélées entre les nœuds, elles ont tendance à provoquer beaucoup plus de défaillances du système que les erreurs matérielles non corrélées [5]. En voici quelques exemples :

- Un bogue logiciel qui fait que chaque instance d'un serveur d'application se plante lorsqu'on lui donne une mauvaise entrée particulière. Par exemple, considérez la seconde intercalaire du 30 juin 2012, qui a provoqué le blocage simultané de nombreuses applications en raison d'un bogue dans le noyau Linux [9].
- Un processus emballement qui utilise une ressource partagée - temps CPU, mémoire, espace disque ou bande passante réseau.
- Un service dont dépend le système qui ralentit, ne répond plus ou commence à renvoyer des réponses corrompues.
- Les défaillances en cascade, où un petit défaut dans un composant déclenche un défaut dans un autre composant, qui à son tour déclenche d'autres défauts [10].

Les bogues à l'origine de ces types de défaillances logicielles restent souvent en sommeil pendant longtemps jusqu'à ce qu'ils soient déclenchés par un ensemble de circonstances inhabituelles. Dans ces circonstances, il s'avère que le logiciel fait une sorte d'hypothèse sur son environnement - et bien que cette hypothèse soit généralement vraie, elle finit par ne plus l'être pour une raison quelconque [11].

Il n'y a pas de solution rapide au problème des fautes systématiques dans les logiciels. De nombreuses petites choses peuvent aider : une réflexion approfondie sur les hypothèses et les interactions dans le système ; des tests complets ; l'isolation des processus ; permettre aux processus de se planter et de redémarrer ; mesurer, surveiller et analyser le comportement du système en production. Si l'on attend d'un système qu'il fournisse une certaine garantie (par exemple, dans une file d'attente de messages, que le nombre de messages entrants soit égal au nombre de messages sortants), il peut s'auto-vérifier en permanence pendant son fonctionnement et déclencher une alerte en cas d'anomalie [12].


### Erreurs humaines

Les humains conçoivent et construisent les systèmes logiciels, et les opérateurs qui assurent le fonctionnement des systèmes sont également humains. Même s'ils sont animés des meilleures intentions, les humains sont connus pour leur manque de fiabilité. Par exemple, une étude portant sur de grands services Internet a révélé que les erreurs de configuration commises par les opérateurs étaient la principale cause des pannes, alors que les défaillances matérielles (serveurs ou réseau) ne jouaient un rôle que dans 10 à 25 % des pannes [13].

Comment faire pour que nos systèmes soient fiables, malgré des humains peu fiables ? Les meilleurs systèmes combinent plusieurs approches :

- Concevoir les systèmes de manière à minimiser les possibilités d'erreur. Par exemple, des abstractions, des API et des interfaces d'administration bien conçues permettent de faire facilement "la bonne chose" et découragent "la mauvaise chose". Cependant, si les interfaces sont trop restrictives, les gens les contourneront, ce qui annulera leurs avantages.
- Découpler les endroits où les gens font le plus d'erreurs des endroits où ils peuvent provoquer des défaillances. En particulier, fournissez des environnements sandbox non productifs et complets où les gens peuvent explorer et expérimenter en toute sécurité, en utilisant des données réelles, sans affecter les utilisateurs réels.
- Tester minutieusement à tous les niveaux, des tests unitaires aux tests d'intégration de systèmes entiers et aux tests manuels [3]. Les tests automatisés sont largement utilisés, bien compris et particulièrement utiles pour couvrir les cas particuliers qui surviennent rarement dans le cadre d'un fonctionnement normal.
- Permettre une récupération rapide et facile des erreurs humaines, afin de minimiser l'impact en cas de défaillance. Par exemple, il faut faire en sorte que les changements de configuration puissent être annulés rapidement, que le nouveau code soit déployé progressivement (de sorte que tout bogue inattendu n'affecte qu'un petit sous-ensemble d'utilisateurs) et que des outils soient fournis pour recalculer les données (au cas où il s'avérerait que l'ancien calcul était incorrect).
- Mettre en place un suivi détaillé et clair, tel que des mesures de performance et des taux d'erreur. Dans d'autres disciplines d'ingénierie, on parle de télémétrie. (Une fois qu'une fusée a quitté le sol, la télémétrie est essentielle pour suivre ce qui se passe, et pour comprendre les défaillances [14]). La surveillance peut nous montrer des signaux d'alerte précoce et nous permettre de vérifier si des hypothèses ou des contraintes sont violées. Lorsqu'un problème survient, les métriques peuvent être d'une aide précieuse pour diagnostiquer le problème.
- Mettre en œuvre de bonnes pratiques de gestion et de formation - un aspect complexe et important, qui dépasse le cadre de cet ouvrage.

## Quelle est l'importance de la fiabilité ?

La fiabilité n'est pas seulement réservée aux centrales nucléaires et aux logiciels de contrôle du trafic aérien - des applications plus banales doivent également fonctionner de manière fiable. Les bogues dans les applications commerciales entraînent une perte de productivité (et des risques juridiques si les chiffres sont incorrects), et les pannes des sites de commerce électronique peuvent avoir des coûts énormes en termes de perte de revenus et d'atteinte à la réputation.

Même dans les applications "non critiques", nous avons une responsabilité envers nos utilisateurs. Prenons l'exemple d'un parent qui stocke toutes les photos et vidéos de ses enfants dans votre application photo [15]. Comment se sentiraient-ils si cette base de données était soudainement corrompue ? Sauraient-ils comment la restaurer à partir d'une sauvegarde ?

Il existe des situations dans lesquelles nous pouvons choisir de sacrifier la fiabilité afin de réduire les coûts de développement (par exemple, lors du développement d'un prototype de produit pour un marché non éprouvé) ou les coûts opérationnels (par exemple, pour un service avec une marge bénéficiaire très étroite) - mais nous devons être très conscients du moment où nous prenons des raccourcis.


## Scalabilité

Même si un système fonctionne de manière fiable aujourd'hui, cela ne signifie pas qu'il fonctionnera nécessairement de manière fiable à l'avenir. Une raison courante de dégradation est l'augmentation de la charge : le système est peut-être passé de 10 000 à 100 000 utilisateurs simultanés, ou d'un million à 10 millions. Il traite peut-être des volumes de données beaucoup plus importants qu'auparavant.

L'évolutivité est le terme que nous utilisons pour décrire la capacité d'un système à faire face à une charge accrue. Notez toutefois qu'il ne s'agit pas d'une étiquette unidimensionnelle que nous pouvons attacher à un système : il est inutile de dire "X est évolutif" ou "Y n'est pas évolutif". Parler d'évolutivité, c'est plutôt se poser des questions telles que "Si le système se développe d'une certaine manière, quelles sont nos options pour faire face à cette croissance ?" et "Comment pouvons-nous ajouter des ressources informatiques pour gérer la charge supplémentaire ?".

### Décrire la charge

Tout d'abord, nous devons décrire succinctement la charge actuelle du système ; ce n'est qu'ensuite que nous pourrons aborder les questions de croissance (que se passe-t-il si notre charge double ?). La charge peut être décrite à l'aide de quelques chiffres que nous appelons paramètres de charge. Le meilleur choix de paramètres dépend de l'architecture de votre système : il peut s'agir de requêtes par seconde sur un serveur web, du rapport entre les lectures et les écritures dans une base de données, du nombre d'utilisateurs actifs simultanément dans un salon de discussion, du taux de réussite d'un cache, etc. Peut-être que le cas moyen est ce qui compte pour vous, ou peut-être que votre goulot d'étranglement est dominé par un petit nombre de cas extrêmes.

Pour rendre cette idée plus concrète, prenons l'exemple de Twitter, en utilisant des données publiées en novembre 2012 [16]. Deux des principales opérations de Twitter sont :

*Publier un tweet*

Un utilisateur peut publier un nouveau message à ses followers (4.6k requêtes/sec en moyenne, plus de 12k requêtes/sec au pic).

*Chronologie d'accueil*

Un utilisateur peut consulter les tweets publiés par les personnes qu'il suit (300 000 demandes/seconde).

Il serait assez facile de gérer simplement 12 000 écritures par seconde (le taux de pointe pour la publication de tweets). Cependant, le problème de mise à l'échelle de Twitter n'est pas principalement dû au volume de tweets, mais au fan-outii-chaque utilisateur suit de nombreuses personnes, et chaque utilisateur est suivi par de nombreuses personnes. Il existe en gros deux façons de mettre en œuvre ces deux opérations :

1.	La publication d'un tweet insère simplement le nouveau tweet dans une collection globale de tweets. Lorsqu'un utilisateur demande sa timeline personnelle, il recherche toutes les personnes qu'il suit, trouve tous les tweets de chacun de ces utilisateurs et les fusionne (triés par ordre chronologique). Dans une base de données relationnelle comme celle de la figure 1-2, vous pourriez écrire une requête telle que :

```sql
SELECT tweets.*, users.* FROM tweets
JOIN users ON tweets.sender_id = users.id
JOIN follows ON follows.followee_id = users.id
WHERE follows.follower_id = current_user
```

2. Maintenez un cache pour la timeline personnelle de chaque utilisateur, comme une boîte aux lettres de tweets pour chaque utilisateur destinataire (voir Figure 1-3). Lorsqu'un utilisateur publie un tweet, recherchez toutes les personnes qui suivent cet utilisateur et insérez le nouveau tweet dans le cache de la ligne du temps de chacun d'eux. La demande de lecture de la chronologie personnelle est alors bon marché, car son résultat a été calculé à l'avance.
  

Figure 1-2. Schéma relationnel simple pour la mise en œuvre d'une chronologie personnelle de Twitter.

Figure 1-3. Le pipeline de données de Twitter pour la diffusion des tweets aux abonnés, avec les paramètres de chargement en novembre 2012 [16].

La première version de Twitter utilisait l'approche 1, mais les systèmes ont eu du mal à suivre la charge des requêtes de la home timeline, si bien que la société est passée à l'approche 2. Cette approche fonctionne mieux parce que le taux moyen de tweets publiés est presque deux ordres de grandeur plus bas que le taux de lecture de la timeline domestique, et donc dans ce cas il est préférable de faire plus de travail au moment de l'écriture et moins au moment de la lecture.

Cependant, l'inconvénient de l'approche 2 est que la publication d'un tweet nécessite maintenant beaucoup de travail supplémentaire. En moyenne, un tweet est transmis à environ 75 followers, donc 4,6k tweets par seconde deviennent 345k écritures par seconde dans les caches de la timeline d'origine. Mais cette moyenne cache le fait que le nombre de followers par utilisateur varie énormément, et que certains utilisateurs ont plus de 30 millions de followers. Cela signifie qu'un seul tweet peut entraîner plus de 30 millions d'écritures dans les caches de la timeline personnelle ! Faire cela en temps voulu - Twitter essaie de transmettre les tweets aux suiveurs dans les cinq secondes - est un défi de taille.

Dans l'exemple de Twitter, la distribution des followers par utilisateur (peut-être pondérée par la fréquence des tweets de ces utilisateurs) est un paramètre de charge clé pour discuter de l'évolutivité, car il détermine la charge de sortie. Votre application peut avoir des caractéristiques très différentes, mais vous pouvez appliquer des principes similaires pour raisonner sur sa charge.

Dernier rebondissement de l'anecdote Twitter : maintenant que l'approche 2 est solidement mise en œuvre, Twitter passe à un hybride des deux approches. Les tweets de la plupart des utilisateurs continuent d'être diffusés sur leur timeline personnelle au moment où ils sont postés, mais un petit nombre d'utilisateurs ayant un très grand nombre de followers (c'est-à-dire les célébrités) sont exclus de cette diffusion. Les tweets de toutes les célébrités qu'un utilisateur peut suivre sont récupérés séparément et fusionnés avec la ligne de temps personnelle de cet utilisateur lorsqu'ils sont lus, comme dans l'approche 1. Cette approche hybride est capable de fournir de bonnes performances de manière constante. Nous reviendrons sur cet exemple au chapitre 12, après avoir abordé d'autres aspects techniques.


### Décrire les performances

Une fois que vous avez décrit la charge de votre système, vous pouvez étudier ce qui se passe lorsque la charge augmente. Vous pouvez examiner cette question de deux façons :

- Lorsque vous augmentez un paramètre de charge et que les ressources du système (CPU, mémoire, bande passante réseau, etc.) restent inchangées, comment les performances de votre système sont-elles affectées ?

- Lorsque vous augmentez un paramètre de charge, de combien devez-vous augmenter les ressources si vous voulez que les performances restent inchangées ?

Ces deux questions requièrent des chiffres de performance, alors examinons brièvement la description de la performance d'un système.

 Dans un système de traitement par lots tel que Hadoop, nous nous intéressons généralement au débit, c'est-à-dire au nombre d'enregistrements que nous pouvons traiter par seconde ou au temps total nécessaire pour exécuter une tâche sur un ensemble de données d'une certaine taille.iii Dans les systèmes en ligne, le temps de réponse du service, c'est-à-dire le temps qui s'écoule entre l'envoi d'une demande par un client et la réception d'une réponse, est généralement plus important.

**LATENCE ET TEMPS DE RÉPONSE**

 La latence et le temps de réponse sont souvent utilisés comme synonymes, mais ce ne sont pas les mêmes. Le temps de réponse est ce que voit le client : outre le temps réel de traitement de la demande (le temps de service), il inclut les délais du réseau et les délais de mise en file d'attente. La latence est la durée pendant laquelle une demande attend d'être traitée, c'est-à-dire qu'elle est latente, en attente de service [17].

Même si vous ne faites que répéter la même demande plusieurs fois, vous obtiendrez un temps de réponse légèrement différent à chaque essai. En pratique, dans un système traitant une grande variété de demandes, le temps de réponse peut varier considérablement. Nous devons donc considérer le temps de réponse non pas comme un nombre unique, mais comme une distribution de valeurs que vous pouvez mesurer.

Dans la figure 1-4, chaque barre grise représente une demande adressée à un service et sa hauteur indique le temps que cette demande a pris. La plupart des requêtes sont raisonnablement rapides, mais il existe des valeurs aberrantes occasionnelles qui prennent beaucoup plus de temps. Peut-être que les requêtes lentes sont intrinsèquement plus coûteuses, par exemple parce qu'elles traitent plus de données. Mais même dans un scénario où l'on pourrait penser que toutes les requêtes prennent le même temps, il y a des variations : une latence supplémentaire aléatoire peut être introduite par un changement de contexte vers un processus d'arrière-plan, la perte d'un paquet réseau et une retransmission TCP, une pause du ramassage des ordures, un défaut de page forçant une lecture sur le disque, des vibrations mécaniques dans le rack du serveur [18], ou de nombreuses autres causes.







