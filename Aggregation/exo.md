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