# Artirev Knime Workflow

## Configuration
Knime version: 4.3.2


## Vue d'ensemble
Voici une vue d'ensemble du Workflow

![artirev_workflow knwf](https://user-images.githubusercontent.com/61782191/123226112-a49ef180-d4d3-11eb-9aba-ca3363652f70.jpg)



## Chargement du fichier
![image](https://user-images.githubusercontent.com/61782191/123223257-07db5480-d4d1-11eb-96e0-cb002bd3a6ee.png)



## Pré-nettoyage
![image](https://user-images.githubusercontent.com/61782191/123225599-293d4000-d4d3-11eb-999f-8192980f6425.png)



## Distance de Levenshtein normalisé
### Première partie (qui englobe toutes les données)
![image](https://user-images.githubusercontent.com/61782191/123226322-d7e18080-d4d3-11eb-9d22-7453fdf028fe.png)



### Deuxième partie (partie de test avec une seule référence à comparer)
![image](https://user-images.githubusercontent.com/61782191/123226457-f8a9d600-d4d3-11eb-8710-1533e0994e09.png)

### Seuil pour la distance normalisé
Selon plusieurs tests effectués, le seuil pour la distance normalisé est à 0.53. Ce seuil pourrait être modifié par la suite si celui-ci ne convient plus.

### Comment choisir par quelle référence on remplace les autres ? (la référence la plus propre)
![image](https://user-images.githubusercontent.com/61782191/123234059-ea12ed00-d4da-11eb-8982-0845a1e45f2f.png)
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

## Fingerprint
![image](https://user-images.githubusercontent.com/61782191/123226503-05c6c500-d4d4-11eb-9df0-93f7def27d56.png)


### Problèmes
Problème avec le noeud "Create Bit Vector":
![image](https://user-images.githubusercontent.com/61782191/123230386-80451400-d4d7-11eb-80dd-11cd1ccfc189.png)
Les caractères qui sont "invalides" se retrouvent être à chaque fois le dernier caractère de chaque référence.
