Example 1: Which variant of which gene predicts a positive prognosis in colorectal cancer
=================

During this assignment, we will have a closer look at an example SPARQL query of Wikidata, called ["Which variant of which gene predicts a positive prognosis in colorectal cancer"](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Which_variant_of_which_gene_predicts_a_positive_prognosis_in_colorectal_cancer). We will first go through the basics of a SPARQL query. Second, we will find out how to execute the query and retain results. Last, we will expand the query and make other (small) changes, to understand the structure of a SPARQL query better, and see what other data is available in Wikidata.

## What goes Where

A SPARQL query consist out of several elements, which can be considered as building blocks. 
We will use the following [example](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Which_variant_of_which_gene_predicts_a_positive_prognosis_in_colorectal_cancer), which is part of the example page of SPARQL queries for Wikidata. 
We will exploring the example question called **"Which variant of which gene predicts a positive prognosis in colorectal cancer"** in more detail below.

### First element: SELECT

The first element we encouter in this example, is the so-called _result clause_, which identifies what information to return from the query. 
This element starts with the word SELECT, and is then followed by two words with a questionmark in front of them:

```sparql 
SELECT ?geneLabel ?variantLabel
```

SELECT is used to indicate with variables from the (to follow) SPARQL query you want to visualise as a result (in other words: which variables we need to answer our biological question). In this example, the name(s) of the gene(s) predicting a positive prognosis in colorectal cancer (?geneLabel), and the name of the variant(s) belonging to this gene/these genes (?variantLabel).

### Second element: WHERE

The second element we encouter, is the _query pattern_, which starts with the word WHERE, with the query itself enclosed in curly brackets: {} .

```sparql 
WHERE
{ 
	query
}
```

#### Variable Names

Within these brackets, the data from an RDF is connected to variable names. We already encoutered two variable names, ?geneLabel and ?variantLabel (indicated by the questionmark). These variables can have any name you see fit. 

**Question1:** Which other variable names are present in the following SPARQL query? (Answers can be found [here](/Answers/AnswersAssignment1.md)).

```sparql 
{ 
	VALUES ?disease {wd:Q188874}
    ?variant wdt:P3358 ?disease ; # P3358 Positive prognostic predictor
          wdt:P3433 ?gene . # P3433 biological variant of
}
```

#### Query Details

In between the curly brackets, we can see three lines to describe the query we want to execute:
1. VALUES ?disease {wd:Q188874}
1. ?variant wdt:P3358 ?disease ; # P3358 Positive prognostic predictor
1. wdt:P3433 ?gene . # P3433 biological variant of

##### Line 1
The first line starts with VALUES; this allows you to queries multiple items, specified in the data block between the brackets (separated with spaces). The variable after VALUES (in this case ?disease) will be completed with the items in the data block. In this case, we are only interested in "colorectal cancer", with 'Q188874' being the identifier of the entry in Wikidata (these always start with a Q) for **Colon Cancer**. To explain to which database this identifier belongs, we add the 'wd:' before the number of the identifier.

##### Line 2
The second line consists of two main parts, the left part is related to the query (which ends with a semicolon ; ), the right is a comment, to explain what the line does (starting with a hash #). The left part is formed in the triplet structure (which we previously encountered in the presentation):

```sparql 
{
?variant wdt:P3358 ?disease ;
}
```

In this case, we are looking for variant(s) (_subject_), which are related to disease(s) (_object_). The relationship (aka _predicate_) is defined as the center part of the triplet, with 'P3358' being the identifier of the property in Wikidata (these always start with a P) for **Positive prognostic predictor**. To explain to which database this relationship belongs, we add the 'wdt:' before the identifier of the property. Notice the small difference with line one; 'wd:' is used for entries (these can serve as subjects or objects), 'wdt:' for properties (or relationships).

This line ends with a semicolon ';' to end our first triplet.

##### Line 3
The third line again consists out of two parts; we will only discuss the left had side, until the point '.' .

```sparql 
{
wdt:P3433 ?gene .
}
```
Even though this line does not directly look like a triplet (since there are only two element), it is read as a triplet.
A point '.' at the end of a triplet really notes the end, while a semicolon ';' notes that another triplet will follow, where the _subject_ may be ommited from writing. Therefore, we need to look back at line 2, to find out which _subject_ is belonging to the triplet in line 3:

```sparql 
{
?variant wdt:P3433 ?gene .
}
```

In this case, we are defining how genes are related to the variants in Wikidata. We are connecting the variant(s) (_subject_) with genes (_object_) in Wikidata, with the following relationship(_predicate_): "P3433", which is also known as "biological variant of".

### Third element: SERVICE

Not that the ?geneLabel and ?variantLabel are not written down in the query, however the variables ?gene and ?variant are. This is possible with the last part of the query, the SERVICE element. By typing the word "Label" (including the capital!) behind the name of a variable, we can obtain the name of that variable, in stead of the identifier used by the RDF. A name makes much more sense to us humans, and allows us to interpret the results.

```sparql 
{ 
	SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en" }
}
```

### Full query

When we combine the three elements above, we get the full query:

```
SELECT ?geneLabel ?variantLabel
WHERE
{ 
	VALUES ?disease {wd:Q188874}
    ?variant wdt:P3358 ?disease ; # P3358 Positive prognostic predictor
          wdt:P3433 ?gene . # P3433 biological variant of
	SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en" }
}
```
We wanted an answer for the following question: "Which variant of which gene predicts a positive prognosis in colorectal cancer".

**Question2A:** Which item in the SPARQL query corresponds to the disease(s) being queried? 
**Question2B:** Which item in the SPARQL query adds a name to the results?

(Answers can be found [here](/Answers/AnswersAssignment1.md)). 

We will now look how to run this query on the data from Wikidata, and how we can save the results from that query.

## Run and Save

ipse lorum

## Change is Coming 

ipse lorum


