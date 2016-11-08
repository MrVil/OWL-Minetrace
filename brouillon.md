
### Identification des instances des classes :
```SPARQL
SELECT distinct ?instance WHERE
{
  ?instance rdf:type db:MinecraftObsel .
}
```

### Identification des instances des propriétés :
```SPARQL
SELECT distinct ?instance WHERE {
  ?s rdfs:property ?instance .
}
```


### Connaître le joueur le plus actif (celui qui a produit le plus d'obsels) en utilisant les relations RDF-S définies auparavant :
```SPARQL
SELECT ?name where
{
  ?s db:playerName ?name . ?s rdf:type db:MinecraftObsel .
}
```

### Raisonnement en OWL

## Altération du graphe :


## Création de la propriété inverse :
```SPARQL
INSERT DATA {
   db:doneBy owl:inverseOf db:did
}
```

## Vérification :
```SPARQL
select *
{
   ?s db:did ?o
}
```

Cela a fonctionné, les propriétés "did" ont été créées.
