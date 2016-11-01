#Dynamique des connaissances
*Raisonnement dans le web sémantique*


## Conception du graphe de connaissances
Au départ, nos obels sont à plat. Nous allons leur donner la structure suivante :

![schema](OWL_minetrace.png)

Voici le code pour mettre en place un tel schéma :

```SPARQL
PREFIX db: <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1>
PREFIX w3: <http://www.w3.org/2000/01/rdf-schema>

INSERT DATA {
  db:PickupItem w3:subClassOf db:ItemObsel .
}

```
