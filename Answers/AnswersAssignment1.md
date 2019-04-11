## What goes Where

#### Variable Names

**Question1:**

There are three variables written down in the query:
1. ?disease
1. ?variant
1. ?gene

### Full query

**Question 2A:**
The VALUES element: VALUES ?disease {wd:Q188874}

**Question 2B:**
The SERVICE element: SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en" }

## Change is Coming 

### More diseases:
**Question 3:** 
VALUES ?disease {wd:Q188874 wd:Q128581 wd:Q18556832}

### Which diseases?
**Question 4:** 
SELECT ?geneLabel ?variantLabel ?diseaseLabel


## Additional questions Assignment 1:

### ?Negatives

The line describing 'positive prognostic markers' can be changed to 'negative prognostic markers'. This property has the identifier P3359 in Wikidata (which you can find with the entry search function.

```SPARQL
?variant wdt:P3359 ?disease ; # P3358 Negative prognostic predictor
```

### ?References

First, we need to connect a new variable, ?article, to the ?variant variable. The relationship connecting these is called 'main subject' (wdt:P921). Last, we need to add the new variable to the _results clause_. The OPTIONAL statement allows us to also get results, if there is no article available for this variant.

See the full query below:


```SPARQL
SELECT ?geneLabel ?variantLabel ?articleLabel
WHERE
{ 
	VALUES ?disease {wd:Q188874}
    ?variant wdt:P3359 ?disease ; # P3358 Negative prognostic predictor
          wdt:P3433 ?gene . # P3433 biological variant of
    OPTIONAL{?article wdt:P921 ?variant} .
	SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en" }
}
```
