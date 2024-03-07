# Liste opérateur 

```js
    $gt // superieur 
    $exists //verification d'une infomation (valeur true false 1 ou 0)
    $size // savoir la taille 
    $mod // modulo pour savoir si quelque chose est pair ou impair 
    $eq  // equal
    $not //pas equal n'ayant pas 
    $lte // inferieur 
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
    $nearSphere// donner les point les plus proche geospatiaux 
    $

```




```js
    .find({}) //Permet de faire des recherches 
    .insertOne({}), .insertMany({}) // permettre d'inserer des elements 
    db.createCollections()//creer une collection 
    db.colllections.drop()//Supprimer une collection 
    db.employees.aggregate([])// Faire plusieurs chose en meme temps filtre trier ...
    db.employees.updateMany({}) .updateOne({}) // permettre de modifier des informations dans une collection 

```

### GeoSpatiaux 

Création d'un index 
```js
db.avignon.createIndex({"localisation":"2dsphere"}) 
```
  