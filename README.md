<link rel="stylesheet" href="path/to/font-awesome/css/font-awesome.min.css">

# Dynamique des connaissances
*Raisonnement dans le web sémantique*

TP réalisé avec le moteur d'inférence [Hylar](https://www.npmjs.com/package/hylar/)

## Sommaire <a name="0"></a>
* [Conception du graphe de connaissances](#1)
* [Utilisation du raisonnement](#2)
  * [RDF](#2-1)
  * [RDF-S](#2-2)
  * [OWL](#2-3)
* [Modification de la base de règles](#3)
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

Supprimer la connexion et la déconnexion d'un utilisateur

#### Résultat

Toutes les instances de PlayerJoin et de PlayerQuit sont supprimées.

### RDF-S <a name="2-2"></a>

Requête : [Raisonnement-rdfs](#4-3-2)

#### Objectif de la requête

Connaître le bloc le plus utilisé (cassé, posé ...) en utilisant les relations RDF-S définies auparavant

#### Résultat

On constate que le bloc le plus utilisé est le bois ("LOG")

### OWL <a name="2-3"></a>

Requête : [Raisonnement-owl](#4-3-3)

#### Objectif de la requête

Inférer le fait que si un MinecraftObsel *est fait* par un joueur, alors le joueur *fait* ce MinecraftObsel

#### Résultat

## Modification de la base de règles <a name="3"></a>

Nous allons modifier la base de règle afin de préciser qu'un *Item* ne peut être que *Drop* ou *Pickup*.

Pour cela, nous allons définir que *ItemPickup* et *ItemDrop* sont deux classes distinctes. Ensuite, nous allons définir que *ItemPickup* est le complet de *ItemObsel*

## Annexe - Liste des requêtes <a name="4"></a> 

On convient d'utiliser les prefixes suivants pour l'ensembles des requêtes :
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
```

### Insertion de la structure du graphe <a name="4-1"></a>

**PickupItem** et **DropItem** sont des **ItemObsel**
```SPARQL
INSERT DATA {
  db:PickupItem rdfs:subClassOf db:ItemObsel .
  db:DropItem rdfs:subClassOf db:ItemObsel .
}
```

**BlockPlace** et **BlockBreak** sont des **BlockObsel**
```SPARQL
INSERT DATA {
  db:BlockPlace rdfs:subClassOf db:BlockObsel .
  db:BlockBreak rdfs:subClassOf db:BlockObsel .
}
```

**ItemObsel** et **BlockObsel** sont des **ObjectObsel**
```SPARQL
INSERT DATA {
  db:BlockObsel rdfs:subClassOf db:ObjectObsel .
  db:ItemObsel rdfs:subClassOf db:ObjectObsel .
}
```

**PlayerJoin**, **PlayerKick** et **PlayerQuit** sont des **NetworkObsel**
```SPARQL
INSERT DATA {
  db:PlayerJoin rdfs:subClassOf db:NetworkObsel .
  db:PlayerQuit rdfs:subClassOf db:NetworkObsel .
  db:PlayerKick rdfs:subClassOf db:NetworkObsel .
}
```

**NetworkObsel**, **PlayerDamage** et **PlayerDeath** sont des **PlayerObsel**
```SPARQL
INSERT DATA {
  db:NetworkObsel rdfs:subClassOf db:PlayerObsel .
  db:PlayerDamage rdfs:subClassOf db:PlayerObsel .
  db:PlayerDeath rdfs:subClassOf db:PlayerObsel .
}
```

**PlayerObsel**, **ObjectObsel** et **Craft** sont des **MinecraftObsel**
```SPARQL
INSERT DATA {
  db:PlayerObsel rdfs:subClassOf db:MinecraftObsel .
  db:ObjectObsel rdfs:subClassOf db:MinecraftObsel .
  db:Craft rdfs:subClassOf db:MinecraftObsel .
}
```

**MinecraftObsel** est un **ObselType**
```SPARQL
INSERT DATA {
  db:MinecraftObsel rdfs:subClassOf db:ObselType .
}
```

**Properties**
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

**Sub properties**
```SPARQL
INSERT DATA {
  db:blockName rdfs:subPropertyOf db:objectName .
  db:itemName rdfs:subPropertyOf db:objectName .
}
```

### Insertion des données <a name="4-2"></a>

Pour insérer les données, on importe les fichiers suivants :
* [obsels0_50](obsels0_50.ttl)
* [obsels50_100](obsels50_100.ttl)

### Raisonnement <a name="4-3"></a>

#### Raisonnement-rdf <a name="4-3-1"></a> [(up)](#2-1)

```SPARQL
DELETE
{
  ?s rdf:type db:PlayerQuit
}
WHERE
{
  ?s rdf:type db:PlayerQuit
}

DELETE
{
  ?s rdf:type db:PlayerJoin
}
WHERE
{
  ?s rdf:type db:PlayerJoin
}
```

#### Raisonnement-rdfs <a name="4-3-2"></a> [(up)](#2-2)

```SPARQL
SELECT ?ressource WHERE
{
  ?s db:blockName ?ressource . ?s rdf:type db:BlockObsel .
}
```

Cette requête permet de voir que les blocks de type LOG sont les plus utilisés (casser ou poser).
Elle aurait été plus efficace accompagnée d'un Count().

#### Raisonnement-owl <a name="4-3-3"></a> [(up)](#2-3)

Pour le raisonnement en OWL, nous avons besoin de modifier quelque peu le graphe

```SPARQL
INSERT DATA
{
   "Drazatlam" rdf:type db:Player .
   "gus3000" rdf:type db:Player .
   "Hakkahi" rdf:type db:Player .
   "Estayr" rdf:type db:Player .
}
```

On triche en insérant des *string* plutôt que des *URIs* pour faciliter la tâche, le fonctionnement aurait été semblable autrement.

```SPARQL
INSERT DATA
{
   db:MinecraftObsel rdfs:property db:doneBy
}

INSERT DATA
{
   ?obsel db:doneBy ?name
}
WHERE
{
   ?obsel db:playerName ?name
}

DELETE
{
   ?obsel db:playerName ?name
} 
WHERE
{
   ?obsel db:playerName ?name
}

INSERT DATA
{
   db:Player rdfs:property db:did
}
```

Voici finalement la requête

```SPARQL
INSERT DATA {
   db:doneBy owl:inverseOf db:did
}
```

### Règles <a name="4-4"></a>

```SPARQL
INSERT DATA {
 db:ItemObsel rdf:type owl:Class .
 db:ItemDrop rdf:type owl:Class .
 db:ItemPickup rdf:type owl:Class .
 
 db:ItemDrop owl:disjointWith db:ItemPickup .
 
 db:PickupItem owl:complementOf db:DropItem . 
}
```
