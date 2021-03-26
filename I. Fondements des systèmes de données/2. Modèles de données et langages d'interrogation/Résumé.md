# Résumé

Les modèles de données constituent un vaste sujet et, dans ce chapitre, nous avons jeté un coup d'œil rapide à une grande variété de modèles différents. Nous n'avons pas eu la place d'entrer dans tous les détails de chaque modèle, mais nous espérons que cet aperçu a suffi à vous mettre en appétit pour en savoir plus sur le modèle qui correspond le mieux aux exigences de votre application.

Historiquement, les données ont commencé à être représentées sous la forme d'un grand arbre (le modèle hiérarchique), mais cela ne permettait pas de représenter les relations entre plusieurs personnes, et le modèle relationnel a donc été inventé pour résoudre ce problème. Plus récemment, les développeurs ont découvert que certaines applications ne s'adaptaient pas non plus au modèle relationnel. Les nouvelles bases de données non relationnelles "NoSQL" ont divergé dans deux directions principales :

1.	Les bases de données documentaires ciblent les cas d'utilisation où les données se présentent sous forme de documents autonomes et où les relations entre un document et un autre sont rares.
1.	Les bases de données graphiques vont dans la direction opposée, ciblant des cas d'utilisation où tout est potentiellement lié à tout.
Les trois modèles (document, relationnel et graphe) sont largement utilisés aujourd'hui, et chacun est bon dans son domaine respectif. Un modèle peut être émulé en termes d'un autre modèle - par exemple, les données de graphe peuvent être représentées dans une base de données relationnelle - mais le résultat est souvent maladroit. C'est pourquoi nous disposons de différents systèmes pour différents objectifs, et non d'une solution unique et universelle.

Les bases de données documentaires et graphiques ont en commun le fait qu'elles n'imposent généralement pas de schéma pour les données qu'elles stockent, ce qui facilite l'adaptation des applications à l'évolution des besoins. Toutefois, il est fort probable que votre application continue de supposer que les données ont une certaine structure ; il s'agit simplement de savoir si le schéma est explicite (appliqué en écriture) ou implicite (géré en lecture).

Chaque modèle de données s'accompagne de son propre langage ou cadre d'interrogation, dont nous avons évoqué plusieurs exemples : SQL, MapReduce, le pipeline d'agrégation de MongoDB, Cypher, SPARQL et Datalog. Nous avons également abordé CSS et XSL/XPath, qui ne sont pas des langages d'interrogation de base de données mais présentent des parallèles intéressants.

Bien que nous ayons couvert beaucoup de terrain, de nombreux modèles de données n'ont toujours pas été mentionnés. Pour ne donner que quelques brefs exemples :

- Les chercheurs qui travaillent sur les données du génome ont souvent besoin d'effectuer des recherches de similitude de séquence, ce qui signifie prendre une très longue chaîne (représentant une molécule d'ADN) et la comparer à une grande base de données de chaînes similaires, mais non identiques. Aucune des bases de données décrites ici ne peut gérer ce type d'utilisation, c'est pourquoi les chercheurs ont écrit des logiciels spécialisés dans les bases de données génomiques comme GenBank [48].
- Les physiciens des particules font de l'analyse de données à grande échelle de type Big Data depuis des décennies, et des projets comme le Large Hadron Collider (LHC) utilisent désormais des centaines de pétaoctets ! À une telle échelle, des solutions personnalisées sont nécessaires pour éviter que le coût du matériel ne devienne incontrôlable [49].
- La recherche en texte intégral est sans doute un type de modèle de données fréquemment utilisé parallèlement aux bases de données. La recherche d'informations est un vaste sujet spécialisé que nous ne couvrirons pas en détail dans ce livre, mais nous aborderons les index de recherche au chapitre 3 et dans la partie III.

Nous devons en rester là pour l'instant. Dans le prochain chapitre, nous aborderons certains des compromis qui entrent en jeu lors de la mise en œuvre des modèles de données décrits dans ce chapitre. 

**Notes de bas de page**

i Terme emprunté à l'électronique. Chaque circuit électrique possède une certaine impédance (résistance au courant alternatif) sur ses entrées et ses sorties. Lorsque vous connectez la sortie d'un circuit à l'entrée d'un autre, le transfert de puissance à travers la connexion est maximisé si les impédances de sortie et d'entrée des deux circuits correspondent. Une désadaptation d'impédance peut entraîner des réflexions de signal et d'autres problèmes.

ii La littérature sur le modèle relationnel distingue plusieurs formes normales différentes, mais ces distinctions ne présentent guère d'intérêt pratique. En règle générale, si vous dupliquez des valeurs qui pourraient être stockées à un seul endroit, le schéma n'est pas normalisé.

iii Au moment de la rédaction du présent document, les jointures sont prises en charge par RethinkDB, mais pas par MongoDB, et uniquement par CouchDB dans les vues prédéfinies.

iv Les contraintes de clé étrangère permettent de restreindre les modifications, mais ces contraintes ne sont pas requises par le modèle relationnel. Même avec des contraintes, les jointures sur les clés étrangères sont effectuées au moment de la requête, alors que dans CODASYL, la jointure était effectivement effectuée au moment de l'insertion.

v La description originale du modèle relationnel de Codd [1] permettait en fait quelque chose de très similaire aux documents JSON dans un schéma relationnel. Il appelait cela des domaines non simples. L'idée était qu'une valeur dans une ligne ne devait pas seulement être un type de données primitif comme un nombre ou une chaîne de caractères, mais pouvait également être une relation imbriquée (table) - vous pouvez donc avoir une structure arborescente arbitrairement imbriquée comme valeur, un peu comme le support JSON ou XML qui a été ajouté à SQL plus de 30 ans plus tard.

vi IMS et CODASYL utilisaient tous deux des API d'interrogation impérative. Les applications utilisaient généralement du code COBOL pour itérer sur les enregistrements de la base de données, un enregistrement à la fois [2, 16].

vii Techniquement, Datomic utilise des 5-tuples plutôt que des triples ; les deux champs supplémentaires sont des métadonnées pour la gestion des versions.

viii Datomic et Cascalog utilisent une syntaxe d'expression S de Clojure pour Datalog. Dans les exemples suivants, nous utilisons une syntaxe Prolog, qui est un peu plus facile à lire, mais cela ne fait aucune différence fonctionnelle.