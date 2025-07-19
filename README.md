# recommender-fantasy-football-picker
⚽️ MPG Player Recommender – Fantasy football picks optimizer Built with Python and Power BI to surface undervalued players on Mon Petit Gazon (Premier League):

WIP


demo 85s https://www.youtube.com/watch?v=ClSAfk3en2Y


demo with explanations https://www.youtube.com/watch?v=CJAY9N-eMJ4


##Contexte et objectifs 
Mon petit gazon (MPG) est un jeu de fantasy football créé en 2011. Il se joue avec six ligues européennes. L’objectif est de remporter un championnat appelé ligue en construisant son équipe via un mercato à enchères en début de partie. Lors de chaque journée de football réelle, les évènements se déroulant dans le match réel seront répercutés sur les joueurs ajoutés (cartons, buts, fautes, blessures etc). Chaque joueur sera noté et le match MPG sera remporté par l’équipe ayant les joueurs avec la meilleure note de la journée.

L’objectif du projet est de faire une analyse de données afin de répondre à  la problématique suivante :
"Quels sont les joueurs sous-cotés offrant un bon rapport qualité/prix en fonction de leurs performances et de leur coût ?"
Pourquoi cette problématique ? 
Cette problématique a été décidée car de nombreux joueurs MPG achètent les mêmes joueurs et dépensent une grosse partie de leur budget mercato sur ces derniers. Mais il doit y avoir de nombreux joueurs moins connus ou cotés qui auraient un meilleur rapport qualité/prix/performance. Sans prendre en compte les formations d’équipes possibles étant donné que cela dépend de la stratégie de chaque joueur.
Nous ne nous concentrerons donc pas sur les meilleurs joueurs les plus connus, mais des joueurs avec une faible visibilité mais à bon rendement et rapport qualité prix.
L’objectif est de déterminé un top 5 des joueurs par poste qui puissent être recommandés aux joueurs MPG lors de leur mercato en début de partie.





##Cadre

Le dataset utilisé est celui des stats officielles MPG https://www.mpgstats.fr/ pour la ligue « Premier League ». Les données sont téléchargeables librement et directement sous le format xls. Deux tableaux sont disponibles (Nouvelle Version et Ancienne Version). Nous avons choisi de télécharger les données de l’ancienne version car ce dernier est plus complet et contient plus de données. Afin d’avoir des statistiques pertinentes qui puissent dégager une meilleure tendance, les stats de la saison uniquement.
En termes de volumétrie, le fichier contient 124 colonnes et 532 lignes pour 531 joueurs.
Pour le data cleaning et processing, le fichier sera converti en csv avant utilisation. Ce format étant mieux adapté pour les actions qui seront réalisées dessus.

##Pertinence 
Nous pouvons observer que beaucoup de colonnes sont incomplètes et aussi de nombreuses données ne sont pas pertinentes pour l’analyse. Dans un premier temps, il faudra alors décider quelles sont les données qui seront utiles pour les analyses et pour répondre à notre problématique. Nous avons décidé de garder les 28 colonnes suivantes :
-	Joueur
-	Poste
-	Cote
-	Var cote
-	Enchère moyenne
-	% achat
-	% achat tour 1
-	Nombre de matchs
-	But
-	% titularisation
-	Temps (de jeu en minutes)
-	Temps moyen
-	Minute/But
-	Prix/but
-	But/Peno
-	Passes décisives
-	Occasions créées
-	Tirs
-	Tirs cadrés
-	Corner gagné
-	Ballons
-	Interceptions
-	Tacles
-	%Duel (gagnés)
-	Fautes
-	Dégagements
-	Ballon perdu
-	Grosse occasion manquée

Le football étant un sport où de nombreuses statistiques existent, il faut donc faire un choix qui nous permettra de faire notre analyse de la manière dont nous le voulons. Il faut également prendre en compte les données spécifiques à des postes précis. Notre jeu de données contient donc des statistiques globales (temps de jeu, etc) mais aussi certaines qui sont spécifiques à des postes précis (% de duel gagnés, interceptions, tacles, dégagements etc). Une des variables à prendre en compte pour le traitement des données est aussi le poste. 

