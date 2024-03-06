# Exercice Aggregation Pipeline 

### Ecrivez un pipeline aggregation qui permet d'obtenir le total payé parpersonnes et arrondissez. 

Ma commande 
```js 
db.achats.aggregate([
    {
        $match: {achats: {$exists: true }}
    },
    {$addFields: 
    {
    "totalPaye": {$round: [{ $sum: "$achats" },2]},
    "totalReduction": {$round: [{ $sum: "$reductions" },2]}
    
    }},
    {$addFields: {"total": {$round: [{$subtract: ["$totalPaye","$totalReduction"]},2]}}},
    
    {$project: {"_id":0, "name":1, "totalPaye":1 , "totalReduction":1, "total":1}}
])
```

### Exercice lookup unwind 

```js
db.ventes.aggregate([
  {$unwind: "$artiste"},
  {$lookup: {
		"from": "artistes",
		"localField": "artiste",
		"foreignField": "nom",
		"as": "detai_artiste"
}}])
```


## TD 

##### Exercice 1

Écrivez le pipeline qui affichera dans un champ nommé ville le nom de celles abritant une salle de plus de `50 personnes` ainsi qu’un `booléen` nommé `grande` qui sera positionné à la valeur « vrai » lorsque la salle dépasse une capacité de 1 000 personnes. Voici le squelette du code à utiliser dans le shell :
```js
var pipeline = [ 
    {$match: {"capacite": {$gt:50}}},
    {$addFields: { "ville" : "$adresse.ville", 
                "grande": {$cond: [ { $gt: ["$capacite", 1000]},true, false]} }},
    {$project: {"_id":0, "ville":1, "grande":1}}
] 

db.salles.aggregate(pipeline) 
```
 ##### Exercice 2

Écrivez le pipeline qui affichera dans un champ nommé apres_extension la capacité d’une salle augmentée de 100 places, dans un champ nommé avant_extension sa capacité originelle, ainsi que son nom.

```js
var pipeline = [
    {$addFields: {"avant_extension": "$capacite"}},
    {$addFields: {"apres_extension": {$add: ["$capacite",100]}}},
    {$project: {"_id":0,"nom":1, "avant_extension":1,"apres_extension":1}}
]

db.salles.aggregate(pipeline) 
```

##### Exercice 3

Écrivez le pipeline qui affichera, par `numéro de département`, la `capacité` totale des salles y résidant. Pour obtenir ce numéro, il vous faudra utiliser l’opérateur $substrBytes dont la syntaxe est la suivante :


{$substrBytes: [ < chaîne de caractères >, < indice de départ >, 
< longueur > ]} 

```js
var pipeline = [{
    $group :{
        "_id": {$substrBytes: ["$adresse.codePostal",0,2]},
				"capacite":{$sum: "$capacite"} 
    },
    
}]
db.salles.aggreagate(pipeline)
```
##### Exercice 4

Écrivez le pipeline qui affichera, pour chaque style musical, le nombre de salles le programmant. Ces styles seront classés par ordre alphabétique.

```js 
var pipeline = [
  {$unwind: "$styles"},
  {$group :{
        "_id":"$styles" , 
        "nombre_salle":{$sum: 1} 
    }},
   {$sort: {"_id": 1}}
]

```

##### Exercice 5

À l’aide des $buckets, comptez les salles en fonction de leur capacité :

celles de 100 à 500 places

celles de 500 à 5000 places

```js
var pipeline = [
    {
    $bucket: {
        groupBy: "$capacite",
        boundaries: [100, 500, 5000],
        default: "other",
        output: {
        count: { $sum: 1 }
        }}
    }
];
 
db.salles.aggregate(pipeline)
```

##### Exercice 6

Écrivez le pipeline qui affichera le nom des `salles` ainsi qu’un `tableau nommé avis_excellents` qui contiendra uniquement les avis dont la note est de 10.

```js
var pipeline = [
    {$unwind: "$avis"},
    {$match: {"avis.note": {$eq: 10}}},
    {$group :{
        "_id":"$avis" , 
        "avis_excellent": [{push }]
    }},
]

```