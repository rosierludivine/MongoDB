## TP exobook

### Création de la base de données ainsi que sa collection  use sample_db
```js
db.employees.insertMany([
  {"name":"John Doe","age":35,"job":"Manager","salary":80000},
  {"name":"Jane Doe","age":32,"job":"Developer","salary":75000},
  {"name":"Jim Smith","age":40,"job":"Manager","salary":85000}],)  Écrivez une requête MongoDB pour trouver tous les documents dans la collection "employees".  db.employees.find()

```
### Écrivez une requête pour trouver tous les documents où l'âge est supérieur à 33. db.employees.find({"age": {$gt: 33}})  Écrivez une requête pour trier les documents dans la collection "employees" par salaire décroissant.
```js
db.employees.find({},{"salary":1}).sort({"salary":-1})
```

### Écrivez une requête pour sélectionner uniquement le nom et le job de chaque document
```js
db.employees.find({},{"_id":0, "name":1,"job":1})
```
### Écrivez une requête pour compter le nombre d'employés par poste.
```js
db.employees.aggregate( [ { $group : {_id : "$job", count: {$sum: 1} } } ] )
```

### Écrivez une requête pour mettre à jour le salaire de tous les développeurs à 80000.
```js 
db.employees.updateMany(
  {"job":"Developer"},
  {$set: {"salary":80000}}) 
 
 ```
 ## TP Exo 

### Exercice 1:Affichez l’identifiant et le nom des salles qui sont des SMAC.
```js
db.salles.find({},{"_id":1, "nom":1 })
```
### Exercice 2 : Affichez le nom des salles qui possèdent une capacité d’accueil strictement supérieure à 1000 places.
```js
db.salles.find({"capacite": {$gt : 1000 }},{ "nom":1 })
```
### Exercice 3 : Affichez l’identifiant des salles pour lesquelles le champ adresse ne comporte pas de numéro.
```js
db.salles.find({"adresse.numero": {$exists:1}}, {"_id":1})
```
### Exercice 4: Affichez l’identifiant puis le nom des salles qui ont exactement un avis.
```js
db.salles.find({"avis": {$size:1}},{"_id":0, "nom":1})
```
### Exercice 5: Affichez tous les styles musicaux des salles qui programment notamment du blues.
```js
db.salles.find({"styles": "blues"},{"_id":0, "nom":1})
```

### Exercice 6 : Affichez tous les styles musicaux des salles qui ont le style « blues » en première position dans leur tableau styles.
db.salles.find({"styles.0": "blues"},{"_id":0, "nom":1})


### Exercice 7 : Affichez la ville des salles dont le code postal commence par 84 et qui ont une capacité strictement inférieure à 500 places (pensez à utiliser une expression régulière).
```js
db.salles.find({ "adresse.codePostal": /^84/, "capacite" : {$lt: 500}}, {_id: 0, "adresse.ville": 1 })  A REVOIR 
```

### Exercice 8 : Affichez l’identifiant pour les salles dont l’identifiant est pair ou le champ avis est absent.
```js
db.salles.find({$or: [ { "_id": { $mod: [2,0]}},{"avis": { $exists:false}}],{"_id":1}}) //A REVOIR 
```
### Exercice 9 : Affichez le nom des salles dont au moins un des avis comporte une note comprise entre 8 et 10 (tous deux inclus).
```js
db.salles.find({"avis.note": {$lte:10, $gt:8}}, {"_id":0, nom:1})
```

### Exercice 10: Affichez le nom des salles dont au moins un des avis comporte une date postérieure au 15/11/2019 (pensez à utiliser le type JavaScript Date).
```js
db.salles.find({"avis.date": {$gt: ISODate("2019-11-15T00:00:00.000+00:00")}},{"_id":0, "nom":1})
```

### Exercice 11: Affichez le nom ainsi que la capacité des salles dont le produit de la valeur de l’identifiant par 100 est strictement supérieur à la capacité.
```js
db.salles.find({$expr: {$gt: [{$multiply: ["$_id", 100]}, "$capacite"]}},{"_id":0,"nom":1,"capacite":1})
```

### Exercice 12: Affichez le nom des salles de type SMAC programmant plus de deux styles de musiques différents en utilisant l’opérateur $where qui permet de faire usage de JavaScript.
Passer nous utilisons pas $where a par en local autrement  atlas nous bloque 

