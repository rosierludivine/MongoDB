## 
un pipeline est souvent un tableau car on peut lister des informations 
 
 Collection contenaire qui contient des documents peut avoir comme similarité table 

## Liste opérateur 

```js
    $addField // creer un nouveau champs 
    $unwind //decomposer les elements du tableau (affiche autant d'objet que de valeur dans la tableau)
    $project //permet d'afficher ce que nous allons voir en resultat 
    $sortByCount // trier et compter 
    $buckets // regroupe les documents 
    $cond // mettre une condition 
    $gt // superieur mais ne prend pas la valeur 
    $gte // 50 et plus egal ou superieur 
    $exists //verification d'une infomation (valeur true false 1 ou 0)
    $size // savoir la taille 
    $mod // modulo pour savoir si quelque chose est pair ou impair 
    $eq  // equal
    $not //pas equal n'ayant pas 
    $lt // inferieur 
    $lte // inferieur ou egal 
    $nin //n'ayant ni un ni l'autre 
    $expr // ecrire une expression 
    $regex //filtre regex 
    $multiply // faire des multiplication 
    $sum // addition
    $subtract // soustraction 
    $push // ajout valeur tableau
    $type // retourne la valeur type 
    $where //transmettre un chaine avec  expression js
    $sort // permet de faire un trie 
    $near //pas de différence avec `$nearSphere`
    $nearSphere// donner les point les plus proche geospatiaux 
    $geoWithin // permet de faire une zone est de voir qu'elle point est dans la salle 
    $each // transformer en tableau 
    $in // dans exemple dans styles il y a blues et soul 
    //exemple  
    "styles": {$in: {"blues", "soul"}}



    //Operateur logique 
    $or // que la valeur est egal
    $nor // A ne pas utiliser 
    $all //affiche toute les informations comportement les specifications 

```




```js
    .find({}) //Permet de faire des recherches 
    .insertOne({}), .insertMany({}) // permettre d'inserer des elements 
    db.createCollections()//creer une collection 
    db.colllections.drop()//Supprimer une collection 
    db.employees.aggregate([])// Faire plusieurs chose en meme temps filtre trier ...
    db.employees.updateMany({}) .updateOne({}) // permettre de modifier des informations dans une collection 

```
### Tableau 

exemple :

```js
// ajoute a id 3 crochet et cuisine a leur passion 
db.hobbies.updateMany({"_id":3}, {$push: {"passions":{$each: ["crochet","cuisine"]}}})
// Ajoute un tableau dans passions avec voiture et lecture 
db.hobbies.updateMany({"_id":3}, {$addToSet: {"passions":{$each: ["Voiture","Lecture"]}}})
// supprime Voiture et Lecture a passion 
db.hobbies.updateOne({"_id":3}, {$pull: {"passions": {"Voiture", "Lecture "}}})

```

### GeoSpatiaux 

Un point longitude et latitude 
Un segement 2 point 

Création d'un index 
```js
db.avignon.createIndex({"localisation":"2dsphere"}) 

```

Pour le ` $nearSphere ` le min et le max sont optionnel. 

 ### Validation 

##### Schema de validation 
```js
var rules = {
	“nom“: {
		“bsonType“:“string“,
        “description“: “Chaine de caractères - obligatoire“
		},
	“capacite“:{
		“bsonType“: “int“,
		“description“: “Le champs doit être un int“
		},
	“codePostal“:{
		“bsonType“:“string“,
		“description“: “Le champs doit être une chaine de commande“
		},
    “adresse.ville“:{
		“bsonType“: “string“,
        “description“:“Le champs doit être une chaine de commande“
		}
}

db.runCommand(
   {
        "collMod": "salles",
       "validator": {
           $jsonSchema: {
                 "bsonType": "object",
                 "required": ["nom", "capacite", "codePostal","adresse.ville"],
                 "properties": rules
           }
       }
   }
)
```