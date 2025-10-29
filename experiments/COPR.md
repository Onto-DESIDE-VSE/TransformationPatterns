# Link Counting Property Pattern

Creating a new dataproperty that counts the same objected property, which lead from the same object.

### SPARQL for detection (Tbox)

```sparql
SELECT ?p ?a ?b
WHERE {
	?p rdf:type owl:ObjectProperty;
		rdfs:domain ?a;
		rdfs:range ?b.
}
```

### SPARQL for detection (Abox)

```sparql
SELECT ?x ?p (COUNT(?y) AS ?count)
		WHERE {?x ?p ?y}
		GROUP BY ?x ?p
        HAVING (COUNT(?y) >= 2) 
```

## SPARQL Abox update:
```sparql
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
```sparql
INSERT {
	<newProperty> a owl:DatatypeProperty;
	rdfs:domain ?A ;
	rdfs:range xsd:integer .}
WHERE {
	?p a owl:ObjectProperty;
	rdfs: domain ?A .
}
```
<img width="246" height="426" alt="Tbox diagram for COPR pattern" src="https://github.com/user-attachments/assets/701f6b6b-37f7-430f-af45-ed00798175c8" />


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

<img width="382" height="386" alt="Example of COPR pattern" src="https://github.com/user-attachments/assets/7aa3c5ca-6a94-4dd1-b0c0-71acaa758785" />










## Transformation Pattern


