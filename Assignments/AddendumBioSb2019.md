## Q and A:

Several questions were asked during the workshop. Below, you can find these questions and answers:


**Q1:** How do I change the language settings of the SPAQRL Endpoint?

Answer: Several participants noticed that their results were displayed in Dutch (Nederlands) when they opened the SPARQL endpoint. This setting can be changed in the top right corner:

![Pref Language](../Images/Preffered_lanuage-Wikidata.jpg)

**Q2:** How can I obtain the query helper menu?

Answer: On the top left side, click on the blue 'i'. This opens up a separate menu, which helps you to construct queries, and see which properties (aka relationships) you want to query:

![Pref Language](../Images/query_helper_Wikidata.jpg)

By clicking this button, the Query constructor will expand:

![Pref Language](../Images/Query_helper_opened.jpg)

**Issue Tracker:**

If you are not new to GitHub, you can add other questions to the [issues tracker](https://github.com/BiGCAT-UM/SPARQLTutorialBioSB2019/issues).
If you are new to GitHub, please consult the following documentation on getting an [account](https://www.wikihow.com/Create-an-Account-on-GitHub) and adding an [issue](https://help.github.com/en/articles/creating-an-issue).

Please note that we will maintain this project for reflection purposes of participants (or others that are interested).
New assignments will be created on the [main project](https://github.com/DeniseSl22/SPARQLTutorialBioSB2019). Other SPARQL tutorials given by BiGCaT (Maastricht University) in the future can be found in our [repository list](https://github.com/bigcat-um).

### SERVICE clause additional information:

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
