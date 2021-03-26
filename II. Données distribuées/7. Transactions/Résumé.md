# Résumé

 Les transactions sont une couche d'abstraction qui permet à une application de prétendre que certains problèmes de concurrence et certains types d'erreurs matérielles et logicielles n'existent pas. Une grande catégorie d'erreurs est réduite à un simple abandon de transaction, et l'application n'a plus qu'à réessayer.

Dans ce chapitre, nous avons vu de nombreux exemples de problèmes que les transactions permettent d'éviter. Toutes les applications ne sont pas sujettes à tous ces problèmes : une application avec des schémas d'accès très simples, comme la lecture et l'écriture d'un seul enregistrement, peut probablement se passer de transactions. Cependant, pour les schémas d'accès plus complexes, les transactions peuvent réduire considérablement le nombre de cas d'erreur potentiels auxquels vous devez penser.

Sans transactions, divers scénarios d'erreur (plantage de processus, interruptions de réseau, coupures de courant, disque plein, concurrence inattendue, etc.) signifient que les données peuvent devenir incohérentes de diverses manières. ), les données peuvent devenir incohérentes de diverses manières. Par exemple, les données dénormalisées peuvent facilement se désynchroniser avec les données sources. Sans transactions, il devient très difficile de raisonner sur les effets que des accès complexes en interaction peuvent avoir sur la base de données.

Dans ce chapitre, nous avons particulièrement approfondi le sujet du contrôle de la concurrence. Nous avons abordé plusieurs niveaux d'isolation largement utilisés, en particulier la lecture engagée, l'isolation instantanée (parfois appelée lecture répétée) et la sérialisation. Nous avons caractérisé ces niveaux d'isolation en discutant de divers exemples de conditions de concurrence :

*Lecture sale*

Un client lit les écritures d'un autre client avant qu'elles n'aient été validées. Le niveau d'isolation "read committed" et les niveaux plus forts empêchent les "dirty reads".

*Écritures sales*

Un client écrase les données qu'un autre client a écrites, mais pas encore validées. Presque toutes les implémentations de transactions empêchent les écritures sales.

*Déséquilibre de lecture (lectures non répétables)*

Un client voit différentes parties de la base de données à différents moments. Ce problème est le plus souvent évité grâce à l'isolation des instantanés, qui permet à une transaction de lire un instantané cohérent à un moment donné. Il est généralement mis en œuvre avec le contrôle de concurrence multi-version (MVCC).

*Mises à jour perdues*

Deux clients effectuent simultanément un cycle de lecture-modification-écriture. L'un d'eux écrase l'écriture de l'autre sans incorporer ses modifications, ce qui entraîne une perte de données. Certaines implémentations de l'isolation instantanée empêchent cette anomalie automatiquement, tandis que d'autres nécessitent un verrouillage manuel (SELECT FOR UPDATE).

*Déséquilibre d'écriture*

Une transaction lit quelque chose, prend une décision en fonction de la valeur qu'elle a vue et écrit cette décision dans la base de données. Cependant, au moment où l'écriture est effectuée, la prémisse de la décision n'est plus vraie. Seule l'isolation sérialisable permet d'éviter cette anomalie.

*Lectures fantômes*

Une transaction lit des objets qui correspondent à une certaine condition de recherche. Un autre client effectue une écriture qui affecte les résultats de cette recherche. L'isolation des instantanés empêche les lectures fantômes directes, mais les fantômes dans le contexte de l'asymétrie d'écriture nécessitent un traitement spécial, comme des verrous de plage d'index.

Les niveaux d'isolation faibles protègent contre certaines de ces anomalies mais vous laissent, en tant que développeur de l'application, gérer les autres manuellement (par exemple, en utilisant un verrouillage explicite). Seule l'isolation sérialisable protège contre tous ces problèmes. Nous avons discuté de trois approches différentes pour mettre en œuvre des transactions sérialisables :

Exécution littérale des transactions dans un ordre sériel

Si vous pouvez faire en sorte que chaque transaction soit très rapide à exécuter, et que le débit de la transaction est suffisamment faible pour être traité sur un seul cœur de CPU, il s'agit d'une option simple et efficace.

*Verrouillage en deux phases*

Depuis des décennies, c'est la façon standard de mettre en œuvre la sérialisation, mais de nombreuses applications évitent de l'utiliser en raison de ses caractéristiques de performance.

*Isolation instantanée sérialisable (SSI)*

Un algorithme relativement nouveau qui évite la plupart des inconvénients des approches précédentes. Il utilise une approche optimiste, permettant aux transactions de se dérouler sans blocage. Lorsqu'une transaction veut commiter, elle est vérifiée, et elle est interrompue si l'exécution n'était pas sérialisable.

Les exemples de ce chapitre utilisaient un modèle de données relationnel. Toutefois, comme nous l'avons vu dans la section "The need for multi-object transactions", les transactions sont une fonctionnalité précieuse des bases de données, quel que soit le modèle de données utilisé.

Dans ce chapitre, nous avons exploré des idées et des algorithmes principalement dans le contexte d'une base de données fonctionnant sur une seule machine. Les transactions dans les bases de données distribuées ouvrent une nouvelle série de défis difficiles, que nous aborderons dans les deux prochains chapitres. 
