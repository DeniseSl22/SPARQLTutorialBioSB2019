Assignment 2: Which variant of which gene predicts a positive prognosis in colorectal cancer
=================

During this assignment, we will investigate another example SPARQL query of Wikidata, called ["Find drugs for cancers that target genes related to cell proliferation"](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Find_drugs_for_cancers_that_target_genes_related_to_cell_proliferation). We will first go through the basics of a SPARQL query. Second, we will find out how to execute the query and retain or share results. Last, we will expand the query and make other (small) changes, to understand the structure of a SPARQL query better, and see what other data is available in Wikidata.

## Step by Step

We will use the following [example](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Find_drugs_for_cancers_that_target_genes_related_to_cell_proliferation), which makes use of Gene Ontology terms in Wikidata and drug information.

### New building blocks
The complete query is depicted below, and has one new building block, called LIMIT. This is a so-called _solution modifier_, which limits the number of rows returned from a query. In the example below, we will recieve a maximum of 1000 rows as a result.

#### LIMIT
```SPARQL
SELECT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel
WHERE {
  ?drug wdt:P129 ?gene_product .   # drug interacts with a gene_product
  ?gene wdt:P688 ?gene_product .  # gene_product (usually a protein) is a product of a gene (a region of DNA)
  ?disease	wdt:P2293 ?gene .    # genetic association between disease and gene
  ?disease wdt:P279*  wd:Q12078 .  # limit to cancers wd:Q12078 (the * operator runs up a transitive relation..)
  ?gene_product wdt:P682 ?biological_process . #add information about the GO biological processes that the gene is related to 
  
   ?biological_process (wdt:P361|wdt:P279)* wd:Q14818032 .  # chain down subclass/part-of
   #Change the last statement (wd:Q14818032) to limit to genes related to certain biological processes (and their sub-processes):
  		#cell proliferation wd:Q14818032 (Current example)
                #apoptosis wd:Q14599311

    #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  #?disease wdt:P2176 ?drug . 	# disease is treated by a drug
  	SERVICE wikibase:label {bd:serviceParam wikibase:language "en" .	}
}
LIMIT 1000
```

There are other _solution modifiers_ available, such as ORDER BY, which can be used to sort the query solutions on the value of one or more variables.

#### SELECT DISTINCT

1. Run the query above in the SPARQL endpoint, and look how many results you have.
1. Change the _result clause_ to the following line, and look at the amount of results.

```SPARQL
SELECT DISTINCT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel
```

The DISTINCT modifier removes duplicate rows from the query results.

#### (Un)Comment

1. Comments (depicted with a hash '#') can help explaining the query (not only to others, but also for yourself).
1. Within comments, one can also add additional query options, which has been done in the following lines:
```SPARQL
  #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  #?disease wdt:P2176 ?drug . 	# disease is treated by a drug
```
You can uncomment by removing the '#' sign, and run the query again.

#### Counting Drugs

1. Comment the line related to true positives again.
1. Change the _result clause_ to the following line:
```SPARQL
SELECT DISTINCT ?drugLabel ?geneLabel ?biological_processLabel (COUNT (?drug) as ?drugcount)  ?diseaseLabel
```
3. Press run, and see what happens. 
![Wikidata Error](/Images/WD_badAggregate.JPG)

You will see an error occuring, because there is an item missing from this query. In this case, we added an _Aggregate Function_ called COUNT, which can calculate a single value from a set of results. We want to count how often a drug is occuring in our results, regardless of the disease or gene it is working on. In order to do so, we need to add one more line, just above the LIMIT section, like the example below:
```SPARQL
}
GROUP BY ?drugLabel ?geneLabel ?biological_processLabel  ?diseaseLabel
LIMIT 1000
```
