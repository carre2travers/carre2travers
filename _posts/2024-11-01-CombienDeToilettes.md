---
layout: post
title: Combien de toilettes nous faut il pour assurer le confort de nos invites?
subtitle: 
category: [blog, home]
banner:
  image: /assets/images/banners/posts_banner.jpg
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
---

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

Cela va sans dire que le bien-être de nos invités nous tient extrêmement à cœur. Un anniversaire réussi signifie pour nous qu'il n'y ait pas eu une seule minute d'inconfort pour nos invités, y compris dans les moments où ils doivent disposer de leurs déchets corporels.

Mais alors, voilà, c'est la première fois que nous organisons un événement de cette ampleur. Une question d'importance capitale se pose : de combien de toilettes avons-nous vraiment besoin pour assurer le confort de nos invités ? Malgré l'intérêt commun de nos organisateurs, une discorde est née. Certains considèrent les estimations des autres complètement désproportionnées, tandis que ces autres trouvent que les estimations des certains reflètent une large sous-estimation du volume d'efflux corporel que vous (nos invités) allez produire. D'autres encore se demandent bien pourquoi on devrait s'embêter avec des toilettes (mais on les écoute pas trop, ceux-là).

Mais alors, que faire ? Une seule solution : laisser la science parler !

# Aux petits besoins les grands moyens

Le problème auquel nous faisons face peut être adressé de façon logique, avec l'aide des avancées technologiques modernes. Nous avons créé un logiciel pour simuler la production de déchets corporels dans le temps (d'une soirée ou plus), en fonction de la configuration sanitaire de l'événement. Ce logiciel porte le doux nom de H.W.M.S. : Human Waste Management Simulator. Avant de présenter les résultats de cette étude, nous allons introduire les détails techniques, pour convaincre les lecteurs de la validité de notre approche.


## Methodes

### Modélisation de l'Accumulation des Déchets Humains lors d'une Fête

#### Configuration des Installations Sanitaires

Pour évaluer l'accumulation des déchets humains lors d'une fête, nous avons développé un modèle de simulation prenant en compte la configuration des installations sanitaires et les caractéristiques des invités. La configuration sanitaire est définie par les paramètres suivants :

- Nombre de toilettes normales ($$N$$) : Représente le nombre total de toilettes conventionnelles disponibles.
- Nombre de pissoirs ($$M$$) : Indique le nombre de seaux destinés spécifiquement aux mictions d'urine.
- Capacité d'une toilette normale ($$C_{\text{toilette}}$$) : Volume maximal que peut contenir une toilette normale, exprimé en litres.
- Capacité d'un seau ($$C_{\text{seau}}$$) : Volume maximal que peut contenir un seau pour pissoirs, exprimé en litres.

La capacité totale des installations sanitaires est ainsi calculée par :

$$V_{toilettes}=N\times C_{toilette}$$

$$V_{seaux}=M×C_{seau}$$

$$V_{totale}=V_{toilettes}+V_{seaux}$$

#### Caractéristiques des invités

Les invités à la fête sont caractérisés par les paramètres suivants :

- Nombre total de personnes ($$P$$) : Nombre d'invités présents.
- Proportion d'utilisateurs de pissoirs ($$r_{\text{utilisateurs pissoirs}}$$) et d'utilisateurs de toilettes ($$r_{\text{utilisateurs toilettes}}$$), tels que $$r_{\text{utilisateurs pissoirs}} + r_{\text{utilisateurs toilettes}} = 1$$.
- Proportion de invités effectuant des défécations ($$r_{\text{défécation}}$$) : Fraction des invités susceptibles de produire des excréments lors de la fête.
- Proportion d'utilsateurs de pissoirs susceptible' d'uriner en pleine nature ($$r_{\text{bush}}$$) : Taux d'utilisateurs de pissoirs qui, en cas de débordement des seaux, choisissent de se soulager en extérieur.

