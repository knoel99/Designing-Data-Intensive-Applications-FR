# Résumé

Dans ce chapitre, nous avons examiné les sujets de la cohérence et du consensus sous plusieurs angles différents. Nous avons étudié en profondeur la linéarisabilité, un modèle de cohérence populaire : son objectif est de faire en sorte que les données répliquées apparaissent comme s'il n'y avait qu'une seule copie, et que toutes les opérations agissent sur elles de manière atomique. Bien que la linéarisabilité soit attrayante parce qu'elle est facile à comprendre - elle fait en sorte qu'une base de données se comporte comme une variable dans un programme à un seul thread - elle a l'inconvénient d'être lente, en particulier dans les environnements où les retards de réseau sont importants.

Nous avons également exploré la causalité, qui impose un ordre aux événements d'un système (ce qui est arrivé avant quoi, sur la base de la cause et de l'effet). Contrairement à la linéarisation, qui place toutes les opérations dans une ligne de temps unique et totalement ordonnée, la causalité nous fournit un modèle de cohérence plus faible : certaines choses peuvent être concurrentes, de sorte que l'historique des versions ressemble à une ligne de temps avec des branchements et des fusions. La cohérence causale n'a pas le surcoût de coordination de la linéarisabilité et est beaucoup moins sensible aux problèmes de réseau.

Cependant, même si nous capturons l'ordre causal (par exemple en utilisant les timestamps de Lamport), nous avons vu que certaines choses ne peuvent pas être implémentées de cette façon : dans "Timestamp ordering is not sufficient" nous avons considéré l'exemple de s'assurer qu'un nom d'utilisateur est unique et de rejeter les enregistrements concurrents pour le même nom d'utilisateur. Si un nœud doit accepter un enregistrement, il doit savoir d'une manière ou d'une autre qu'un autre nœud n'est pas en train d'enregistrer simultanément le même nom. Ce problème nous a conduit vers le consensus.

