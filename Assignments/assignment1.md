Example 1: Which variant of which gene predicts a positive prognosis in colorectal cancer
=================

During this assignment, we will have a closer look at an example SPARQL query of Wikidata, called ["Which variant of which gene predicts a positive prognosis in colorectal cancer"](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Which_variant_of_which_gene_predicts_a_positive_prognosis_in_colorectal_cancer). We will first go through the basics of a SPARQL query. Second, we will find out how to execute the query and retain results. Last, we will expand the query and make other (small) changes, to understand the structure of a SPARQL query better, and see what other data is available in Wikidata.

## What goes Where

A SPARQL query consist out of several elements, which can be considered as building blocks. 
We will use the following [example](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Which_variant_of_which_gene_predicts_a_positive_prognosis_in_colorectal_cancer), which is part of the example page of SPARQL queries for Wikidata. 
We will exploring the exampled called **"Which variant of which gene predicts a positive prognosis in colorectal cancer"** in more detail below.

The first element we encouter, is the so-called _result clause_, which identifies what information to return from the query. 
This element starts with the word SELECT, and then two words with a questionmark in front of them:

```sparql 
SELECT ?geneLabel ?variantLabel
```

SELECT is used to indicate with variables from the (to follow) SPARQL query you want to visualise as a result, or which variables we need to answer our biological question. In this example, the name(s) of the gene(s) predicting a positive prognosis in colorectal cancer (?geneLabel), and the name of the variant belonging to this gene (?variantLabel).

The second element we encouter, is the _query pattern_, which starts with the word WHERE, with the query itself enclosed in curly brackets: {} .

```sparql 
WHERE
{ 
	query
}
```

Within these brackets, the data from an RDF is connected to variable names. We already encoutered two variable names, ?geneLabel and ?variantLabel (indicated by the questionmark). These variables can have any name you see fit. Try to find out which other variable names are present in the following SPARQL query (answers can be found here).

```sparql 
{ 
	VALUES ?disease {wd:Q188874}
    ?variant wdt:P3358 ?disease ; # P3358 Positive prognostic predictor
          wdt:P3433 ?gene . # P3433 biological variant of
	SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en" }
}
```

## Run and Save

ipse lorum

## Change is Coming 

ipse lorum


