# Class by Attribute Type (CAT)

Based on Class by Attribute Type (CAT) alignment pattern from [ScharffePhDThesis09].

## ABox structure

Input
```
$x $p $y . $y a $c 
```
Output
```
$x a $d .
```

## TBox structure
```
$p rdfs:domain $a ; rdfs:range $b . $c rdfs:subClassOf $b .
```

![image](https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/8502675/fe728e13-017b-4df0-b53b-c52f5de61dc9)


## SPARQL for detection (TBox)

Ontology for testing: [cmt](https://oaei.ontologymatching.org/2024/conference/data/cmt.owl)

```
SELECT DISTINCT ?A ?p ?B ?C
WHERE { 
  ?p rdfs:domain ?A .
  ?p rdfs:range ?B .
  ?C rdfs:subClassOf ?B 
} 
```

## SPARQL update (TBox)

```
TODO
```
## Transformation Pattern

This part demonstrates possible serialization of TP in
- XML
  - serialization in original patomat, omitting naming patterns
  - original axioms are in Manchester syntax; now as triples
- JSON
  - tailored to patomat2 and SPARQL usage
  - BUT strings cannot span across more lines
- YAML

**XML serialization** (updated, [original one using Manchester syntax](https://nb.vse.cz/~svabo/patomat/tp/new/tp_hasSome2.xml))

```
<tp name="CAT1">
  <op1>
    <entity_declarations>
      <placeholder type="ObjectProperty">?p</placeholder>
      <placeholder type="Class">?A</placeholder>
      <placeholder type="Class">?B</placeholder>
      <placeholder type="Class">?C</placeholder>
    </entity_declarations>
    <triples>
      <triple>?p rdfs:domain ?A</triple>
      <triple>?p rdfs:range ?B</triple>
      <triple>?C rdfs:subClassOf ?B</triple>
    </triple>    
  </op1>
  <op2>
    <entity_declarations>
      <placeholder type="ObjectProperty">?q</placeholder>
      <placeholder type="Class">?D</placeholder>
      <placeholder type="Class">?E</placeholder>
      <placeholder type="Class">?F</placeholder>
      <placeholder type="Class">?G</placeholder>
    </entity_declarations>
    <triples>
      <triple>?q rdfs:domain ?D</triple>
      <triple>?q rdfs:range ?E</triple>
      <triple>?F rdfs:subClassOf ?E</triple>
      <triple>_:restriction rdf:type owl:Restriction</triple>
      <triple>_:restriction owl:onProperty ?q</triple>
      <triple>_:restriction owl:someValuesFrom ?F</triple>
    </triples>
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
			"triples": {
				"triple": [
					"?p rdfs:domain ?A",
					"?p rdfs:range ?B",
					"?C rdfs:subClassOf ?B"
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
			"triples": {
				"triple": [
					"?q rdfs:domain ?D",
					"?q rdfs:range ?E",
					"?F rdfs:subClassOf ?E",
					"_:restriction rdf:type owl:Restriction",
					"_:restriction owl:onProperty ?q",
					"_:restriction owl:someValuesFrom ?F"
				]
			}
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
		},
		"_name": "CAT1"
	}
}
```

<!-- **YAML**
We might try, but XML or JSON should be fine.
```
```
-->

## Transformation Pattern Lite

OZ NOTE After some consideration, I think we can try simplification of transformation pattern structure wrt. original patomat due to new SPARQL perspective. I would say we do not need two sets of placeholders. We do not even need to declare placeholders at all. But, it can happen that we will have to return it back later on. The structure can evolve. However, I would start with simpler one now.

```{
    "tp": {
        "name": "CAT1",
        "op_source": {
            "triples": {
                "triple": [
                    "?p rdfs:domain ?A",
                    "?p rdfs:range ?B",
                    "?C rdfs:subClassOf ?B"
                ]
            }
        },
        "op_target": {
            "triples": {
                "triple": [
                    "?p rdfs:domain ?A",
                    "?p rdfs:range ?B",
                    "?C rdfs:subClassOf ?B",
                    "?G rdfs:subClassOf ?A",
                    "?G owl:equivalentClass _:restriction",
                    "_:restriction rdf:type owl:Restriction",
                    "_:restriction owl:onProperty ?p",
                    "_:restriction owl:someValuesFrom ?C"
                ]
            }
        },
        "naming_transformation": [
            {                
                "lex": [
                    {
                        "?G": ["?C", "?A"]
                    }
                ],
                "ntp": [
                    {
                        "?G": ["?A that ?p a ?C"]
                    }
                ]
            }
        ]
    }
}
```
## Transformation Pipeline Driven by Transformation Pattern Lite

We describe the step-by-step pipeline of applying transformation pattern on ontology.

1. detection using SPARQL (user interaction)

Use

```
$.tp.op_source.triples
```
for SPARQL query preparation:

```
SELECT (extract_variables_from_triples($.tp.op_source.triples))
WHERE {
  $.tp.op_source.triples
}
```

**CAT1 Example**

(extract_variables_from_triples($.tp.op_source.triples))={?A, ?p, ?B, ?C}

```
SELECT ?A ?p ?B ?C 
WHERE { 
	?p rdfs:domain ?A .
	?p rdfs:range ?B .
	?C rdfs:subClassOf ?B 
} 
```
This is basically already implemented. Now user could select pattern instance for transformation.

**TODO** New feature is related to lexicalization which should be output for example for LLM trainining based on `$.tp.naming_transformation.lex`

```
"naming_transformation": [
    {                
	"lex": [
	    {
		"?G": ["?C", "?A"]
	    }
	],
	"ntp": [
	    {
		"?G": ["make_passive_verb(?C)+head_noun(?A)", "make_passive_verb(?C)"]
	    }
	]
    }
]
```

2. SPARQL update preparation (user interaction)

Use

```
$.tp.op_source.triples
```

and

```
$.tp.op_target.triples
```

For SPARQL update preparation. User interaction: axiom selection and entities naming.

```
INSERT_triples = $.tp.op_target.triples - $.tp.op_source.triples
```

```
DELETE_triples = $.tp.op_source.triples - $.tp.op_target.triples
```

```
new_entities = extract_variables_from_triples($.tp.op_target.triples) - extract_variables_from_triples($.tp.op_source.triples)
```

**CAT1 Example**

```
INSERT_triples = {"?G rdfs:subClassOf ?A", "?G owl:equivalentClass _:restriction","_:restriction rdf:type owl:Restriction", "_:restriction owl:onProperty ?p", "_:restriction owl:someValuesFrom ?C"}
```

```
DELETE_triples = {}
```

```
new_entities = {?G}
```

Next, we will show the user suggestions on which axioms should be added **INSERT_triples_selected** and which axioms could be removed **DELETE_triples_selected**. There will also be some newly added entities **new_entities** - for the beginning, they can be named randomly. IRIs of new entities will be generated but labels will be prepared according to instrutions in **ntp**. Users will have an option to edit the labels. Next, the user will confirm the INSERT and DELETE changes. The corresponding SPARQL will be generated:

```
"?A that ?p a ?C"
```
For example-> "Paper that hasDecision a Accept"



```
INSERT {
  INSERT_triples_selected
}
WHERE {
  $.tp.op_source.triples
}
```
NOTE But, variables will be rather already bound from user interaction, i.e., INSERT DATA without WHERE clause.

(To start we can consider only the INSERT part and focus on DELETE later on)

```
DELETE {
  DELETE_triples_selected
}
WHERE {
 $.tp.op_source.triples   
}
```

**CAT1 Example**
```
INSERT {
  ?G rdfs:subClassOf ?A .
  ?G owl:equivalentClass _:restriction .
  _:restriction rdf:type owl:Restriction .
  _:restriction owl:onProperty ?p .
  _:restriction owl:someValuesFrom ?C 
}
WHERE {
  ?p rdfs:domain ?A .
  ?p rdfs:range ?B .
  ?C rdfs:subClassOf ?B
}
```

NOTE But, variables will be rather already bound from user interaction, i.e., INSERT DATA without WHERE clause, e.g.

```
INSERT DATA {
  :AcceptedPaper rdfs:subClassOf :Paper.
  :AcceptedPaper owl:equivalentClass _:restriction .
  _:restriction rdf:type owl:Restriction .
  _:restriction owl:onProperty :hasDecision .
  _:restriction owl:someValuesFrom :Acceptance
}
```
**ABox**
ABox triples should be converted accordingly. Interesting example is within the [Class hierarchy conversion to codelist](https://docs.google.com/document/d/1DgX78h7WpHdcytKjxzsYCVbSrOQLCMYQ97odE8OUY-w/edit#heading=h.tzov4axobh0t) where we could add instance into op_source and op_target and try to derive instructions for ABox update using SPARQL update.

```
TODO example
```

3. result to download, show, visualize

The result can be both SPARQL Update and transformed ontology for download.

TODO later on we should also involve visualization, e.g. using [VOWL]((for start we can consider only INSERT part and focus on DELETE later on))


