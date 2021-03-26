# Résumé

Dans ce chapitre, nous avons examiné la question de la réplication. La réplication peut servir plusieurs objectifs :

*Haute disponibilité*

Maintenir le système en fonctionnement, même lorsqu'une machine (ou plusieurs machines, ou un centre de données entier) tombe en panne.

*Fonctionnement déconnecté*

Permettre à une application de continuer à fonctionner lorsqu'il y a une interruption du réseau.

*Latence*

Placer les données géographiquement proches des utilisateurs, afin que ceux-ci puissent interagir avec elles plus rapidement.

*Évolutivité*

Capacité à gérer un volume de lectures plus important que celui qu'une seule machine pourrait traiter, en effectuant les lectures sur des répliques.

Bien que l'objectif soit simple - conserver une copie des mêmes données sur plusieurs machines - la réplication s'avère être un problème remarquablement délicat. Elle exige une réflexion approfondie sur la concurrence et sur toutes les choses qui peuvent mal tourner, ainsi que la gestion des conséquences de ces erreurs. Au minimum, nous devons faire face à des nœuds indisponibles et à des interruptions de réseau (sans même considérer les types de fautes plus insidieuses, comme la corruption silencieuse des données due à des bogues logiciels).

Nous avons abordé trois approches principales de la réplication :

*Réplication à chef unique*

Les clients envoient toutes les écritures à un seul nœud (le leader), qui envoie un flux d'événements de changement de données aux autres répliques (followers). Les lectures peuvent être effectuées sur n'importe quelle réplique, mais les lectures des suiveurs peuvent être périmées.

*Réplication multi-leader*

Les clients envoient chaque écriture à l'un de plusieurs nœuds leaders, qui peuvent tous accepter des écritures. Les leaders envoient des flux d'événements de changement de données les uns aux autres et à tous les nœuds suiveurs.

*Réplication sans leader*

Les clients envoient chaque écriture à plusieurs nœuds, et lisent à partir de plusieurs nœuds en parallèle afin de détecter et de corriger les nœuds dont les données sont périmées.

Chaque approche présente des avantages et des inconvénients. La réplication à un seul leader est populaire parce qu'elle est assez facile à comprendre et qu'il n'y a pas de résolution de conflit à prendre en compte. La réplication multi-leader et sans leader peut être plus robuste en présence de nœuds défectueux, d'interruptions de réseau et de pics de latence, au prix d'être plus difficile à comprendre et de ne fournir que de très faibles garanties de cohérence.

La réplication peut être synchrone ou asynchrone, ce qui a un effet profond sur le comportement du système en cas de défaillance. Bien que la réplication asynchrone puisse être rapide lorsque le système fonctionne sans problème, il est important de comprendre ce qui se passe lorsque le retard de réplication augmente et que les serveurs tombent en panne. Si un leader échoue et que vous promouvez un follower mis à jour de manière asynchrone pour devenir le nouveau leader, les données récemment engagées peuvent être perdues.

Nous avons examiné certains effets étranges qui peuvent être causés par le décalage de réplication, et nous avons discuté de quelques modèles de cohérence qui sont utiles pour décider comment une application doit se comporter en cas de décalage de réplication :

*Cohérence lecture-après-écriture*

Les utilisateurs doivent toujours voir les données qu'ils ont eux-mêmes soumises.

*Lectures monotones*

Après que les utilisateurs aient vu les données à un moment donné, ils ne devraient pas voir plus tard les données d'un moment antérieur.

*Lecture de préfixes cohérents*

Les utilisateurs doivent voir les données dans un état qui a un sens causal : par exemple, voir une question et sa réponse dans le bon ordre.

Enfin, nous avons abordé les problèmes de concurrence inhérents aux approches de réplication multi-leaders et sans leader : parce qu'elles permettent à plusieurs écritures de se produire simultanément, des conflits peuvent survenir. Nous avons examiné un algorithme qu'une base de données pourrait utiliser pour déterminer si une opération a eu lieu avant une autre, ou si elles ont eu lieu simultanément. Nous avons également abordé les méthodes de résolution des conflits en fusionnant les mises à jour simultanées.

Dans le prochain chapitre, nous continuerons à étudier les données distribuées sur plusieurs machines, par le biais de la contrepartie de la réplication : la division d'un grand ensemble de données en partitions. 
