# le pipeline d'aggregations @

## Operateur a voir avec exemple 

### $group

Permet de regourper les documents dune colections en fonctions des critere 

```js 
{$group: {_id: <critere specifie >, <field>: {<accumateur1> : <expression1>}, ... }}
```
``` js
//grouper par age 
var pipeline = [{
    $group :{
        "_id":"age" // id de group n'est pas le meme que le document 
    }
}]
```

``` js
//grouper par age avec le fields
var pipeline = [{
    $group :{
        "_id":"age" , 
        "fields":{$sum: 1} //nombre de personne qui a dans chaque groupe 
    }
}]
```

``` js
//grouper par age avec le fields
var pipeline = [{
    $group :{
        "_id":"age" , 
        "fields":{$sum: 1} 
    },
    $sort: {fields: -1}// trie 
}]
```

### $unwind ( au controle)

Permet de déconstruire un tableau dan s n document, creer autant de document que d'élément dans un tableau 
```js
{$unwind: <path> }
```

### $sortByCount 
N'est pas utiliser car le nom est count par defaut et ne peut pas etre changez 
```js
// reproduite le $group par age avec le fields
{$sortByCount: "$age"}
```

### les jointures avecc $lookup 

Malgrès que mongoDB soit du NoSQL nous pouvons faire des jointures avec l'operateur `$lookup`.

##### La syntaxe 
```js
{
   $lookup:
     {
       from: <collection ou on recupere les informations  >,
       localField: <field point de comparaison >,
       foreignField: <field champs que nous voulons avoir  >,
       as: <output tableau du resultat du jeu  >
     }
}
```

##### Exemple 
```js
db.ventes.aggregate([//on est dans vente 
  {$lookup: {
		"from": "artistes",//Collection a joindre  
		"localField": "artiste",// Notre point de comparraison que nous retrouvons dans l'autre table 
		"foreignField": "nom",// champs de la collection a joindre qui doit etre afficher 
		"as": "detai_artiste"// Affiche la resultat de la nouvelle collection jointe
}}
])
```