##Pré-processing
Il fallait également prendre en compte les colonnes avec des valeurs nulles. En effet, de nombreuses colonnes avaient des données nulles. Ceci dépendant de nombreux facteurs (poste, titularisation, joueurs non disponibles). Mais il faut également prendre en compte le fait que tous les joueurs listés ne jouent pas. En effet il y’ a seulement 11 titulaires et 5 changements. Ce qui fait 16 joueurs par équipe et donc 320 joueurs au maximum par journée. En laissant donc 211 listés qui ne joueront potentiellement pas.

Pour traiter les valeurs nulles ou manquantes, remplacer ces valeurs par des moyennes ou médiane n’a pas vraiment de sens car nous sommes sur des performances individuelles. La méthode choisie est donc de les remplacer par 0. Ce qui est pertinent car un 0 permettra de soit communiquer une performance mauvaise ou inexistante.
Ensuite il a fallu procéder à la vérification du type de variable et au changement de certaines d’entre elles afin que les analyses puissent être faites correctement. Une fois cela effectué, une dernière vérification du type de variables et du nombre de valeurs nulles ou manquantes est faite. Nous avons pu ensuite procéder à l’analyse des données.
A noter que le traitement des données s’est fait à partir du fichier original mais qu’une autre DataFrame a été créée afin de garder les données voulues et modifier afin de conserver le fichier initial en cas de besoin.


##Visualisations et statistiques

Distribution des données 
En termes de distribution des données, il est intéressant d’analyser tout d’abord la répartition des prix par poste avec des boites à moustaches.

<img width="945" height="563" alt="image" src="https://github.com/user-attachments/assets/eee875e2-fe34-4d79-b7c4-f62e4f6f43a7" />


Nous pouvons observer que la répartition des prix par poste est inégale. Les attaquants sont ceux ayant les plus gros écarts. Pour la majorité d’entre eux, les prix sont similaires mais nous pouvons constater quelques valeurs aberrantes qui atteignent des extrêmes pour un très faible nombre de joueurs. Nous pouvons observer une tendance de répartition similaire pour les milieux offensifs. Toutefois avec des écarts moins conséquents. Concernant la défense, nous pouvons observer des écarts plus conséquents pour les centraux que pour les latéraux. Les milieux défensifs et défenseurs centraux sont ceux qui coutent le plus cher.
Les gardiens quant à eux sont ceux qui coutent le moins cher et avec le moins d’écart. Ceci peut s’expliquer que peu de gardiens soient nécessaires à l’achat, ce qui limite grandement leur coût. 
Il semblerait qu’en termes de stratégie, les joueurs MPG se basent surtout sur les milieux et les défenseurs. Ceci est également plus logique car en termes de formation, généralement les équipes sont constituées de plus de milieux et de défenseurs.

Deux autres variables à étudier sont le rapport prix/ performance avec un graphique de nuage de points.
•	Rapport prix/performance :

<img width="945" height="563" alt="image" src="https://github.com/user-attachments/assets/352a4abc-9e0f-4891-8ac2-77eee0069876" />

 

Concernant ce rapport entre variable, nous pouvons observer que certains joueurs se démarquent par leur prix élevé. Ces derniers sont tous attaquants, mais qu’en terme de performance, elle est moyenne/faible.
Nous pouvons observer que de nombreux joueurs, tous postes confondus, ont un prix faible mais avec de bonnes performances globales. Cela s’applique surtout aux gardiens, défenseurs, quelques milieux et quelques attaquants. Ceci confirme bien que notre problématique est pertinente et qu’il n’est pas nécessaire d’acheter des joueurs extrêmement chers, pour que ces derniers soient performants.

Une autre variable intéressante à étudier est celle du rapport performance globale par poste avec une représentation sous la forme de boite à moustache.

•	Performance globale par poste :

 
Nous pouvons observer que la performance par poste est très hétérogène pour tous les postes. Les attaquants ont le moins d’écart de performances mais nous pouvons observer quelques extrêmes. Ceci est similaire pour les milieux mais avec des écarts légèrement plus élevés. Pour les défenseurs les écarts sont beaucoup plus grands. Mais le poste ayant les écarts les plus grand est le poste de gardien. Ce qui est normal puisque la forme du gardien influe grandement sur le match mais également que beaucoup moins de gardiens sont utilisés. Donc ces statistiques sont faites pour un faible nombre de gardiens.
Maintenant que nous connaissons les écarts en termes de performance par poste, il serait intéressant d’approfondir et d’évaluer la variable de performance avec le temps de jeu avec un graphique de nuage de points.




