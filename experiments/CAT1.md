# Class by Attribute Type (CAT)

Based on Class by Attribute Type (CAT) alignment pattern from [ScharffePhDThesis09].

## ABox structure

Input
```
TODO
```
Output
```
TODO
```

## TBox structure

![image](https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/8502675/fe728e13-017b-4df0-b53b-c52f5de61dc9)


## SPARQL for detection (TBox)

```
SELECT DISTINCT ?class1 ?property1 ?class2 ?ent1 
WHERE { 
  ?property1 rdfs:domain ?ent1 .
  ?property1 rdfs:range ?class1 .
  ?class2 rdfs:subClassOf ?class1 
} 
```

## SPARQL update (TBox)

```
TODO
INSERT {
  <newProperty> a owl:ObjectProperty;
      rdfs:domain ?x;
      rdfs:range ?z.
}
WHERE {
  ?p a owl:ObjectProperty ;
      rdfs:domain ?x;
      rdfs:range ?a.
  ?r a owl:ObjectProperty ;
      rdfs:domain ?a;
      rdfs:range ?z.
}
```
## Transformation Pattern

This part demonstrates possible serialization of TP in
- XML
  - serialization in original patomat, omitting naming patterns
  - axioms are in Manchester syntax.
- JSON
  - tailored to patomat2 and SPARQL usage
  - BUT strings cannot span across more lines
- YAML

**XML serialization**

```
<tp name="tp_hasSome2" xmlns="http://nb.vse.cz/~svabo/patomat/tp/tp-schema.xsd">
  <op1>
    <entity_declarations>
      <placeholder type="ObjectProperty">?p</placeholder>
      <placeholder type="Class">?A</placeholder>
      <placeholder type="Class">?B</placeholder>
      <placeholder type="Class">?C</placeholder>
    </entity_declarations>
    <axioms>
      <axiom>ObjectProperty: ?p Domain: ?A</axiom>
      <axiom>ObjectProperty: ?p Range: ?B</axiom>
      <axiom>Class: ?C SubClassOf: ?B</axiom>
    </axioms>    
  </op1>
  <op2>
    <entity_declarations>
      <placeholder type="ObjectProperty">?q</placeholder>
      <placeholder type="Class">?D</placeholder>
      <placeholder type="Class">?E</placeholder>
      <placeholder type="Class">?F</placeholder>
      <placeholder type="Class">?G</placeholder>
    </entity_declarations>
    <axioms>
      <axiom>ObjectProperty: ?q Domain: ?D</axiom>
      <axiom>ObjectProperty: ?q Range: ?E</axiom>
      <axiom>Class: ?F SubClassOf: ?E</axiom>
      <axiom>Class: ?G EquivalentTo: (?q some ?F)</axiom>
    </axioms>
  </op2>
  <pt>
    <eq op1="?A" op2="?D"/>
    <eq op1="?B" op2="?E"/>
    <eq op1="?C" op2="?F"/>
    <eq op1="?p" op2="?q"/>    
  </pt>
</tp>
```

**JSON**

```
{
	"tp": {
    "tp_name": "tp_hasSome2",
		"op1": {
			"entity_declarations": {
				"placeholder": [
					{
						"type": "ObjectProperty",
						"name": "?p"
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
			"where_clause": 
					"ObjectProperty: ?p Domain: ?A . ObjectProperty: ?p Range: ?B . Class: ?C SubClassOf: ?B ."
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
					},
					{
						"type": "Class",
						"name": "?F"
					},
					{
						"type": "Class",
						"name": "?G"
					}
				]
			},
			"insert_clause":				
					"ObjectProperty: ?q Domain: ?D . ObjectProperty: ?q Range: ?E . Class: ?F SubClassOf: ?E . Class: ?G EquivalentTo: (?q some ?F)"			
		},
		"pt": {
			"eq": [
				{
					"op1": "?A",
					"op2": "?D"
				},
				{
					"op1": "?B",
					"op2": "?E"
				},
				{
					"op1": "?C",
					"op2": "?F"
				},
				{
					"op1": "?p",
					"op2": "?q"
				}
			]			
		}				
	}
}
```

**YAML**

```
```