Chaque participant est modélisé par deux principales activités :

1. Miction :
- Fréquence moyenne ($$\lambda_{\text{pee}}$$) : Nombre moyen d'événements de miction par jour.
- Volume moyen par événement ($$v_{\text{pee}}$$) : Quantité d'urine produite lors d'un événement de miction, exprimée en litres.
1. Défécation (si applicable) :
- Fréquence moyenne ($$\lambda_{\text{poo}}$$) : Nombre moyen d'événements de défécation par jour.
- Volume moyen par événement ($$v_{\text{poo}}$$) : Quantité d'excréments produite lors d'un événement de défécation, exprimée en litres.

Ces paramètres sont modélisés à l'aide de distributions statistiques pour refléter la variabilité individuelle entre les invités.

#### Simulation des Événements et Accumulation des Déchets

La simulation se déroule sur une période de temps divisée en intervalles réguliers ($$\Delta t$$, par exemple, une heure). À chaque intervalle de temps $$t$$, les étapes suivantes sont exécutées :

1. Détermination des Événements : Pour chaque participant, les événements de miction et de défécation sont déterminés de manière probabiliste en fonction de leurs fréquences moyennes. La probabilité qu'un participant $$i$$ effectue une miction durant l'intervalle $$t$$ est donnée par :

$$P_{pee,i}(t)=1−e^{−λ_{pee,i}\timesΔt}$$

De même, pour la défécation :

$$
P_{poo,i}(t) =  \begin{cases}
P_{poo,i}​(t)=1−e^{−λ_{poo,i}​\timesΔt} & si\ r_{defecation}=1\\
0 & \text{sinon}\\
\end{cases}
$$

Ces probabilités sont utilisées pour déterminer stochastiquement si un événement se produit pour chaque participant à chaque intervalle de temps.

1. Accumulation des Volumes :
  
##### Miction :

Si le participant est un utilisateur de pissoirs et que la capacité des pissoirs n'est pas atteinte, le volume d'urine produit est ajouté aux seaux :

$$V_{seau}(t)=V_{seau}(t−1)+v_{pee,i}\times(1+\alpha)$$

où $$\alpha$$ représente le ratio de sciure ajouté au volume des déchets.

Si les seaux sont pleins et que le participant choisit de se soulager en pleine nature ($$r_{\text{bush}} = 1$$), l'urine n'est pas ajoutée aux installations sanitaires.

Sinon, le volume est redirigé vers les toilettes normales :

$$V_{toilette}(t)=V_{toilette}(t−1)+v_{pee,i}×(1+α)$$

##### Défécation :

Si le participant effectue une défécation, le volume est directement ajouté aux toilettes normales :

$$V_{toilette}(t)=V_{toilette}(t−1)+v_{poo},i\times(1+\alpha)$$


1. Gestion des Débordements : 

Si, après ajout, le volume dans les toilettes normales dépasse leur capacité totale ($$V_{\text{toilette}}(t) > V_{\text{toilettes totales}}$$), un débordement est modélisé en limitant le volume à la capacité maximale :

$$V_{toilette}(t)=min⁡(V_{toilette}(t),V_{toilettes totales})$$

Les déchets excédentaires ne sont pas pris en compte dans le modèle.


#### Calcul du Volume Total des Déchets

À chaque intervalle de temps $t$, le volume total des déchets accumulés est calculé en sommant les volumes présents dans les pissoirs et les toilettes normales :

$$V_{total}(t)=V_{seau}(t)+V_{toilette}(t)$$


Ce calcul permet de suivre l'évolution de l'accumulation des déchets au fil du temps et d'évaluer la capacité des installations sanitaires à gérer la charge produite par les invités.
Représentation Mathématique Globale

En résumé, le modèle peut être représenté par les équations suivantes pour chaque intervalle de temps $t$ :

