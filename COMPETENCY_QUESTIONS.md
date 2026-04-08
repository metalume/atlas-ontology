# ATLAS Competency Questions

These questions define what ATLAS should be able to answer. They guide ontology development and serve as acceptance criteria. Each question is paired with a SPARQL query sketch that should return correct results when run against a conformant ATLAS knowledge base.

## Core Classification

**CQ1**: What unreliability modes does document X have?

```sparql
SELECT ?mode ?role WHERE {
  ex:doc_X atlas:primaryUnreliabilityMode ?mode .
  BIND("primary" AS ?role)
} UNION {
  ex:doc_X atlas:secondaryUnreliabilityMode ?mode .
  BIND("secondary" AS ?role)
}
```

**CQ2**: What is the default severity of document X's primary failure mode?

```sparql
SELECT ?severity WHERE {
  ex:doc_X atlas:primaryUnreliabilityMode ?mode .
  ?mode atlas:defaultSeverity ?severity .
}
```

**CQ3**: What top-level category does unreliability mode Y fall under?

```sparql
SELECT ?category WHERE {
  ?mode rdfs:subClassOf+ ?category .
  ?category rdfs:subClassOf atlas:UnreliabilityMode .
  FILTER(?mode = atlas:BiofieldEnergyHealing)
  BIND(?category AS ?category)
}
```

## Detection Markers

**CQ4**: What detection markers have been observed for document X?

```sparql
SELECT ?marker WHERE {
  ex:doc_X atlas:detectionMarkerObserved ?marker .
}
```

**CQ5**: Which detection markers are evidence for unreliability mode Y?

```sparql
SELECT ?marker ?strength WHERE {
  ?marker atlas:evidenceFor ?mode .
  ?marker atlas:evidenceStrength ?strength .
  FILTER(?mode = atlas:PapermillOperation)
}
```

**CQ6**: Which unreliability modes could be indicated by detection marker M?

```sparql
SELECT ?mode WHERE {
  atlas:TorturedPhrases atlas:evidenceFor ?mode .
}
```

## Scientific Domain

**CQ7**: What is the claimed scientific domain of document X?

```sparql
SELECT ?topic ?subfield ?field ?domain WHERE {
  ex:doc_X atlas:claimedDomain ?topic .
  ?topic atlas:parentSubfield ?subfield .
  ?subfield atlas:parentField ?field .
  ?field atlas:parentDomain ?domain .
}
```

**CQ8**: What are the sibling topics of topic T?

```sparql
SELECT ?sibling WHERE {
  atlas:oa_topic_T12345 atlas:siblingTopic ?sibling .
}
```

## Vocabulary Inheritance

**CQ9**: What is the full inherited vocabulary for unreliability mode Y?

This requires traversing the class hierarchy and collecting lexicon file references:

```sparql
SELECT ?mode ?lexiconFile WHERE {
  atlas:BiofieldEnergyHealing rdfs:subClassOf* ?mode .
  ?mode atlas:lexiconFile ?lexiconFile .
}
```

The downstream consumer then loads and merges all referenced lexicon files.

**CQ10**: What is the parent chain for unreliability mode Y?

```sparql
SELECT ?ancestor WHERE {
  atlas:BiofieldEnergyHealing rdfs:subClassOf+ ?ancestor .
  ?ancestor rdfs:subClassOf* atlas:UnreliabilityMode .
}
ORDER BY DESC(?depth)
```

## Cross-Dimensional Queries

**CQ11**: Which documents in domain D have unreliability mode Y?

```sparql
SELECT ?doc WHERE {
  ?doc atlas:claimedDomain atlas:oa_topic_T12345 .
  ?doc atlas:primaryUnreliabilityMode atlas:Pseudoscience .
}
```

**CQ12**: Which corpus family does document X belong to?

```sparql
SELECT ?family WHERE {
  ex:doc_X atlas:corpusFamily ?family .
}
```