<img width="945" height="529" alt="image" src="https://github.com/user-attachments/assets/fba25312-6e0d-46ad-84c3-1c14185e05c1" />



•	Temps de jeu/Performance : 

<img width="945" height="529" alt="image" src="https://github.com/user-attachments/assets/b31f78dd-1ac5-42a1-99ee-009128e2a1c1" />


Nous pouvons observer que le temps de jeu et la performance ne sont pas forcément corrélés. En effet, la tendance est que plus les joueurs jouent et plus ils ont une performance globale croissante. Mais cela dépend des postes. En effet, il faut prendre également en compte, car un remplaçant ayant peu de temps de jeu, peut tout de même avoir une performance très élevée. Car de ce que nous pouvoir voir, c’est que très peu de joueurs jouant beaucoup, ont une performance supérieure à la moyenne des autres. Nous pouvons également observer que les joueurs les plus performants ne sont pas forcément les attaquants mais plutôt les milieux ou défenseurs. En restant sur la performance, il serait intéressant d’étudier l’impact décisif d’un joueur en fonction de son prix. Ceci sera fait en comparant le nombres de buts et de passes décisives en rapport avec le prix d’achat dans un autre graphique nuage de points.





•	Rapport buts/passes décisives et prix :

<img width="945" height="566" alt="image" src="https://github.com/user-attachments/assets/abd436e9-2781-4ffa-aa1c-bf53defcb84b" />


Pour ces variables, la tendance est très claire. Les joueurs les plus décisifs sont des attaquants et milieux offensifs cumulent le plus de buts et de passes décisives. Quelques attaquants se distinguent de par leur prix élevé. Toutefois, un grand nombre de joueurs ayant de bonnes stats ont un prix bas. La conclusion que nous pouvons en tirer et que le rapport efficacité/prix n’est pas corrélé. Nous pouvons voir également qu’un certain nombre de défenseurs sont présents dans les meilleurs buteurs/passeurs. Il serait également intéressant d’observer leur efficacité défensive en fonction de leur prix dans un graphique nuage de points.










•	Rapport interceptions/prix :

<img width="945" height="572" alt="image" src="https://github.com/user-attachments/assets/4b842c6d-d841-4458-a806-b38e713a6de2" />


Là encore, la tendance qui est dégagée est que le prix d’un joueur défensif n’est pas corrélé avec ses prestations défensives. En effet nous pouvons observer que de nombreux joueurs ont un prix relativement faible mais un grand nombre d’interceptions. Et cela indépendamment qu’ils soient centraux ou latéraux.
Cela confirme la tendance que nous avons observée sur les graphiques précédents étant que le prix n’est pas fortement corrélé avec la performance.

Pour vérifier cela, nous allons faire une matrice de corrélation.







•	Matrice de corrélation :

<img width="945" height="842" alt="image" src="https://github.com/user-attachments/assets/14a2cf56-aeac-42ff-ba64-39bfcf624808" />

Cette matrice est très intéressante à observer car il s’en dégage qu’au final très peu de variables sont corrélées entre elles. Les variables prix et performances ne sont pas corrélées significativement.  Beaucoup de variables ne sont pas corrélées ou ont des corrélations négatives. 
Pour les corrélations positives, cela concerne surtout les statistiques quantitatives comme les interceptions avec le temps, le nombre de duels remportés qui impacte la performance globale, le temps de jeu qui influe sur le nombre de buts, de passes décisives. Ce qui est normal car plus un joueur joue, plus ses statistiques seront élevées.
Pour en revenir à notre problématique, cela nous permet de comprendre que le prix d’un joueur n’est pas corrélé avec sa performance. Donc de nombreux joueurs chers sont au final moins performants que des joueurs moins onéreux. L’objectif est donc de faire un top 5 des joueurs par poste à recommander aux joueurs MPG. Afin que ces derniers puissent faire un mercato plus stratégique, moins onéreux, ou leur proposer des alternatives si les joueurs les plus performants ou joueurs stars sont perdus aux enchères.

