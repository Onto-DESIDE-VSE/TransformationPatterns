# Object Property Chain Shortcutting Pattern

Shortening a property path.

## ABox

Input:
```
$x $p $y . $y $r $z .
```
Output:
```
$x $q $z.
```
<img src="https://github.com/Onto-DESIDE-VSE/TransformationPatterns/assets/65444662/9b154c6d-66fd-410e-88e2-d712813a8f3b" width="400px">


## Links
- {$p, $r} → $q

## TBox hints

```
$p rdfs:range $a . $r rdfs:domain $a .
```

## SPARQL for detection

```
SELECT ?p ?r ?a
WHERE {
  ?p rdfs:range ?a.
  ?r rdfs:domain ?a.
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

### Detection Examples in Ontologies

- [Real example 1](https://people.geog.ucsb.edu/~jano/odpld.pdf)



