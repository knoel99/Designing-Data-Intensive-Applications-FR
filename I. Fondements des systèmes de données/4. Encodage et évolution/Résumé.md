# Résumé

Dans ce chapitre, nous avons examiné plusieurs façons de transformer des structures de données en octets sur le réseau ou en octets sur le disque. Nous avons vu comment les détails de ces encodages affectent non seulement leur efficacité, mais surtout l'architecture des applications et vos options pour les déployer.

En particulier, de nombreux services doivent prendre en charge les mises à jour permanentes, c'est-à-dire qu'une nouvelle version d'un service est progressivement déployée sur quelques nœuds à la fois, au lieu d'être déployée sur tous les nœuds simultanément. Les mises à jour progressives permettent de publier les nouvelles versions d'un service sans temps d'arrêt (ce qui encourage les petites versions fréquentes plutôt que les grandes versions rares) et rendent les déploiements moins risqués (les versions défectueuses peuvent être détectées et annulées avant qu'elles n'affectent un grand nombre d'utilisateurs). Ces propriétés sont extrêmement bénéfiques pour l'évolutivité, c'est-à-dire la facilité d'apporter des modifications à une application.

Pendant les mises à jour, ou pour diverses autres raisons, nous devons supposer que différents nœuds exécutent les différentes versions du code de notre application. Il est donc important que toutes les données circulant dans le système soient codées de manière à assurer une compatibilité ascendante (le nouveau code peut lire les anciennes données) et descendante (l'ancien code peut lire les nouvelles données).

 Nous avons abordé plusieurs formats d'encodage de données et leurs propriétés de compatibilité :

- Les codages spécifiques à un langage de programmation sont limités à un seul langage de programmation et n'assurent souvent pas la compatibilité ascendante et descendante.

- Les formats textuels comme JSON, XML et CSV sont très répandus et leur compatibilité dépend de la façon dont vous les utilisez. Ils disposent de langages de schéma optionnels, qui sont parfois utiles, parfois gênants. Ces formats sont quelque peu vagues en ce qui concerne les types de données, de sorte que vous devez faire attention à des choses comme les nombres et les chaînes binaires.

- Les formats basés sur des schémas binaires comme Thrift, Protocol Buffers et Avro permettent un codage compact et efficace avec une sémantique clairement définie de compatibilité en amont et en aval. Les schémas peuvent être utiles pour la documentation et la génération de code dans les langages à typage statique. Cependant, ils présentent l'inconvénient que les données doivent être décodées avant d'être lisibles par l'homme.

Nous avons également discuté de plusieurs modes de flux de données, illustrant différents scénarios dans lesquels les codages de données sont importants :

- Bases de données, où le processus qui écrit dans la base de données encode les données et le processus qui lit dans la base de données les décode.

- Les API RPC et REST, où le client encode une requête, le serveur décode la requête et encode une réponse, et le client décode finalement la réponse.

- Le passage asynchrone de messages (à l'aide de courtiers en messages ou d'acteurs), où les nœuds communiquent en s'envoyant des messages codés par l'expéditeur et décodés par le destinataire.

Nous pouvons conclure qu'avec un peu d'attention, la compatibilité amont/aval et les mises à jour permanentes sont tout à fait réalisables. Que l'évolution de votre application soit rapide et que vos déploiements soient fréquents.

**Notes de bas de page**

i À l'exception de certains cas particuliers, comme certains fichiers à mémoire partagée ou lorsque l'on opère directement sur des données compressées (comme décrit dans "Compression de colonnes").

ii Notez que le codage n'a rien à voir avec le cryptage. Nous n'abordons pas le cryptage dans ce livre.

iii En fait, il en a trois - BinaryProtocol, CompactProtocol et DenseProtocol - bien que DenseProtocol ne soit supporté que par l'implémentation C++, il ne compte donc pas comme inter-langage [18]. En outre, il dispose également de deux formats d'encodage différents basés sur JSON [19]. Quel plaisir !

iv Pour être précis, la valeur par défaut doit être du type de la première branche de l'union, bien que ce soit une limitation spécifique d'Avro, et non une caractéristique générale des types d'union.

v À l'exception de MySQL, qui réécrit souvent une table entière même si cela n'est pas strictement nécessaire, comme mentionné dans "Schema flexibility in the document model".

vi Même au sein de chaque camp, les arguments ne manquent pas. Par exemple, HATEOAS (hypermédia comme moteur de l'état des applications), provoque souvent des discussions [35].

vii Malgré la similitude des acronymes, SOAP n'est pas une exigence de l'AOS. SOAP est une technologie particulière, alors que la SOA est une approche générale de la construction de systèmes.

