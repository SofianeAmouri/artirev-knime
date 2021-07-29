# Artirev Knime Workflow

## Configuration
Knime version: 4.3.2

NodePit


## Vue d'ensemble
Voici une vue d'ensemble du Workflow

![artirev_workflow knwf](https://user-images.githubusercontent.com/61782191/123226112-a49ef180-d4d3-11eb-9aba-ca3363652f70.jpg)



## Chargement du fichier

![image](https://user-images.githubusercontent.com/61782191/123223257-07db5480-d4d1-11eb-96e0-cb002bd3a6ee.png)

Le chargement du fichier permet tout simplement de charger le fichier de type csv dans Knime, par la suite on garde uniquement deux colonnes qui nous intéresse, EID qui correspond à l'identifiant du document (articles ou autres) et References qui correspond à toutes les références citées dans le document. On sépare toutes les références des documents afin de les avoir séparement et on passe à la partie suivante.


## Pré-nettoyage (ancien, à refaire)
![image](https://user-images.githubusercontent.com/61782191/123225599-293d4000-d4d3-11eb-999f-8192980f6425.png)

On effectue un pré-nettoyage afin d'avoir des matchs de référence plus précise. On supprime plusieurs éléments de la référence, en commencant par supprimer les documents qui ne contiennent aucune référence, afin de gagner un peu de temps sur le traitement plus tard. 

Ensuite, le pré-nettoyage va effectuer les étapes suivantes:
- Normalisé les espaces et convertir la chaine de caractère en minuscule.
- Supprimer la partie "http/https" (donc l'URL).
- Supprimer "available", "accessed", "retrieved".
- Supprimer la partie [../..], par exemple [online] ou [eb/ol].
- Supprimer les numéros de pages (pas encore au point).
- Supprimer le DOI.

Cette partie doit être améliorer afin d'être utilisable pour tout type de références possible.

L'étape de pré-nettoyage peut encore être amélioré afin d'avoir de meilleur résultat au niveau des matching des références.

## Distance de Levenshtein normalisé



### Première partie (qui englobe toutes les données)
Lorsque l'on prend toutes les références afin de tester le matching entre elle, cela prend un temps considérable, même en ayant un petit data set qui contient uniquement 146 documents de type article.

![image](https://user-images.githubusercontent.com/61782191/123226322-d7e18080-d4d3-11eb-9d22-7453fdf028fe.png)



### Deuxième partie (partie de test avec une seule référence à comparer)

A des fins de tests, la deuxième partie utilise uniquement une référence qui correspond à "Row29_2" qui est "nakamoto s. bitcoin: a peer-to-peer electronic cash system. (2008-0) [2019-0]".
Ceci permet de tester la démarche sans perdre énormement aux chargements de toutes les références.

![image](https://user-images.githubusercontent.com/61782191/123226457-f8a9d600-d4d3-11eb-8710-1533e0994e09.png)

### Seuil pour la distance normalisé
Selon plusieurs tests effectués, le seuil pour la distance normalisé est à 0.53. Ce seuil pourrait être modifié par la suite si celui-ci ne convient plus.

### Comment choisir par quelle référence on remplace les autres ? (la référence la plus propre)
#### Méthode 1
![image](https://user-images.githubusercontent.com/61782191/123234059-ea12ed00-d4da-11eb-8982-0845a1e45f2f.png)

On sépare en différente zone pour chaque matching pour chaque référence, c'est-à-dire on observe les différents matching et on calcul le "score total" afin d'avoir une sorte de moyenne pour chaque référence.

Zone 1:
- 0.212
- 0.257
- 0.297
- Score total: 0.766

Zone 2:
- 0.054
- 0.057
- 0.257
- Score total: 0.368

Zone 3:
- 0.054
- 0.108
- 0.297
- Score total: 0.459

Zone 4:
- 0.057
- 0.108
- 0.212
- Score total: 0.377

Donc, cela nous montre que la référence qui a un petit score total (en additionnant tous les scores) correspond le plus à la réalité et que c’est cette référence qui devrait être prise comme référence de remplacement pour toutes les autres. Malgré un taux d’erreurs de faute d’orthographe, cela revient quand même à uniformiser vers une référence contenant le moins possible d’erreurs voire même aucune.

#### Méthode 2
(à appronfondir) Remplacer toutes les références par la référence qui a eu le plus de match de similarité, donc cette référence serait celle qui couvre le plus de référence similaire. 

## Fingerprint Similarity
![image](https://user-images.githubusercontent.com/61782191/123226503-05c6c500-d4d4-11eb-9df0-93f7def27d56.png)


### Problèmes
Problème avec le noeud "Create Bit Vector":

![image](https://user-images.githubusercontent.com/61782191/123230386-80451400-d4d7-11eb-80dd-11cd1ccfc189.png)

Les caractères qui sont "invalides" se retrouvent être à chaque fois le dernier caractère de chaque référence.
