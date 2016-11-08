# Dynamique des connaissances
*Raisonnement dans le web sémantique*


## Conception du graphe de connaissances
Au départ, nos obels sont à plat. Nous allons leur donner la structure suivante :

![schema](OWL.png)


### Voici le code pour mettre en place un tel schéma :

PickupItem and DropItem are ItemObsel
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:PickupItem w3:subClassOf db:ItemObsel .
}

INSERT DATA {
  db:DropItem w3:subClassOf db:ItemObsel . }
```
BlockPlace and BlockBreak are BlockObsel
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:BlockPlace w3:subClassOf db:BlockObsel . 
}

INSERT DATA {
  db:BlockBreak w3:subClassOf db:BlockObsel . 
}

```

ItemObsel and BlockObsel are ObjectObsel
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:BlockObsel w3:subClassOf db:ObjectObsel . 
}


INSERT DATA {
  db:ItemObsel w3:subClassOf db:ObjectObsel . 
}

```

PlayerJoin, PlayerKick and PlayerQuit are NetworkObsel
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:PlayerJoin w3:subClassOf db:NetworkObsel . 
}


INSERT DATA {
  db:PlayerQuit w3:subClassOf db:NetworkObsel . 
}


INSERT DATA {
  db:PlayerKick w3:subClassOf db:NetworkObsel . 
}
```

NetworkObsel, PlayerDamage and PlayerDeath are PlayerObsel
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:NetworkObsel w3:subClassOf db:PlayerObsel . 
}


INSERT DATA {
  db:PlayerDamage w3:subClassOf db:PlayerObsel . 
}


INSERT DATA {
  db:PlayerDeath w3:subClassOf db:PlayerObsel . 
}
```

PlayerObsel, ObjectObsel and Craft are MinecraftObsel
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:PlayerObsel w3:subClassOf db:MinecraftObsel . 
}


INSERT DATA {
  db:ObjectObsel w3:subClassOf db:MinecraftObsel . 
}


INSERT DATA {
  db:Craft w3:subClassOf db:MinecraftObsel . 
}
```

MinecraftObsel is ObselType
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:MinecraftObsel w3:subClassOf db:ObselType . 
}
```

Properties
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>

INSERT DATA {
  db:ObselType w3:property db:end . 
}


INSERT DATA {
  db:ObselType w3:property db:begin . 
}


INSERT DATA {
  db:ObselType w3:property db:@id . 
}


INSERT DATA {
  db:ObselType w3:property db:playerName . 
}


INSERT DATA {
  db:ObjectObsel w3:property db:data . 
}


INSERT DATA {
  db:BlockObsel w3:property db:blockName . 
}


INSERT DATA {
  db:ItemObsel w3:property db:itemName . 
}


INSERT DATA {
  db:MinecraftObsel w3:property db:x . 
}


INSERT DATA {
  db:MinecraftObsel w3:property db:y . 
}


INSERT DATA {
  db:MinecraftObsel w3:property db:z . 
}


INSERT DATA {
  db:MinecraftObsel w3:property db:playerName . 
}

```

## Utilisation du raisonnement

**Objectif de la requête**: Supprimer la connexion et la déconnexion d'un utilisateur

**Requête**
```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3b : <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

DELETE DATA
{
  ?s w3b db:NetworkObsel
}
```

**Résultat**
`TODO`

### Connaître le bloc le plus utilisé (cassé, posé ...) en utilisant les relations RDF-S définies auparavant :
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?ressource where { ?s db:blockName ?ressource . ?s <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> db:BlockObsel . }

### Connaître le joueur le plus actif en utilisant les relations RDF-S définies auparavant :
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?name where { ?s db:playerName ?name . ?s <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> db:MinecraftObsel . }

### Raisonnement en OWL
`TODO`


##  Modification de la base de règles
### Lite des règles supprimées de la base
* Foo : Parce que

### Liste des règles ajoutées