$$
V_{\text{seau}}(t) = V_{\text{seau}}(t-1) + \sum_{i \in M} [v_{\text{pee}, i} \times (1 + \alpha) \times \mathbb{1} \cdot \text{pee}_i(t) \times \mathbb{1} \cdot \text{seau disponible}]\\
$$

$$
V_{\text{toilette}}(t) = V_{\text{toilette}}(t-1) + \\ \sum_{i \in M} [ v_{\text{pee}, i} \times (1 + \alpha) \times \mathbb{1} \cdot \text{pee}_i(t) \times \mathbb{1} \cdot \text{seau disponible} + v_{\text{poo, i}} \times (1 + \alpha) \times \mathbb{1} \cdot \text{poo}_i(t)]
$$

$$
V_{\text{total}}(t) = V_{\text{seau}}(t) + V_{\text{toilette}}(t)
$$


Où :

- $$M$$ est l'ensemble des invités privilegiant les pissoirs.
- $$\mathbb{1}_{\cdot}$$ est la fonction indicatrice qui vaut 1 si la condition est vraie, sinon 0.
- $$\alpha$$ est le ratio de sciure ajoutée aux déchets.
- $$\text{pee}_i(t)$$ et $$\text{poo}_i(t)$$ indiquent si le participant $$i$$ effectue une miction ou une défécation à l'instant $$t$$.
- $$\text{seau disponible}$$ et $$\text{toilette utilisée}$$ indiquent si le seau est disponible pour l'ajout ou si les toilettes sont utilisées respectivement.

Ce cadre mathématique permet une représentation dynamique et probabiliste de l'accumulation des déchets humains dans le contexte d'une fête, prenant en compte les comportements individuels et les capacités des installations sanitaires.

#### Analyse Statistique et Estimation des Intervalles de Confiance

Afin de prendre en compte l'aléatoire inhérent au processus de simulation, nous avons établi des intervalles de confiance à 95 % en recourant à la méthode de bootstrapping. Cette approche statistique a été mise en œuvre en exécutant la simulation 1 000 fois indépendamment. Chaque itération de la simulation génère une distribution des volumes totaux de déchets accumulés, permettant ainsi de capturer la variabilité et l'incertitude liées aux paramètres d'entrée et aux comportements individuels des invités. À partir de ces 1 000 réplicats, nous avons calculé les quantiles à 2,5 % et 97,5 % pour chaque point temporel, définissant ainsi les bornes inférieure et supérieure des intervalles de confiance à 95 %. Cette méthodologie garantit une estimation robuste et fiable des volumes de déchets, offrant une compréhension approfondie des fluctuations possibles et des marges d'erreur associées aux résultats de la simulation. Les intervalles de confiance obtenus permettent ainsi de quantifier l'incertitude et de valider la capacité des installations sanitaires à gérer efficacement la charge en déchets générée durant l'événement simulé.

### Sélection des Paramètres de Volume et de Fréquence des Déchets

Les paramètres relatifs aux volumes et aux fréquences de production des déchets humains ont été sélectionnés en s'appuyant sur des sources fiables et des considérations de réalisme biologique. Les volumes moyens de miction et de défécation ont été déterminés en se référant aux données disponibles sur Wikipédia, qui fournissent des estimations généralement acceptées pour une population adulte en bonne santé. Ainsi, le volume moyen par événement de miction a été fixé à 0,3 litre, tandis que celui de la défécation a été établi à 0,2 litre.

Pour capturer la variabilité individuelle inhérente à ces processus biologiques, des écarts-types raisonnables ont été attribués à ces volumes. L'écart-type pour le volume de miction a été fixé à 0,1 litre, reflétant une dispersion modérée autour de la moyenne, tandis que celui pour la défécation a été déterminé à 0,03 litre, indiquant une variabilité légèrement moindre. Ces choix garantissent que la distribution des volumes reste réaliste et conforme aux observations cliniques (voire le graphique si-dessous).

