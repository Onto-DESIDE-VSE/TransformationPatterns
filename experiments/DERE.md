# Relationship dereification

Restoring a reified relationship as a binary property. May lead to reduction of an n-ary (n>2) relationship to a binary one.

Similar to Object Property Chain Shortcut Pattern [CHASH](https://github.com/Onto-DESIDE-VSE/TransformationPatterns/blob/main/experiments/CHASH.md).

### SPARQL for detection

```sparql
SELECT ?reif ?p ?a
WHERE{
    ?p rdfs:domain ?reif .
    ?p rdfs:range ?a .
}
```

### SPARQL update Tbox

```sparql
INSERT {
	<newProperty> a rdf:Property, owl:ObjectProperty ;
		rdfs:domain ?a ;
		rdfs:range ?b . }
WHERE { 
	?p rdfs:domain ?reif ;
     rdfs:range ?a .
	?q rdfs:domain ?reif ;
     rdfs:range ?b .
} 
```

## ABox Structure

Input
```
$x $p $y ; $r $z .
```
Output
```
$y $q $z .
```

### SPARQL update Abox

```sparql
INSERT {
	?y <newProperty> ?z . }
WHERE { 
?x ?p ?y ; ?r ?z .
} 
```


## Examples

### Artificial Example
Input:
```
:marriageAB v:involvesHusband :Bob; v:involvesWife :Alice; v:placeOfWedding :Prague .
```
Output:
```
:Bob vnew:isMarriedTo :Alice .
```

TODO (diagram)




## Transformation Pattern

