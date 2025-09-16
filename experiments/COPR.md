# Link Counting Property Pattern

Creating a new dataproperty that counts the same objected property, which lead from the same object.

### SPARQL for detection

```
SELECT ?x (COUNT(?y) AS ?count)
		WHERE {?x ?p ?y}
		GROUP BY ?x   

```

## SPARQL Abox update:
```
INSERT {?x <newProperty> ?count .}
WHERE { 
	{
	SELECT ?x (COUNT(?y) AS ?count)
		WHERE {?x ?p ?y}
		GROUP BY ?x   
	}
} 
```

### SPARQL Tbox update
```
INSERT {
	<newProperty> a owl:DatatypeProperty;
	rdfs:domain ?A ;
	rdfs:range xsd:integer .}
WHERE {
	?p a owl:ObjectProperty;
	rdfs: domain ?A .
}
```



## Examples

### Artificial Example
Input:
```
  :Alice v:isACitizenOf :Prague, :Paris .
```
Output:
```
  :Alice vnew:numberOfCitizenships “2” .
```









## Transformation Pattern


