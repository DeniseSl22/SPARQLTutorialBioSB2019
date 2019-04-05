### SERVICE clause:

The SERVICE element is a neat trick build into the SPARQL endpoint of Wikidata.
The SPARQL way of getting the label of an item, is depicted in the following example"

```sparql
OPTIONAL {
		?event rdfs:label ?eventLabel.
		FILTER(LANG(?eventLabel) = "en").
	}
```

If we would integrate this into the example of [Assignment 1](../Assignments/assignment1.md), we would get the following query"

```sparql
SELECT ?geneLabel ?variantLabel
WHERE
{ 
	VALUES ?disease {wd:Q188874}
    ?variant wdt:P3358 ?disease ; # P3358 Positive prognostic predictor
          wdt:P3433 ?gene . # P3433 biological variant of
	OPTIONAL {?gene rdfs:label ?geneLabel.
		FILTER(LANG(?geneLabel) = "en").
	}
  
  OPTIONAL {?variant rdfs:label ?variantLabel.
		FILTER(LANG(?variantLabel) = "en").
	}
  
}
```
