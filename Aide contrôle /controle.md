1. Vous devez interroger une collection de personnes pour trouver les trois individus les plus jeunes qui ont un emploi dans le domaine de l'ingénierie, triés par le plus jeune en premier.


var pipeline = [
     {$match: { "vocation": "ENGINEER" } }, 
     {$addFields: {age: { $subtract: [new Date(), "$dateofbirth"] } }}
  { $sort: { age: 1 } },
  { $limit: 3 } 
]

2. Vous devez générer un rapport pour montrer ce que chaque client de la boutique a acheté en 2020. Vous regrouperez les enregistrements de commandes individuelles par client, en notant la date du premier achat de chaque client, le nombre de commandes qu'ils ont passées, la valeur totale de toutes leurs commandes, et une liste de leurs articles de commande triés par date.



3. Vous souhaitez générer un rapport de vente au détail pour lister la valeur totale et la quantité de produits chers vendus (d'une valeur supérieure à 15 dollars). Les données sources sont une liste de commandes de magasin, où chaque commande contient l'ensemble des produits achetés dans le cadre de la  commande . avec l'operateur $unwind

var pipeline = [
    {$unwind: "$products"},
    {$match: {  "products.price": {$gt: 15}  }},
    {$group: {"_id": "null",
            "valeur_totale":{$sum: {$toDouble: "$products.price"}},
            "quantite": {$sum: 1 }
    }},
    {$project: {"_id":0, "valeur_totale":1, "quantite":1}}
]
db.produits.aggregate(pipeline)

4. Vous souhaitez interroger une collection de personnes où chaque document contient des données sur une ou plusieurs langues parlées par la personne. Le résultat de la requête doit être une liste alphabétiquement triée de langues uniques qu'un développeur peut ensuite utiliser pour peupler une liste de valeurs dans un widget de liste déroulante d'une interface utilisateur.
Cet exemple est l'équivalent d'une instruction SELECT DISTINCT en SQL.

Cette requete permet de voir tout les langues sans avoir de doublons . 
var pipeline = [
  { $unwind: "$language" }, // Décompose les éléments du tableau
  { $group: { _id: "$language" } }, // Grouper par langue
  { $sort: { "_id": 1 } }, // Trier par ordre alphabétique
  { $project: { "_id": 0, "languages": "$_id" } } // Renommer _id en languages
]

db.personne.aggregate(pipeline)

5. Vous souhaitez générer un rapport pour lister tous les achats en magasin pour 2020, en montrant le nom et la catégorie du produit pour chaque commande, plutôt que l'ID du produit. Pour ce faire, vous devez prendre la collection de commandes des clients et joindre chaque enregistrement de commande au produit correspondant dans la collection de produits. Il y a une relation plusieurs-à-un entre les deux collections, résultant en une jointure un-à-un lors de la correspondance d'une commande à un produit. La jointure utilisera une comparaison de champ unique entre les deux côtés, basée sur l'ID du produit.
Commençons par choisir le jeu de données et le préparer pour le pipeline d'agrégation.

Operateur: $lookup

db.ventes.aggregate([
  {$unwind: "$artiste"},
  {$lookup: {
		"from": "artistes",
		"localField": "artiste",
		"foreignField": "nom",
		"as": "detai_artiste"
}}])


var pipeline = [
    //Filtre qui permette de d'avoir que les dates de 2020
  { $match: { 
      date: { 
        $gte: ISODate("2020-01-01"), 
        $lt: ISODate("2021-01-01") 
      } 
    } 
  },
  { 
    $lookup: { 
      from: "produits",
      localField: "produitId", 
      foreignField: "id", 
      as: "produit" 
    } 
  },
  { 
    $unwind: "$produit" 
  },
  { 
    $project: { 
      _id: 0, 
      produitName: "$produit.name",
      produitCategory: "$produit.category" 
    } 
  }
]

6. Vous voulez générer un rapport pour lister toutes les commandes effectuées pour chaque produit en 2020. Pour ce faire, vous devez prendre une collection de produits d'une boutique et joindre chaque enregistrement de produit à toutes ses commandes stockées dans une collection de commandes. Il y a une relation un-à-plusieurs entre les deux collections, basée sur la correspondance de deux champs de chaque côté. Plutôt que de joindre sur un seul champ tel que product_id (qui n'existe pas dans ce jeu de données), vous devez utiliser deux champs communs pour joindre (product_name et product_variation).

var pipeline = [
    {
          $lookup: {
            from: "orders",
            let: {
              Name: "$name",
              Variation: "$variation"
            },
            pipeline: [
              {
                $match: {
                  $expr: {
                    $and: [
                      { $eq: ["$product_name", "$$Name"] },
                      { $eq: ["$product_variation", "$$Variation"] },
                      { $gte: ["$orderdate", ISODate("2020-01-01")] },
                      { $lt: ["$orderdate", ISODate("2021-01-01")] }
                    ]
                  }
                }
              }
            ],
            as: "orders"
          }
        },
        {
          $unwind: "$orders"
        },
        {
          $project: {
            _id: 0,
            Name: "$name",
            Variation: "$variation",
            category: 1,
            description: 1,
            orderdate: "$orders.orderdate",
            customer_id: "$orders.customer_id",
            value: "$orders.value"
          }
        }
    ]
    db.products.aggregate(pipeline)

  7. Analysez le dataset suivant : https://www.kaggle.com/datasets/joebeachcapital/tornados/data
Analyser le format des données et identifier les champs contenant des informations géographiques pertinentes (comme la localisation des tornades).Créer un index géospatial sur le champ contenant les informations de localisation des tornades.

Ce dataset a pour bute de retracer la trajectoire des tornade qui ont eu lieu au États Unis plus exactement a Porto Rico et dans les îles Vierges américaines. Depuis les années de 1950 à 2013.

8. Écrire des requêtes (structure) pour répondre à plusieurs scénarios,  :
- Identifier toutes les tornades survenues dans un rayon spécifique autour d'un point donné.
- Calculer le nombre de tornades par état ou région.
- Trouver les tornades les plus proches d'une ville spécifique.


