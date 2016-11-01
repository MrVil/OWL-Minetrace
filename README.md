#Dynamique des connaissances
*Raisonnement dans le web sémantique*


## Conception du graphe de connaissances
Au départ, nos obels sont à plat. Nous allons leur donner la structure suivante :

![schema](OWL_minetrace.png)

```SPARQL
INSERT DATA {
  <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#PickupItem>
  <http://www.w3.org/2000/01/rdf-schema#subClassOf>
  <https://liris-ktbs01.insa-lyon.fr:8000/public/master-ia-2016/zguyl/model1#ItemObsel> .
}

```
