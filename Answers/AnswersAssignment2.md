## Step by Step

**Question 1A:** Which variables are depicted in which manner? 

1. The first results column (?drugLabel) is depicted on the horizontal (x) axis.
1. The second results column (?geneLabel) is depicted on the vertical (y) axis.
1. The third results column (?biological_processLabel) is depicted as a colour code on the data points.
1. The fourth results column (?diseaseLabel) is visualised in the top left corner.

With this data visualisation option, one can only visualise the results of 4 variables simultaneously.

**Question 1B:** What would change to the visualisation, if you switch the place of the variables ?geneLabel and ?biological_processLabel ?

This would switch the data on the vertical axis, with the data depicted by the colour codes (see example below). Choosing which result is display in which column has an effect on the interpretability of your data plot. Colour coding the different GO-terms related to biological processes, makes more sense then colour coding the different genes.

![Answer Q1B](/Images/Scatter_chart_visualisation_switched_variables_example2.jpg)

## Changing the Question

### ?biological_process

**TIPS:** Adding a VALUES statement will handle the required GO-terms. Furthermore, a new variable (called ?goterms) should be connected to these values, and should be added to the triplet where ?biological_process is related to the go terms. 

See the full query below:

```SPARQL
SELECT DISTINCT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel
WHERE {
  VALUES ?goterms {wd:Q14818032 wd:Q14599311 wd:Q2383867}
  ?drug wdt:P129 ?gene_product .   # drug interacts with a gene_product
  ?gene wdt:P688 ?gene_product .  # gene_product (usually a protein) is a product of a gene (a region of DNA)
  ?disease	wdt:P2293 ?gene .    # genetic association between disease and gene
  ?disease wdt:P279*  wd:Q12078 .  # limit to cancers wd:Q12078 (the * operator runs up a transitive relation..)
  ?gene_product wdt:P682 ?biological_process . #add information about the GO biological processes that the gene is related to 
  
   ?biological_process (wdt:P361|wdt:P279)* ?goterms .  # chain down subclass/part-of
   #Change the last statement (wd:Q14818032) to limit to genes related to certain biological processes (and their sub-processes):
  		#cell proliferation wd:Q14818032 (Current example)
        #apoptosis wd:Q14599311

    #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  #?disease wdt:P2176 ?drug . 	# disease is treated by a drug
  	SERVICE wikibase:label {
        bd:serviceParam wikibase:language "en" .
	}
}
LIMIT 1000
```

### ?drug structure

**TIPS:** Since we only want the InchiKey (a new variable!) when it is available, we will use the OPTIONAL statement. Within this statement, we want to connect the ?drug variable to the inchikey in Wikidata (which is linked with the property wdt:P235). Last, we need to add the new variable to the _results clause_.

See the full query below:

```SPARQL
SELECT DISTINCT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel ?inchikey
WHERE {
  VALUES ?goterms {wd:Q14818032 wd:Q14599311 wd:Q2383867}
  ?drug wdt:P129 ?gene_product .   # drug interacts with a gene_product
  OPTIONAL{?drug wdt:P235 ?inchikey} .
  ?gene wdt:P688 ?gene_product .  # gene_product (usually a protein) is a product of a gene (a region of DNA)
  ?disease	wdt:P2293 ?gene .    # genetic association between disease and gene
  ?disease wdt:P279*  wd:Q12078 .  # limit to cancers wd:Q12078 (the * operator runs up a transitive relation..)
  ?gene_product wdt:P682 ?biological_process . #add information about the GO biological processes that the gene is related to 
  
   ?biological_process (wdt:P361|wdt:P279)* ?goterms .  # chain down subclass/part-of
   #Change the last statement (wd:Q14818032) to limit to genes related to certain biological processes (and their sub-processes):
  		#cell proliferation wd:Q14818032 (Current example)
        #apoptosis wd:Q14599311

    #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  #?disease wdt:P2176 ?drug . 	# disease is treated by a drug
  	SERVICE wikibase:label {
        bd:serviceParam wikibase:language "en" .
	}
}
LIMIT 1000
```

### ?References

**TIPS:** We will first need to uncomment the line related to finding true positives. Second, we need to connect a new variable, ?article, to the ?drug variable. The relationship connecting these is called 'main subject' (wdt:P921). We also want to order our results based on the name of the drug, for which we can use a ORDER BY statement. Last, we need to add the new variable to the _results clause_.

See the full query below:

```SPARQL
SELECT DISTINCT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel ?inchikey ?articleLabel
WHERE {
  VALUES ?goterms {wd:Q14818032 wd:Q14599311 wd:Q2383867}
  ?drug wdt:P129 ?gene_product .   # drug interacts with a gene_product
  OPTIONAL{?drug wdt:P235 ?inchikey} .
  ?gene wdt:P688 ?gene_product .  # gene_product (usually a protein) is a product of a gene (a region of DNA)
  ?disease	wdt:P2293 ?gene .    # genetic association between disease and gene
  ?disease wdt:P279*  wd:Q12078 .  # limit to cancers wd:Q12078 (the * operator runs up a transitive relation..)
  ?gene_product wdt:P682 ?biological_process . #add information about the GO biological processes that the gene is related to 
  
   ?biological_process (wdt:P361|wdt:P279)* ?goterms .  # chain down subclass/part-of
   #Change the last statement (wd:Q14818032) to limit to genes related to certain biological processes (and their sub-processes):
  		#cell proliferation wd:Q14818032 (Current example)
        #apoptosis wd:Q14599311
       #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  ?disease wdt:P2176 ?drug . 	# disease is treated by a drug
 ?article wdt:P921 ?drug  .
  	SERVICE wikibase:label {
        bd:serviceParam wikibase:language "en" .
	}
}
ORDER BY ?drugLabel
LIMIT 1000
```