### Exercice 13:Affichez les différents codes postaux présents dans les documents de la collection salles.
```js
db.salles.find({},{"_id":0, "adresse.codePostal":1})
```

### Exercice 14: Mettez à jour tous les documents de la collection salles en rajoutant 100 personnes à leur capacité actuelle.
```js
db.salles.updateMany({},{$inc: {"capacite":100}},{})
```
### Exercice 15:: Ajoutez le style « jazz » à toutes les salles qui n’en programment pas.
```js
db.salles.upadteMany({"styles": {$ne:jazz" }}, {$push: {"styles:"jazz"}})
```
### Exercice 16: Retirez le style «funk» à toutes les salles dont l’identifiant n’est égal ni à 2, ni à 3.
```js
db.salles.updateMany( { _id: { $nin: [2, 3] } }, { $pull: { styles: "funk" } })
```
### Exercice 17: Ajoutez un tableau composé des styles «techno» et « reggae » à la salle dont l’identifiant est 3.
```js
db.salles.updateOne(
{"_id":3}, 
{$addToSet:
	{"styles": 
		{$each: ["techno", "reggae"]}}
})
// suivie d'un 
db.salles.find()
//Afin de voir si les modification on était effectue 
```



### Exercice 18: Pour les salles dont le nom commence par la lettre P (majuscule ou minuscule), augmentez la capacité de 150 places et rajoutez un champ de type tableau nommé contact dans lequel se trouvera un document comportant un champ nommé telephone dont la valeur sera « 04 11 94 00 10 ».
```js
$regex permet de faire une recherche avec une expression reguliere 
db.salles.updateMany( { nom: { $regex: /^P/i } }, { $inc: { capacite: 150 }, $set: { contact: { telephone: "04 11 94 00 10" } } })
```
### Exercice 19: Pour les salles dont le nom commence par une voyelle (peu importe la casse, là aussi), rajoutez dans le tableau avis un document composé du champ date valant la date courante et du champ note valant 10 (double ou entier). L’expression régulière pour chercher une chaîne de caractères débutant par une voyelle suivie de n’importe quoi d’autre est [^aeiou]+$.
```js
db.salles.updateMany( { nom: { $regex: /^[aeiou]/i } }, { $push: { avis: { date: new Date(), note: 10 } } })
```

### Exercice 20: En mode upsert, vous mettrez à jour tous les documents dont le nom commence par un z ou un Z en leur affectant comme nom « Pub Z », comme valeur du champ capacite 50 personnes (type entier et non décimal) et en positionnant le champ booléen smac à la valeur « false ».
```js
db.salles.updateMany( { nom: { $regex: /^[zZ]/ } }, { $set: { nom: "Pub Z", capacite: 50, smac: false } }, { upsert: true })
```
### Exercice 21: Affichez le décompte des documents pour lesquels le champ _id est de type « objectId ».
```js
db.salles.find({ _id: { $type: "objectId" } }).count()
```
### Exercice 22: Pour les documents dont le champ _id n’est pas de type « objectId », affichez le nom de la salle ayant la plus grande capacité. Pour y parvenir, vous effectuerez un tri dans l’ordre qui convient tout en limitant le nombre de documents affichés pour ne retourner que celui qui comporte la capacité maximale.
```js
db.salles.find({ _id: { $not: { $type: "objectId" } } }) .sort({ capacite: -1 }) .limit(1)
```
### Exercice 23: Remplacez, sur la base de la valeur de son champ _id, le document créé à l’exercice 20 par un document contenant seulement le nom préexistant et la capacité, que vous monterez à 60 personnes.
```js
db.salles.replaceOne( { nom: "Pub Z" }, { nom: "Pub Z", capacite: 60 })
```
### Exercice 24: Effectuez la suppression d’un seul document avec les critères suivants : le champ _id est de type « objectId » et la capacité de la salle est inférieure ou égale à 60 personnes.
```js
db.salles.deleteOne({ _id: { $type: "objectId" }, capacite: { $lte: 60 }})
```
### Exercice 25: À l’aide de la méthode permettant de trouver un seul document et de le mettre à jour en même temps, réduisez de 15 personnes la capacité de la salle située à Nîmes.
t