Nous avons vu qu'atteindre le consensus signifie décider quelque chose de telle manière que tous les nœuds sont d'accord sur ce qui a été décidé, et de telle manière que la décision est irrévocable. En creusant un peu, il s'avère qu'un large éventail de problèmes sont en fait réductibles au consensus et sont équivalents les uns aux autres (dans le sens où si vous avez une solution pour l'un d'entre eux, vous pouvez facilement la transformer en une solution pour l'un des autres). De tels problèmes équivalents incluent :

Registres de comparaison et de mise à zéro linéarisables

Le registre doit décider de manière atomique s'il doit fixer sa valeur, en fonction de l'égalité de sa valeur actuelle avec le paramètre donné dans l'opération.

Validation atomique de transaction

Une base de données doit décider de valider ou d'interrompre une transaction distribuée.

Diffusion de l'ordre total

Le système de messagerie doit décider de l'ordre de diffusion des messages.

Verrous et baux

Lorsque plusieurs clients sont en course pour s'emparer d'un verrou ou d'un bail, le verrou décide lequel l'a acquis avec succès.

Service d'adhésion/coordination

Compte tenu d'un détecteur d'échec (par exemple, les délais d'attente), le système doit décider quels nœuds sont vivants et lesquels doivent être considérés comme morts parce que leurs sessions ont expiré.

Contrainte d'unicité

Lorsque plusieurs transactions essaient simultanément de créer des enregistrements conflictuels avec la même clé, la contrainte doit décider laquelle doit être autorisée et laquelle doit échouer avec une violation de la contrainte.

Tous ces éléments sont simples si vous ne disposez que d'un seul nœud ou si vous êtes prêt à attribuer la capacité de prise de décision à un seul nœud. C'est ce qui se passe dans une base de données à leader unique : tout le pouvoir de décision est confié au leader, ce qui explique pourquoi ces bases de données sont capables de fournir des opérations linéarisables, des contraintes d'unicité, un journal de réplication totalement ordonné, etc.

Cependant, si ce leader unique échoue, ou si une interruption du réseau rend le leader inaccessible, un tel système devient incapable de progresser. Il existe trois façons de gérer cette situation :

1.	Attendre que le leader se rétablisse et accepter que le système soit bloqué pendant ce temps. De nombreux coordinateurs de transactions XA/JTA choisissent cette option. Cette approche ne résout pas complètement le problème du consensus car elle ne satisfait pas la propriété de terminaison : si le leader ne se rétablit pas, le système peut être bloqué pour toujours.
2.	Basculer manuellement en demandant aux humains de choisir un nouveau nœud leader et de reconfigurer le système pour l'utiliser. De nombreuses bases de données relationnelles adoptent cette approche. Il s'agit d'une sorte de consensus par "acte de Dieu" - l'opérateur humain, extérieur au système informatique, prend la décision. La vitesse du basculement est limitée par la vitesse à laquelle les humains peuvent agir, qui est généralement plus lente que celle des ordinateurs.
3.	Utiliser un algorithme pour choisir automatiquement un nouveau leader. Cette approche nécessite un algorithme de consensus, et il est conseillé d'utiliser un algorithme éprouvé qui gère correctement les conditions de réseau défavorables [107].

Bien qu'une base de données à chef unique puisse assurer la linéarisabilité sans exécuter un algorithme de consensus à chaque écriture, elle nécessite toujours un consensus pour maintenir son chef et pour les changements de chef. Ainsi, dans un certain sens, le fait d'avoir un leader ne fait que "mettre la charrue avant les bœufs" : le consensus est toujours nécessaire, mais à un endroit différent et moins fréquemment. La bonne nouvelle est qu'il existe des algorithmes et des systèmes tolérants aux pannes pour le consensus, et nous les avons brièvement abordés dans ce chapitre.

Des outils comme ZooKeeper jouent un rôle important en fournissant un service "externalisé" de consensus, de détection des pannes et d'adhésion que les applications peuvent utiliser. Ce n'est pas facile à utiliser, mais c'est beaucoup mieux que d'essayer de développer vos propres algorithmes qui peuvent résister à tous les problèmes discutés dans le chapitre 8. Si vous vous retrouvez à vouloir faire une de ces choses qui est réductible au consensus, et que vous voulez qu'elle soit tolérante aux pannes, alors il est conseillé d'utiliser quelque chose comme ZooKeeper.

Néanmoins, tous les systèmes ne requièrent pas nécessairement le consensus : par exemple, les systèmes de réplication sans leader et multi-leader n'utilisent généralement pas le consensus global. Les conflits qui se produisent dans ces systèmes (voir "Gérer les conflits d'écriture") sont une conséquence de l'absence de consensus entre les différents leaders, mais peut-être que cela n'est pas grave : peut-être que nous devons simplement nous débrouiller sans linéarisation et apprendre à mieux travailler avec des données qui ont des histoires de versions ramifiées et fusionnées.

Ce chapitre a fait référence à un grand nombre de recherches sur la théorie des systèmes distribués. Bien que les articles théoriques et les preuves ne soient pas toujours faciles à comprendre et qu'ils reposent parfois sur des hypothèses irréalistes, ils sont extrêmement précieux pour éclairer les travaux pratiques dans ce domaine : ils nous aident à raisonner sur ce qui peut et ne peut pas être fait et à trouver les façons contre-intuitives dont les systèmes distribués sont souvent défectueux. Si vous avez le temps, les références valent la peine d'être explorées.  

Ceci nous amène à la fin de la deuxième partie de ce livre, dans laquelle nous avons couvert la réplication (chapitre 5), le partitionnement (chapitre 6), les transactions (chapitre 7), les modèles de défaillance des systèmes distribués (chapitre 8), et enfin la cohérence et le consensus (chapitre 9). Maintenant que nous avons posé des bases théoriques solides, nous nous tournerons à nouveau vers des systèmes plus pratiques dans la troisième partie, et nous verrons comment construire des applications puissantes à partir de blocs de construction hétérogènes.

