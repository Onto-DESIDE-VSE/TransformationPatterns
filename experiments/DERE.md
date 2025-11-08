# Relationship dereification (DERE)

Restoring a reified relationship as a binary property. May lead to reduction of an n-ary (n>2) relationship to a binary one.

Similar to Object Property Chain Shortcut Pattern [CHASH](https://github.com/Onto-DESIDE-VSE/TransformationPatterns/blob/main/experiments/CHASH.md).

### SPARQL for detection

```sparql
SELECT ?a ?p ?reif ?q ?b
WHERE{
    ?p rdfs:domain ?reif ;
     rdfs:range ?a .
	?q rdfs:domain ?reif ;
     rdfs:range ?b .
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

<img width="505" height="234" src="https://github.com/user-attachments/assets/53a66b17-47c4-49ec-9004-8bc20b724332" />

<img width="502" height="240" src="https://github.com/user-attachments/assets/eaa40bae-2b26-471f-bf50-ec24cc927a42" />




## Transformation Pattern

First version of corresponding transformation pattern is available: <a href="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/blob/main/submissions/DERE1.json" target="_blank">DERE1.json</a>.
