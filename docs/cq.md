## Competency Questions

BiRO can be used for answering several questions related to bibliographic records and references.
In the following subsections, some of them are introduced together with their respective SPARQL queries. 

The prefixes that are used in all the SPARQL queries provided below are defined as follows:

    PREFIX biro: <http://purl.org/spar/biro/>
    PREFIX co: <http://purl.org/co/>
    PREFIX dcterms: <http://purl.org/dc/terms/>
    PREFIX frbr: <http://purl.org/vocab/frbr/core#>>
    PREFIX foaf: <http://xmlns.com/foaf/0.1/>
    PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>

### CQ1

Which reference lists are contained within a paper?

    SELECT ?paper ?referenceList
    WHERE {
        ?paper frbr:part ?referenceList .
        ?referenceList a biro:ReferenceList .
    }

### CQ2

What is the ordered sequence of references in a reference list?

    SELECT ?referenceList ?listItem ?reference ?nextItem
    WHERE {
        ?referenceList a biro:ReferenceList ;
            co:item ?listItem .
        ?listItem co:itemContent ?reference .
        OPTIONAL { ?listItem co:nextItem ?nextItem . }
    }

### CQ3

Which cited works do bibliographic references refer to?

    SELECT ?reference ?citedWork
    WHERE {
        ?reference a biro:BibliographicReference ;
            biro:references ?citedWork .
    }

### CQ4

What are the constituent literal parts and values that form a bibliographic reference?

    SELECT ?reference ?partItem ?literalNode ?literalValue ?nextPartItem
    WHERE {
        ?reference a biro:BibliographicReference ;
            co:item ?partItem .
        ?partItem co:itemContent ?literalNode .
        ?literalNode literal:hasLiteralValue ?literalValue .
        OPTIONAL { ?partItem co:nextItem ?nextPartItem . }
    }