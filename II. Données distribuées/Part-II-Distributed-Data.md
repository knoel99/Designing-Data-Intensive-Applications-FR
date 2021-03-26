# Partie II. Les données distribuées

*Pour qu'une technologie réussisse, la réalité doit primer sur les relations publiques, car la nature ne peut être trompée.*

Richard Feynman, Rapport de la Commission Rogers (1986)

Dans la première partie de cet ouvrage, nous avons abordé les aspects des systèmes de données qui s'appliquent lorsque les données sont stockées sur une seule machine. Dans la deuxième partie, nous passons à un niveau supérieur et posons la question suivante : que se passe-t-il si plusieurs machines sont impliquées dans le stockage et la récupération des données ?

Il existe plusieurs raisons pour lesquelles vous pouvez souhaiter répartir une base de données sur plusieurs machines :

## Évolutivité

Si votre volume de données, votre charge de lecture ou d'écriture devient plus importante que ce qu'une seule machine peut gérer, vous pouvez potentiellement répartir la charge sur plusieurs machines.

Tolérance aux pannes/haute disponibilité

Si votre application doit continuer à fonctionner même si une machine (ou plusieurs, ou le réseau, ou un centre de données entier) tombe en panne, vous pouvez utiliser plusieurs machines pour assurer la redondance. Lorsqu'une machine tombe en panne, une autre peut prendre le relais.

## Latence

Si vous avez des utilisateurs dans le monde entier, vous voudrez peut-être avoir des serveurs à différents endroits du globe afin que chaque utilisateur puisse être servi à partir d'un centre de données géographiquement proche de lui. Cela évite aux utilisateurs d'avoir à attendre que les paquets du réseau voyagent à l'autre bout du monde.

