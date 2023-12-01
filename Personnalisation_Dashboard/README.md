# [ Grafana ] : Astuces pour personnaliser rapidement son tableau de bord 
Vous avez importé un ou plusieurs tableaux de bord dans Grafana, mais vous ne savez pas trop comment les personnaliser en ajoutant ou modifiant certains éléments, Voici quelques astuces qui pourraient vos être utiles.
>L'idée est de passer de ces 2 tableaux de bord BigBlueButton :
![](http://93.90.205.194/docs/bbb-dashboard/all-server-instance.jpg)
> ##
>À ce tableau de bord regroupant les éléments essentiels qui nous intéressent :
![](http://93.90.205.194/docs/bbb-dashboard/final-dashboard-3.jpg)
> La nécessité de concaténer les informations vient du fait que l'essentiel du monitoring sera observé depuis un ordinateur portable, l'objectif est donc de centraliser les informations essentielles sans pour autant dénaturer la lisibilité du Dashboard.
> ##

## Création d'un nouveau Dashboard

>![](http://93.90.205.194/docs/bbb-dashboard/new-dashboard.png)
>Nous voilà donc avec notre nouveau Dashboard, il n'est pas nécessaire d'ajouter une nouvelle visualisation pour l'instant.
> ##

## Importer des Panels
> Commençons plutôt par copier les panels qui nous intéressent dans les Dashboard que nous avons importé.
> 
> Pour cela rien de plus "simple", cliquez sur les 3 petits points du panel que vous souhaitez copier, aller dans l'onglet **< More >** tout en bas, puis **< Copy >**:
![](http://93.90.205.194/docs/bbb-dashboard/copy-panel.png)
> ##
> Il ne reste plus qu'à vous rendre dans votre nouveau Dashboard, cliquez sur **< Add >** en haut en gauche du Dahsboard, puis **< Paste Panel >**
![](http://93.90.205.194/docs/bbb-dashboard/paste-panel-2.png)
> ##
> Et nous voilà avec nos premiers panels, mais vide !
![](http://93.90.205.194/docs/bbb-dashboard/empty-panel.png)
> ##
> C'est parce qu'il nous faut implémenter les variables sur le Dashboard, les variables apparaissent en haut du Dashboard et correspondent à ce que vous pouvez voir sur l'image ci-dessous:
![](http://93.90.205.194/docs/bbb-dashboard/variables.png)
> ###
> Vous devrez constater qu'elles sont présentes dans le Dashboard importé mais pas dans le nouveau.
> ##

## Créer/Copier des Variables
 >Je vous recommande pour cette opération d'ouvrir un onglet pour chacun des Dashboards, cela facilitera grandement la copie ou réplication de variables d'un Dashboard à l'autre.
 >
> Sur la page principale, cliquer sur les **< paramètres >** en haut à droite représente par un engrenage.
> ![](http://93.90.205.194/docs/bbb-dashboard/parameters-gear.png)
> ##
> Puis dans le menu tout à gauche, cliquez sur **< Variables >**
> ![](http://93.90.205.194/docs/bbb-dashboard/variables-settings.png)
> Répétez l'opération pour chacun des Dashboards, le but sera de recréer les mêmes variables sur votre nouveau Dashboard qui est actuellement vierge.
> 
 > Cliquez sur chaque variable pour en voir les propriétés puis recopier l'ensemble des informations présentes sur votre nouveau Dashboard de telle sorte que les données soient similaires.
 > 
 > Ne vous inquiétez pas si vous n'arriver pas à cocher la case : **< $data source >** dans **< Query Options>**, il peut afficher **< Prometheus >** par défaut.![](http://93.90.205.194/docs/bbb-dashboard/query-datasource-old.png)  
 > ##
 > Une fois toutes les variables copiées, n'oubliez pas de sauvegarder immédiatement (en haut à droite) pour ne pas perdent les modifications.![](http://93.90.205.194/docs/bbb-dashboard/variable-save-dashboard.png)
 > ##
 > Vous pouvez maintenant retourner dans l'affichage principal et constater que les données sont bien remontées cette fois-ci !
 > ![](http://93.90.205.194/docs/bbb-dashboard/dashboard-first-import.jpg)
 > Vous n'avez plus qu'à récupérer tous les panels qui vous intéressent.
 > ##
 
## Personnalisation des Panels
> Voici les différents petits Tips que je mets en place pour obtenir les informations que je souhaite récupérer ou exclure de mon panel.
> ## Inclusion / Exclusion d'instances
> Si vous prenez l'exemple du panel ci-dessous, l'on voit que toutes les instances incluses dans mon **< node exporté >** remontent, y compris mon Prometheus **(localhost : 9100)** lui-même:
> ![](http://93.90.205.194/docs/bbb-dashboard/all-server-cpu.jpg)
> ###
> Pour ma part je ne souhaite voir apparaître uniquement les instances **. cloudns.eu**
> L'on va donc éditer ce panel et modifier sa **query metrics**
>  Voici la requête que l'on trouve:
> ![](http://93.90.205.194/docs/bbb-dashboard/all-server-cpu-query-normal.png)
> 
> Elle récupère l'ensemble des informations contenues dans la metric **node_cpu_seconds_total** et y appliquent différents filtres, comme **< Job=$job node exporter >** qui inclut seulement les metrics de la variable **< $job node exporter >** qui vaut **node**.
> 
> Pour ne pas garder seulement les valeurs qui vous intéressent vous pouvez soit exclure la valeur qui dérange, soit inclure celles qui vous intéressent.
> ##

> ## Inclusion: 
> ![](http://93.90.205.194/docs/bbb-dashboard/all-server-cpu-query-include.png)On ajoute un filtre **< instance >** à notre requête, pensez bien au **< =~ >**, qui nous permet d'utiliser des **expressions régulières comme **< .\* >**, qui **"englobe toutes les occurrences"** précédentes ou suivantes selon qu'il soit placé avant ou après votre valeur de référence.
> 
> Donc la syntaxe **<.\*cloudns.\*>** signifie que toutes les occurrences suivantes et précédentes à **< cloudns >** sont acceptées, pour simplifier, tout ce qui contient **< cloudns >** sera accepté.
> ##

> ## Exclusion
> ![](http://93.90.205.194/docs/bbb-dashboard/all-server-cpu-query-exclude.png)
> De la même manière que l'inclusion, on peut également exclure les instances que l'on ne souhaite pas voir s'afficher sur ce panel avec le symbole **< !~ >** .
> 
> Vous pouvez cliquer sur **< Apply >** pour appliquer les modifications, n'oubliez pas de sauvegarder votre Dashboard au risque de perdre vos modifications à chaque actualisation de la page web.
> ##

## Gestion des instances sur le Panel
> Certains  Panels  reconfigurent  ne  sont  pas  toujours  **"architectures"**  de  la  manière  que  l'on  souhaite,  l'on  aimerait  parfois  avoir  tous  les  **graphes**  de  nos  **instances**  sur  le même  Dashboard  (en  parallèle)  sans  avoir  besoin  de  sélectionner  la  bonne  instance  à  chaque  fois  comme  dans  l'image  ci-dessous:
> ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection.jpg)
> ###
> Ou l'inverse ! Avoir toutes les instances sur le même Dashboard et vouloir les séparer instance par instance.
> ##

> ## Afficher toutes les Instances
> Éditions le Panel de notre Panel en image ci-dessus:
> ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-old.png)
> 
> Cette requête utilise la fonction **< avg() >** (average) pour calculer la moyenne des valeurs remontées par la metric **< node_memory_MemAvailable_bytes>** et **<node_memory_MemTotal_bytes>** et le opères pour en calculer un pourcentage.
>
>L'on voit également qu'il filtre les metrics reçu par le contenu de la variable **< $instance >**, qui est liée à la valeur indiquée par **Instance** en haut de notre Dashboard.
>Il nous suffit donc de la retirer pour recevoir les metrics de toutes les instances:
>![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-try.png)
>
> Résultat:
>![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-try-res.png)
> ##
> La requête a pris en compte l'intégralité des instances, les a confondu ensemble pour en faire une grosse moyenne globale. Si l'on souhaite **séparer les moyennes par instance** malgré tout. il faudra ajouter **By (instance)** derrière chacun requête:
> ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-new.png)
> 
> Résultat:
> ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-new-res.jpg)
> 
> Parfait ! Si vous voyez les instances mais pas les jauges, changez de visualisation puis revenez dessus au cas où ce n'est pas juste un soucis d'affichage.
> ##

> ## Sélection de l'Instance à afficher
> Si vous êtes dans le cas de figure inverse, et ne souhaitez afficher l'instance que par sélection depuis le Dashboard, vous devrez effectuer l’opération oppose.
> C'est à dire enlever **by (instance)**, en rajoutant le filtre **instance=$instance**
> (Dans notre cas nous nous focalisons sur les instances, mais le procédons et ### répercutable sur d'autres propriétés, je vous laisse rechercher et tester de votre cote).

## Personnalisation du Label
> Sur  notre  Dashboard  ci-dessus,  chaque  jauge  a  un  nom  (Label),  pas  très  élégant  **({instance="bigblue..."})**,  il  existe  plusieurs  façons  de  personnaliser  l'affichage  (transformations  etc...).
>
> Ici nous irons simplement dans les options juste en dessous de notre metric :
> ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-new-options.png)
> 
> J'ai modifié la valeur de **< Legend >**, pour y mettre **{{instance}}**, ce qui affichera le nom de l'instance sur chaque jauge affiliée:
> ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-new-label.jpg)
> ## 

## Personnalisation du Graphisme
> Le  "plus  gros"  a  été  fait,  dans  le  sens  où  nos  données  s'affichent  comme  nous  le  souhaitons,  mais  il  reste  une partie essentielle  qui  est  d'avoir  une vue claire et efficace  de  ce  que  nous  affichons. Dans  mon  cas,  l'affichage  doit  être  la  plus  complète  et  évidente  à  lire  sur  un  écran  de  portable  14".  Déjà  que  les  3  jauges  apparaissent  petites,  mais  si  je  passe  à  4  voir  5  jauges  cela  deviendra  illisible  !
>  ##

> ## Changement de Visualisation
>  Voici comment j'ai mis en place une solution plus pertinente:
>  J'ai sélectionné la visualisation **< Bar Gauge >**
>  ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-new-gauge-3.jpg)
>  ##
>  Elle prend encore trop de largeur à mon goût, beaucoup d'espaces "gaspillés" dans la largeur des jauges (elles sont modifiables également), mais mettons la plutôt à **< l'horizontal >**, en sélection l'affichage **< Basic >** dans les paramètres de visualisations:
>  ![](http://93.90.205.194/docs/bbb-dashboard/server-instance-selection-panel-new-gauge-4.jpg)
> ##  
>  Mieux, vous pouvez faire vos propres essais pour trouver ce qui vous correspond le mieux, cela peut prendre un peu de temps de prise en main.
>  Je vous recommande vraiment de penser à **sauvegarder** et **dupliquer** le panel avant de poursuivre les modifications surtout lorsqu'une de vos approches vous plaît bien mais que souhaiter quand même continuer à explorer. Cela vous évitera de tous recommencer !
>  ##

> ## Configuration de la valeur de sortie 
>  Penser à bien identifier et paramétrer le format des valeurs de sortie que vous souhaitez voir s'afficher, le **< Min >** et le **< Max >** sont indispensables pour équilibrer la répartition de la jauge sur le graphe:
>  ![](http://93.90.205.194/docs/bbb-dashboard/visu-panel-percent.png)
>  ##

 ## Personnalisation des couleurs
> ## Palette classique
> Juste en dessous nous pouvons personnaliser nos couleurs, il existe déjà des palettes de couleurs standard et des dégradés de couleurs qui évolueront en fonction des intervalles **[min, max]** que vous avez paramétré juste au dessus:
> ![](http://93.90.205.194/docs/bbb-dashboard/visu-panel-color.jpg)
> ##

>  ## Palette Personnalisée
>  Vous avez également pour les plus perfectionnistes d’entre vous l'option **< From thresholds (by value) >** qui permet de personnaliser chaque couleurs en fonction d'un seuil que l'on peut définir juste en dessous:
>  ![](http://93.90.205.194/docs/bbb-dashboard/visu-panel-thresholds.png)
>  Cliquez (ou double-cliquez) sur la couleur de **< Base >** pour choisir la couleur d'affichage par défaut.
>   
>   Ajoutez autant de couleur de **seuil** que vous souhaitez en cliquant sur **< + Add threshold >**, et définissez la valeur de seuil à partir duquel la couleur se déclenchera:
>   ![](http://93.90.205.194/docs/bbb-dashboard/visu-panel-color-perso.jpg)
>    
>   Voila une première transformation intéressante de notre panel "classique" vers un second un peu plus élaboré et correspondant plus a nos besoins !
>   ##

## Tips: Tableau vers Graph
> 
> ![](http://93.90.205.194/docs/bbb-dashboard/table-graph-storage.jpg)
>  ##
>  Éditez votre Tableau, et sélectionnez la visualisation **< Bar Panel >**
>  ![](http://93.90.205.194/docs/bbb-dashboard/bar-panel-all.jpg)
> ##
> Arrondissons légèrement les contours de nos jauges rectangulaires en donnant un **Radius=0.05**:
> ![](http://93.90.205.194/docs/bbb-dashboard/bar-radius.jpg)
>  ##
>  Personnalisation du label si besoin avec **< X Axis >**:
>  (Peut empêcher la gestion de l’opacité par la suite)
> ![](http://93.90.205.194/docs/bbb-dashboard/bar-labels.jpg)
> ##
> Afficher ou Masquer la légende tout en bas en décochant **< Visiblity >**:
> ![](http://93.90.205.194/docs/bbb-dashboard/bar-legend.png)
>  ##
>  Gestion de l’opacité, passez le **< Gradiant mode >** en **< Opacity >**:
>  ![](http://93.90.205.194/docs/bbb-dashboard/bar-opacity.jpg)
>  ##
>  Puis comme plus haut paramétrer vos intervalles **min** et **max** dans la section **< Standard Options >**, n'oubliez pas de modifier le **< Color Scheme >** à la valeur **< From Thresholds >**
>  ##
>  Mettez à jour vos couleurs dans la section **< Thresholds >** tout en bas, pensez également à en ajouter un second pour définir le seuil limite.
>  ![](http://93.90.205.194/docs/bbb-dashboard/bar-thresholds.jpg)
>  #
>  Pour finir, définir dans la section **<>** encore plus bas la valeur à **<>**
>  ![](http://93.90.205.194/docs/bbb-dashboard/bar-thresholds-show.png)
>  
>  Et voila !
>  ![](http://93.90.205.194/docs/bbb-dashboard/bar-new.jpg)
>  ##
## Tips: Rendu de Tableau 
> Si vous souhaitez garder le tableau pour son aspect pratique mais souhaitez avoir une plus grande visibilité sur la valeur de la donnée (et pourquoi pas ajouter des seuil de couleur)
> ![](http://93.90.205.194/docs/bbb-dashboard/table-stat.jpg)
> ##
> Éditer votre tableau, et sélectionnez la visualisation **< Stat >**:
> ![](http://93.90.205.194/docs/bbb-dashboard/table-visu.png)
>##
> Cliquez sur **< All Values >** dans la section **< Value Options >**
> ![](http://93.90.205.194/docs/bbb-dashboard/table-values.jpg)
> ##
> Un peu plus bas dans la section **< Stat Styles >** cliquez sur **< Horizontal >** et sélectionnez dans **< Text Mode >** la case **< Value and name >**
> ![](http://93.90.205.194/docs/bbb-dashboard/table-horizontal-name.jpg)
> ##
> Pour modifier la couleur, c'est identique aux étapes précédentes, vous pouvez mettre des couleurs de seuil pour changer la couleur en fonction de la valeur que vous souhaitez.
> (Il se peut que ces étapes ne suffisent pas toujours, il faudra dans ce cas creuser un peu plus)
>##