En ce qui concerne les fréquences de production des déchets, les paramètres ont été établis en tenant compte des habitudes physiologiques des individus en bonne santé. Il est généralement admis qu'une personne adulte urine en moyenne 7 fois par jour et défèque une fois par jour. Par conséquent, la fréquence moyenne de miction ($\lambda_{\text{pee}}$) a été fixée à 7 événements par jour, tandis que celle de défécation ($\lambda_{\text{poo}}$) a été établie à 1 événements par jour. Ces valeurs tiennent compte des variations normales et des contextes spécifiques tels que la durée de la fête simulée.

Les écarts-types associés à ces fréquences ont également été sélectionnés pour représenter une variabilité réaliste sans introduire de valeurs extrêmes. L'écart-type pour la fréquence de miction a été fixé à 2 événement par jour, ce qui permet une fluctuation modérée autour de la moyenne. De même, l'écart-type pour la fréquence de défécation a été déterminé à 0,5 événement par jour, assurant une dispersion contrôlée des données.

Ces choix méthodologiques, basés sur des références établies et des considérations pratiques, permettent de modéliser de manière précise et fiable l'accumulation des déchets humains lors de l'événement simulé, tout en tenant compte des variations individuelles et des comportements physiologiques normaux.


## Resultats

Les résultats de cette étude sont illustrés par quatre GIF, chacun représentant l'accumulation des déchets dans les installations sanitaires au fil du temps sous différentes configurations. Chaque GIF simule un scénario distinct en variant principalement le nombre de toilettes normales disponibles, tout en maintenant constant le nombre de pissoirs. Ces visualisations dynamiques permettent d'observer en temps réel le taux de remplissage des seaux et des toilettes, mettant en évidence le moment précis où un débordement survient selon la capacité des installations choisies. En comparant ces différentes configurations, il devient possible d'identifier combien de toilettes sont nécessaires pour éviter tout débordement durant la durée de l'événement simulé. 
<figure>
<img src="{{site.baseurl | prepend: site.url}}assets/videos/simulation_2_toilettes_2_pissoirs.gif"  alt="Sim1"/>
<figcaption>Fig.1 - 2 pissoirs 2 toilettes.</figcaption>
</figure>
<figure>
<img src="{{site.baseurl | prepend: site.url}}assets/videos/simulation_3_toilettes_2_pissoirs.gif"  alt="Sim2"/>
<figcaption>Fig.1 - 2 pissoirs 3 toilettes.</figcaption>
</figure>
<figure>
<img src="{{site.baseurl | prepend: site.url}}assets/videos/simulation_4_toilettes_3_pissoirs.gif"  alt="Sim3"/>
<figcaption>Fig.1 - 3 pissoirs 4 toilettes.</figcaption>
</figure>
<figure>
<img src="{{site.baseurl | prepend: site.url}}assets/videos/simulation_4_toilettes_4_pissoirs.gif"  alt="Sim4"/>
<figcaption>Fig.1 - 4 pissoirs 4 toilettes.</figcaption>
</figure>

Nous remarquons tout d'abord que peu importe la configuration des toilettes, les toilettes conventionnelles se remplissent plus vite que les pissoirs. Cela est tout à fait attendu. En effet, les toilettes conventionnelles sont utilisées non seulement par les femmes pour uriner (qui représentent 50 % des invités dans notre simulation), mais aussi par les déféculateurs. Les pissoirs en revanche ne sont utilisés que pour les mictions. Il faut aussi remarquer que dans les cas où les pissoirs sont remplis avant les toilettes assises (deuxième et troisième images), la vitesse de remplissage des toilettes conventionnelles augmente. Cela aussi est prévisible. En cas de dépassement du volume des pissoirs, il y a un déport de certains utilisateurs des pissoirs vers les toilettes assises, tandis que d'autres iront satisfaire leurs besoins dans les buissons. Pour les simulations présentées dans les animations ci-dessus, nous avons estimé que 50 % des utilisateurs de pissoirs se déporteraient sur les toilettes assises en cas de débordement des pissoirs.

