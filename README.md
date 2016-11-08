# Dynamique des connaissances
*Raisonnement dans le web sémantique*

## Sommaire
* [Conception du graphe de connaissances](#1)
* [Utilisation du raisonnement](#2)
  * [RDF](#2-1)
  * [RDF-S](#2-2)
  * [OWL](#2-3)
* [Modification de la base de règles](#3)
  * [Liste des règles supprimées](#3-1)
  * [Liste des règles ajoutées](#3-2)
* [Annexe - Liste des requêtes](#4)
  * [Insertion de la structure du graphe](#4-1)
  * [Insertion des données](#4-2)
  * [Raisonnement](#4-3)
  * [Règles](#4-4)


## Conception du graphe de connaissances <a name="1"></a>

Voici la structure que nous allons donner à notre graphe de connaissance :

![schema](OWL_trace.png)


## Utilisation du raisonnement <a name="2"></a>

### RDF <a name="2-1"></a>

Requête : [Raisonnement-rdf](#4-3-1)

#### Objectif de la requête

#### Résultat

### RDF-S <a name="2-2"></a>

Requête : [Raisonnement-rdfs](#4-3-2)

#### Objectif de la requête

#### Résultat

### OWL <a name="2-3"></a>

Requête : [Raisonnement-owl](#4-3-3)

#### Objectif de la requête

#### Résultat

## Modification de la base de règles <a name="3"></a>

### Liste des règles supprimées <a name="3-1"></a>

### Liste des règles ajoutées <a name="3-2"></a>

## Annexe - Liste des requêtes <a name="4"></a>

### Insertion de la structure du graphe <a name="4-1"></a>

### Insertion des données <a name="4-2"></a>

### Raisonnement <a name="4-3"></a>

#### Raisonnement-rdf <a name="4-3-1"></a>

#### Raisonnement-rdfs <a name="4-3-2"></a>

#### Raisonnement-owl <a name="4-3-3"></a>

### Règles <a name="4-4"></a>

Au départ, nos obels sont à plat. Nous allons leur donner la structure suivante :

![schema](OWL_trace.png)


### Voici le code pour mettre en place un tel schéma :
On convient d'utiliser les prefixes suivants pour l'ensembles des requêtes :
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
```

#### PickupItem and DropItem are ItemObsel
```SPARQL
INSERT DATA {
  db:PickupItem rdfs:subClassOf db:ItemObsel .
  db:DropItem rdfs:subClassOf db:ItemObsel .
}
```

#### BlockPlace and BlockBreak are BlockObsel
```SPARQL
INSERT DATA {
  db:BlockPlace rdfs:subClassOf db:BlockObsel .
  db:BlockBreak rdfs:subClassOf db:BlockObsel .
}
```

#### ItemObsel and BlockObsel are ObjectObsel
```SPARQL
INSERT DATA {
  db:BlockObsel rdfs:subClassOf db:ObjectObsel .
  db:ItemObsel rdfs:subClassOf db:ObjectObsel .
}
```

#### PlayerJoin, PlayerKick and PlayerQuit are NetworkObsel
```SPARQL
INSERT DATA {
  db:PlayerJoin rdfs:subClassOf db:NetworkObsel .
  db:PlayerQuit rdfs:subClassOf db:NetworkObsel .
  db:PlayerKick rdfs:subClassOf db:NetworkObsel .
}
```

#### NetworkObsel, PlayerDamage and PlayerDeath are PlayerObsel
```SPARQL
INSERT DATA {
  db:NetworkObsel rdfs:subClassOf db:PlayerObsel .
  db:PlayerDamage rdfs:subClassOf db:PlayerObsel .
  db:PlayerDeath rdfs:subClassOf db:PlayerObsel .
}
```

#### PlayerObsel, ObjectObsel and Craft are MinecraftObsel
```SPARQL
INSERT DATA {
  db:PlayerObsel rdfs:subClassOf db:MinecraftObsel .
  db:ObjectObsel rdfs:subClassOf db:MinecraftObsel .
  db:Craft rdfs:subClassOf db:MinecraftObsel .
}
```

#### MinecraftObsel is ObselType
```SPARQL
INSERT DATA {
  db:MinecraftObsel rdfs:subClassOf db:ObselType .
}
```

#### Properties
```SPARQL
INSERT DATA {
  db:ObselType rdfs:property db:end .
  db:ObselType rdfs:property db:begin .
  db:ObselType rdfs:property <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1@id> .
  db:Craft rdfs:property db:resultType .
  db:Craft rdfs:property db:resultData .
  db:Craft rdfs:property db:numberOfCrafts .
  db:PlayerDamage rdfs:property db:cause .
  db:PlayerDamage rdfs:property db:damage .
  db:ObjectObsel rdfs:property db:data .
  db:BlockObsel rdfs:property db:blockName .
  db:ItemObsel rdfs:property db:itemName .
  db:ItemObsel rdfs:property db:amount .
  db:MinecraftObsel rdfs:property db:x .
  db:MinecraftObsel rdfs:property db:y .
  db:MinecraftObsel rdfs:property db:z .
  db:MinecraftObsel rdfs:property db:playerName .
}
```

#### Sub properties
```SPARQL
INSERT DATA {
  db:blockName rdfs:subPropertyOf db:objectName .
  db:itemName rdfs:subPropertyOf db:objectName .
}
```

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

## Utilisation du raisonnement

**Objectif de la requête**: Supprimer la connexion et la déconnexion d'un utilisateur

**Requête**
```SPARQL
DELETE DATA
{
  ?s rdf:type db:PlayerJoin .
  ?s rdf:type db:PlayerQuit
}
```

### Connaître le bloc le plus utilisé (cassé, posé ...) en utilisant les relations RDF-S définies auparavant :
```SPARQL
SELECT ?ressource where
{
  ?s db:blockName ?ressource . ?s rdf:type db:BlockObsel .
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
```SPARQL
INSERT
{
   ?obsel db:doneBy ?name
}
WHERE
{
   ?obsel db:playerName ?name
}

<<<<<<< HEAD
DELETE {
=======
DELETE
{ 
>>>>>>> 6761502b07c3625e43c5ac3069279515fc2dc6f1
   ?obsel db:playerName ?name
} WHERE {
   ?obsel db:playerName ?name
}

insert data
{
   db:Player rdfs:property db:did
}
```

## Création de la propriété inverse :
```SPARQL
{
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


##  Modification de la base de règles
### Lite des règles supprimées de la base
* Foo : Parce que

### Liste des règles ajoutées
