# Class hierarchy conversion to codelist

A hierarchy of classes is transformed to a hierarchy of metamodeling individuals. (The target can possibly be a SKOS taxonomy, but this would be an extension.) The instantiations to these classes then become dedicated properties.

## ABox Structure

Input
```
$x rdfs:subClassOf $y . $a a $x .
```
Output
```
$x1 $p $y1 .  $a $q $x1 .
```
<img src="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/65444662/9ca6f778-d8c0-4471-96d4-4fc131b0ddf7" width="600px">


### SPARQL ABox
```
INSERT {
?a <q> <x1>
<x1> <p> <y1>.
}
WHERE{
?a rdf:type ?x.
x? rdfs:subClassOf y?
}
```

## Semantic Links
- $x → $x1
- $y → $y1
- rdfs:subClassOf → $p
- {rdf:type, $y} → $q


## TBox Structure

### TBox hints

```
$x rdfs:subClassOf $y .
```

### SPARQL for detection

```
SELECT DISTINCT ?x ?y
WHERE{
x? rdfs:subClassOf y?
}
```

### SPARQL update

```
TBD
```


## Examples

### Artificial Example
Input:
```
:Metropoly rdfs:subClassOf :City .
:OWLtopia a :Metropoly .  
```
Output:
```
:MetropolyCityType v:subTypeOf :CityType . 
:OWLtopia v:hasCityType :MetropolyCityType.
```

<img src="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/65444662/d132c244-3830-429e-b8f0-b58c0ced05ed" width="600px">



### Detection Examples in Ontologies

- TBD


## Transformation Pattern