Enfin, pour répondre à nos questions principales (combien de toilettes nous faut-il et à quelle fréquence faudra-t-il vider les toilettes), il suffit de regarder l'heure à laquelle le volume total de déchets corporels excède notre capacité sanitaire (la ligne violette). Dans la configuration avec la plus petite capacité sanitaire (2 toilettes assises et 2 pissoirs), la capacité limite sera atteinte vers 2 h 30 du matin en moyenne, en assumant que les sanitaires soient vides à partir de 18 h. En d'autres termes, si les sanitaires sont vides une première fois à 18 h, le prochain vidage ne sera nécessaire qu'à 2 h du matin. En suivant les estimations les plus pessimistes (les 5 % les plus alarmants de notre simulation), il faudrait compter vider les toilettes aux alentours de 00 h 30 (minuit et demi).

Bien évidemment, augmenter le nombre de toilettes a un effet de retardement sur l'heure à laquelle un vidage sera nécessaire. Si nous optons pour 3 toilettes et 2 pissoirs, la capacité maximale ne sera atteinte qu'aux alentours de 4 h du matin. Avec 4 toilettes et 2 pissoirs, vers 7 h du matin (même si les pissoirs seront remplis à 2 h 30). Enfin, avec 4 toilettes et 4 pissoirs, la capacité maximale ne sera atteinte que vers 10 h du matin.

## Discussion

Les résultats de cette étude suggèrent que la configuration composée de quatre toilettes et de deux à trois pissoirs est adéquate pour gérer l'accumulation des déchets humains lors d'une fête de grande envergure telle que celle simulée. Cette configuration permet une gestion efficace des installations sanitaires tout au long de l'événement, tout en évitant le besoin de vidage récurrent. Toutefois, il convient de souligner que le modèle utilisé présente certaines simplifications qui peuvent limiter la précision des prédictions. En effet, le modèle actuel ne prend pas en compte l'augmentation potentielle de la consommation de liquides durant la fête, ce qui pourrait entraîner une augmentation de la fréquence des mictions et, par conséquent, des débordements. De plus, les estimations relatives à la production d'urine sont relativement libérales, basées sur une fréquence de sept mictions par jour et un volume de 300 mL par événement, totalisant ainsi 2,1 L par jour. Cette valeur dépasse la moyenne réelle de production d'urine, généralement comprise entre 800 mL et 2 000 mL par jour selon Wikipédia. En outre, la simulation ne considère pas la diminution progressive du nombre de invités actifs à mesure que la soirée avance et que les invités commencent à se reposer, ce qui devrait naturellement ralentir la production de déchets.

Pour améliorer la fiabilité des résultats, des études futures devraient enrichir le modèle en intégrant des facteurs tels que l'augmentation de la production d'urine au niveau individuel en réponse à une consommation accrue de liquides, ainsi que la réduction du nombre de producteurs actifs au fil du temps en raison de la fatigue ou du repos des invités. Ces ajustements permettraient de refléter de manière plus réaliste les dynamiques de production de déchets humains lors d'événements festifs et d'optimiser davantage la gestion des installations sanitaires.

## Disponibilité des Codes et des Données

Les scripts de simulation et les données utilisées pour cette étude sont disponibles publiquement sur le dépôt GitHub suivant : https://github.com/AlexLepauvre/HWMS. Ce dépôt contient l'intégralité du code source nécessaire pour reproduire les résultats présentés, ainsi que les ensembles de données utilisés lors des différentes simulations. Les utilisateurs intéressés peuvent accéder librement à ces ressources pour effectuer des analyses complémentaires, adapter le modèle à d'autres configurations ou contribuer à l'amélioration du simulateur. Toutes les contributions et les requêtes sont les bienvenues via les issues et les pull requests du dépôt.

