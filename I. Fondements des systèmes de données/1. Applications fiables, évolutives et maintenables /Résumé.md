# Résumé

Dans ce chapitre, nous avons exploré certaines façons fondamentales de penser les applications à forte intensité de données. Ces principes nous guideront dans le reste du livre, où nous nous plongerons dans les détails techniques.

Pour être utile, une application doit répondre à plusieurs exigences. Il existe des exigences fonctionnelles (ce qu'elle doit faire, par exemple permettre le stockage, l'extraction, la recherche et le traitement des données de diverses manières) et des exigences non fonctionnelles (propriétés générales telles que la sécurité, la fiabilité, la conformité, l'évolutivité, la compatibilité et la maintenabilité). Dans ce chapitre, nous avons abordé en détail la fiabilité, l'évolutivité et la maintenabilité.

*La fiabilité* signifie que les systèmes doivent fonctionner correctement, même en cas de défaillance. Les défauts peuvent provenir du matériel (généralement aléatoires et non corrélés), des logiciels (les bogues sont généralement systématiques et difficiles à traiter) et des humains (qui font inévitablement des erreurs de temps en temps). Les techniques de tolérance aux pannes peuvent cacher certains types de pannes à l'utilisateur final.

*L'évolutivité* consiste à disposer de stratégies pour maintenir de bonnes performances, même lorsque la charge augmente. Afin de discuter de l'évolutivité, nous devons d'abord trouver des moyens de décrire la charge et les performances de manière quantitative. Nous avons brièvement examiné les timelines de Twitter comme exemple de description de la charge, et les percentiles de temps de réponse comme moyen de mesurer les performances. Dans un système évolutif, vous pouvez ajouter de la capacité de traitement afin de rester fiable en cas de charge élevée.

*La maintenabilité* a de nombreuses facettes, mais il s'agit essentiellement d'améliorer la vie des équipes d'ingénierie et d'exploitation qui doivent travailler avec le système. De bonnes abstractions peuvent contribuer à réduire la complexité et à rendre le système plus facile à modifier et à adapter à de nouveaux cas d'utilisation. Une bonne opérabilité signifie avoir une bonne visibilité sur la santé du système et disposer de moyens efficaces pour le gérer.

Il n'existe malheureusement pas de solution miracle pour rendre les applications fiables, évolutives ou faciles à maintenir. Cependant, il existe certains modèles et techniques qui réapparaissent constamment dans différents types d'applications. Dans les prochains chapitres, nous examinerons quelques exemples de systèmes de données et nous analyserons comment ils atteignent ces objectifs.

Plus tard dans le livre, dans la partie III, nous examinerons les modèles pour les systèmes qui consistent en plusieurs composants travaillant ensemble, comme celui de la Figure 1-1.
