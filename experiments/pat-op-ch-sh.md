# Object Property Chain Shortcutting Pattern

Shortening a property path.

## ABox structure

Input
```
$x $p $y . $y $r $z .
```
Output
```
$x $q $z.
```
<img src="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/65444662/9b154c6d-66fd-410e-88e2-d712813a8f3b" width="400px">


### Semantic links
- {$p, $r} → $q

## TBox hints

```
$p rdfs:range $a . $r rdfs:domain $a .
```

## SPARQL for detection (TBox)

```
SELECT ?p ?r ?a
WHERE {
  ?p rdfs:range ?a.
  ?r rdfs:domain ?a.
}
```

## SPARQL update (TBox)

```
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



