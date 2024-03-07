### Exercice 1

Vous disposez du code JavaScript suivant qui comporte une fonction de conversion d’une distance exprimée en kilomètres vers des radians ainsi que d’un document dont les coordonnées serviront de centre à notre sphère de recherche. Écrivez la requête $geoWithin qui affichera le nom des salles situées dans un rayon de 60 kilomètres et qui programment du Blues et de la Soul.

var KilometresEnRadians = function(kilometres){ 
   var rayonTerrestreEnKm = 6371; 
   return kilometres / rayonTerrestreEnKm; 
}; 
 
var salle = db.salles.findOne({"adresse.ville": "Nîmes"}); 
 
var requete = { ... }; 
 
db.salles.find(requete ... }; 

##### reponse 
```js
var KilometresEnRadians = function(kilometres){ 

   var rayonTerrestreEnKm = 6371; 

   return kilometres / rayonTerrestreEnKm; 

};
 
var salle = db.salles.findOne({"adresse.ville": "Nîmes"});
 
var rayonEnRadians = KilometresEnRadians(60); // Convertir 60 kilomètres en radians
 
var requete = {

  "adresse.localisation": {

    $geoWithin: {

      $centerSphere: [ [salle.adresse.localisation.coordinates[0], salle.adresse.localisation.coordinates[1]], rayonEnRadians ]

    }

  }

};
 
db.salles.find(requete, { _id: 0, nom: 1 });
```

```js 
//Correction 

// Fonction qui a été ecris dans l'ennocer
var KilometresEnRadians = function(kilometres){ 

   var rayonTerrestreEnKm = 6371; 

   return kilometres / rayonTerrestreEnKm; 

};

var salle = db.salles.findOne({"adresse.ville": "Nîmes"});

var req = {"adresse.localisation": {
  $geoWithin: { 
    $centerSphere: [
      salle.adresse.localisation.coordinates,
      KilometresEnRadians(60)
    ]
  }
}, "styles": {$in: {"blues", "soul"}}}

db.salles.find(req, { "_id":0, "nom":1})

//on obtiens 

nom: 'AJMI Jazz Club' 



```

### Exercice 2:

Écrivez la requête qui permet d’obtenir la ville des salles situées dans un rayon de 100 kilomètres autour de Marseille, triées de la plus proche à la plus lointaine :


 
db.salles.find(...) 
```js

// creer le point marseille c
var marseille = {"type": "Point", "coordinates": [43.300000, 5.400000]} 

//Creation d'un index afin de pouvoir faire la requete 
db.salles.createIndex({"adresse.localisation": "2dsphere"})

// Verification index 
db.salles.getIndexes()

// Faire la requete suivante 

db.salles.find({
  "adresse.localisation": {
    $nearSphere: {
      $geometry: marseille,
      $maxDistance: 100000 // Distance maximale en mètres
    }
  }
}, {
  "_id": 0,
  "ville": 1
})


```


### Exercice 3:

Soit polygone un objet GeoJSON de la forme suivante :

var polygone = { 
     "type": "Polygon", 
     "coordinates": [ 
            [ 
               [43.94899, 4.80908], 
               [43.95292, 4.80929], 
               [43.95174, 4.8056], 
               [43.94899, 4.80908] 
            ] 
     ] 
} 
Donnez le nom des salles qui résident à l’intérieur.

###### Pour trouver qu'elle salle es dans le polygones il faut copier  coller le var polygone et ecrire cette requete
```js
db.salles.find({
  "adresse.localisation": {
    $geoWithin: {
      $geometry: polygone
    }
  }
}, {
  "_id": 0,
  "nom": 1
})

```
Nom trouver par cette requete 
Ainsi nous trouvons le nom: 'AJMI Jazz Club'