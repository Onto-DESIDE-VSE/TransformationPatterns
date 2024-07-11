# Object Property Chain Shortcutting Pattern

Shortening a property path.

## ABox Structure

Input
```
$x $p $y . $y $r $z .
```
Output
```
$x $q $z.
```
<img src="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/65444662/9b154c6d-66fd-410e-88e2-d712813a8f3b" width="400px">

### SPARQL ABox
```
INSERT {?x <newProperty> ?z}
WHERE{
	?x ?p ?y.
	?y ?r ?z.
}
```

## Semantic Links
- {$p, $r} → $q

## TBox Structure

### TBox hints

```
$p rdfs:range $a . $r rdfs:domain $a .
```

### SPARQL for detection

```
SELECT ?p ?r ?a ?b ?c
WHERE {
	?p rdfs:domain ?b .
	?p rdfs:range ?a .
	?r rdfs:domain ?a .
	?r rdfs:range ?c .
}

```

### SPARQL update

```
INSERT DATA {
<newProperty> a owl:ObjectProperty;
	rdfs:domain <b>;
	rdfs:range <c>. }
```


## Examples

### Artificial Example
Input:
```
  :OWLtopia v:hasCapital :Knowgratown .
  :Knowgratown v:hasMayor :JohnLinkedton .
```
Output:
```
  :OWLtopia vnew:hasMayorOfCapital :JohnLinkedton .
```

<img src="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/65444662/235a3241-f12a-4e71-83f8-695241c1520f" width="700px">


### Detection Examples in Ontologies

- [Real example 1](https://people.geog.ucsb.edu/~jano/odpld.pdf)


## Transformation Pattern

```
{
	"tp": {
		"op1": {
			"entity_declarations": {
				"placeholder": [
					{
						"type": "ObjectProperty",
						"name": "?p"
					},
					{
						"type": "ObjectProperty",
						"name": "?r"
					},
					{
						"type": "Class",
						"name": "?A"
					},
					{
						"type": "Class",
						"name": "?B"
					},
					{
						"type": "Class",
						"name": "?C"
					}
				]
			},
			"triples": {
				"triple": [
					"?p rdfs:range ?A",
					"?p rdfs:domain ?B",
					"?r rdfs:domain ?A",
					"?r rdfs:range ?C"
				]
			}
		},
		"op2": {
			"entity_declarations": {
				"placeholder": [
					{
						"type": "ObjectProperty",
						"name": "?q"
					},
					{
						"type": "Class",
						"name": "?D"
					},
					{
						"type": "Class",
						"name": "?E"
					}				
				]
			},
			"triples": {
				"triple": [
					"?q rdfs:domain ?D",
					"?q rdfs:range ?E"
				]
			}
		},
		"pt": {
			"eq": [
				{
					"op1": "?B",
					"op2": "?D"
				},
				{
					"op1": "?C",
					"op2": "?E"
				}				
			]
		},
		"_name": "op-ch-sh"
	}
}
```

## Transformation Pattern Lite

Evolved version see notes at [CAT1](https://github.com/Onto-DESIDE-VSE/TransformationPatterns/blob/main/experiments/CAT1.md#transformation-pattern-lite)

```
{
	"tp": {
	"name": "op-ch-sh",
		"op_source": {
			"triples": {
				"triple": [
					"?p rdfs:range ?A",
					"?p rdfs:domain ?B",
					"?r rdfs:domain ?A",
					"?r rdfs:range ?C"
				]
			}
		},
		"op_target": {			
			"triples": {
				"triple": [
					"?p rdfs:range ?A",
					"?p rdfs:domain ?B",
					"?r rdfs:domain ?A",
					"?r rdfs:range ?C",
					"?q rdfs:domain ?B",
					"?q rdfs:range ?C"
				]
			}
		}		
	}
}
```
## Transformation Pipeline Driven by Transformation Pattern Lite

Here is the example. Generally described at [CAT1](https://github.com/Onto-DESIDE-VSE/TransformationPatterns/blob/main/experiments/CAT1.md#transformation-pipeline-driven-by-transformation-pattern-lite)

1.

```
SELECT ?p ?r ?A ?B ?C
WHERE {
?p rdfs:range ?A .
?p rdfs:domain ?B .
?r rdfs:domain ?A .
?r rdfs:range ?C .
}					
```
2.

```
INSERT_axioms = {"?q rdfs:domain ?B", "?q rdfs:range ?C"}
```

```
DELETE_axioms = {}
```

```
new_entities = {?q}
```

Considering user have already made some selection and named new entity ?q:

```
INSERT DATA {
 :hasMayorOfCapital rdfs:domain :OWLtopia .
 :hasMayorOfCapital rdfs:range :JohnLinkedton .
}
```

**ABox**

ABox triples should be converted accordingly.

```
INSERT {?x <newProperty> ?z}
WHERE{
?x ?p ?y.
?y ?r ?z.
}
```

3. option to show SPARQL update, transformed ontology, (later on) visualization