<img width="945" height="648" alt="image" src="https://github.com/user-attachments/assets/8047ae68-8176-47a0-bd79-717eb0d2df17" />

<img width="709" height="450" alt="image" src="https://github.com/user-attachments/assets/3bbb7cd5-606b-4c52-9a13-61f6c0e616a5" />


<img width="945" height="287" alt="image" src="https://github.com/user-attachments/assets/2fee7e9a-a6e6-4d3e-aa53-8d3a4df92832" />

<img width="945" height="281" alt="image" src="https://github.com/user-attachments/assets/edb814e3-91b9-45db-ac4f-b5ef52521421" />


<img width="945" height="258" alt="image" src="https://github.com/user-attachments/assets/e5e0eca2-eb82-45ea-b642-af22bf09e3f3" />


<img width="945" height="263" alt="image" src="https://github.com/user-attachments/assets/34fad9c6-b6c8-42a3-96e5-a4644d8e5fdb" />



<img width="945" height="240" alt="image" src="https://github.com/user-attachments/assets/9cea720d-145e-4c70-a0ae-8b5e21927e3d" />


<img width="945" height="245" alt="image" src="https://github.com/user-attachments/assets/db843df9-b9dd-415c-bfb6-30650ef9aa1f" />

Apres fitrage
<img width="945" height="292" alt="image" src="https://github.com/user-attachments/assets/c84b19c7-ceb0-4810-bba5-1dc088e2f10a" />



<img width="945" height="239" alt="image" src="https://github.com/user-attachments/assets/d666ee52-a161-4e08-8880-ef9cebd1eec9" />


<img width="945" height="255" alt="image" src="https://github.com/user-attachments/assets/afc50a10-8d42-4396-a55a-0fe98964dd75" />


<img width="945" height="245" alt="image" src="https://github.com/user-attachments/assets/8d9ca7e7-c685-427c-9bef-f635d88dbbf3" />


<img width="945" height="263" alt="image" src="https://github.com/user-attachments/assets/575d8740-acaa-48c2-94fb-9fb594c92514" />



<img width="945" height="230" alt="image" src="https://github.com/user-attachments/assets/ab4f0450-38db-46ac-9e35-eca418b86f13" />







##isualisation power Bi

Après avoir déterminé que le prix d’un joueur ne reflète pas sa performance. Nous allons désormais créer un Dashboard de visualisation avec l’outil Power BI, qui nous permettra de recommander le top 5 des joueurs par poste.

Nous allons utiliser les données analysées et transformées avec python lors de la partie précédente. Le fichier nommé « df_clean ».
La première étape est la transformation des données. En effet, sur Power BI, de nombreuses colonnes du fichier sont de types ABC alors que ce sont des valeurs numériques.
Une transformation des données au format numérique avec virgules doit être faite afin de pouvoir procéder à la transformation. Notons que la variable « minutes » sera en format 123 et non pas en durée car les minutes seront une des bases numériques de notre analyse. Et pourront être additionnées. En football, le raisonnement ne se fait pas en termes d’heures mais de minutes jouées sans les transformer en heures ou autre format temporel.
Ensuite une vérification sera faite pour déterminer s’il y a des erreurs ou des valeurs manquantes.
Cette visualisation nous servira à déterminer les joueurs les plus sous-cotés. Le football comportant de nombreux postes différents. Le but est de pouvoir déterminer les joueurs sous-cotés par poste en fonctions des critères les plus pertinents par poste. En effet, un attaquant n’aura pas les mêmes critères de performance qu’un gardien ou défenseur.
Dans cette visualisation, nous partirons donc d’une visualisation globale, pour ensuite pouvoir entrer dans le détail poste par poste. Le tableau de bord comporte donc 5 pages : 
-	Vue Globale
-	Vue Attaquants
-	Vue Milieux
-	Vue Défenseurs
-	Vue Gardiens
Chaque page comportera un tableau récapitulatif, qui permettra d’afficher les statistiques essentielles des joueurs dans la catégorie de poste. Et si un joueur est sélectionné sur les différents graphiques, le tableau permettra de visualiser toutes ses statistiques en simultané.








##CODE
