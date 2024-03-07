### Exercice 1 Modifiez la collection salle afin que soient dorénavant validés les documents destinés à y être insérés ; cette validation aura lieu en mode « strict » et portera sur les champs suivants :

nom sera obligatoire et devra être de type chaîne de caractères.

capacite sera obligatoire et devra être de type entier (int).

Dans le champ adresse, les champs codePostal et ville, tous deux de type chaîne de caractères, seront obligatoires.

Que constatez-vous lors de la tentative d’insertion suivante, et quelle en est la cause ?

db.salles.insertOne( 
{"nom": "Super salle", "capacite": 1500, "adresse": {"ville": "Musiqueville"}} 
) 
Que proposez-vous pour régulariser la situation ?

```json
var rules = {
	"nom": {
		"bsonType":"string",
        		"description": "Chaine de caractères - obligatoire"
		},
	"capacite":{
		"bsonType": "int",
		"description": "Entier - obligatoire"
		},
	"codePostal":{
		"bsonType":"string",
		"description": "Chaine de caractères - obligatoire"
		},
    "ville":{
		    "bsonType": "string",	
		    "description":"Chaine de caractères - obligatoire"
		}
}

db.runCommand(
   {
        "collMod": "salles",
       "validator": {
           $jsonSchema: {
                 "bsonType": "object",
                 "required": ["nom", "capacite", "codePostal","ville"],
                 "properties": rules
           }
       }
   }
)
```

Nous pouvons constater que le documents n'est pas en accord avec les regles de validations.

`Voici l'erreur :`
MongoServerError: Document failed validation

### Exercice 2

Rajoutez à vos critères de validation existants un critère supplémentaire : le champ _id devra dorénavant être de type entier (int) ou ObjectId.

```js
// Commande permettant d'ajouter _id
rules._id = {
    "anyOf": [
        { "bsonType": "int" },
        { "bsonType": "objectId" }
    ],
    "description": "Entier (int) ou ObjectId"
};

//Mettre a jour les rules
db.runCommand({
    "collMod": "salles",
    "validator": {
        "$jsonSchema": {
            "bsonType": "object",
            "required": ["_id", "nom", "capacite", "codePostal", "ville"],
            "properties": rules
        }
    }
});

```
Que se passe-t-il si vous tentez de mettre à jour l’ensemble des documents existants dans la collection à l’aide de la requête suivante :

db.salles.updateMany({}, {$set: {"verifie": true}}) 

voila l'erreur que la requete nous affiche 
`Document failed validation`

Supprimez les critères rajoutés à l’aide de la méthode delete en JavaScript


### Exercice 3

Rajoutez aux critères de validation existants le critère suivant :

Le champ smac doit être présent OU les styles musicaux doivent figurer parmi les suivants : "jazz", "soul", "funk" et "blues".

Que se passe-t-il lorsque nous exécutons la mise à jour suivante ?

db.salles.update({"_id": 3}, {$set: {"verifie": false}})


db.personnes.drop();   

  db.personnes.insertMany(   [ {"nom": "Durand", "prenom": "René", "interets": ["jardinage", "bricolage"], "age": 77},  {"nom": "Durand", "prenom": "Gisèle", "interets": ["bridge", "cuisine"], "age": 75},  {"nom": "Dupont", "prenom": "Gaston", "interets": ["jardinage",   "pétanque"], "age": 79},  {"nom": "Dupont", "prenom": "Catherine", "interets": ["cuisine"], "age": 66},  {"nom": "Duport", "prenom": "Eric", "interets": ["cuisine", "pétanque"], "age": 57},  {"nom": "Duport", "prenom": "Arlette","interets": ["jardinage"], "age": 80},  {"nom": "Lejeune", "prenom": "Jean","interets": ["jardinage"], "age": 75},  {"nom": "Lejeune", "prenom": "Mariette","interets": ["jardinage", "bridge"], "age": 66}  ]  )  

 