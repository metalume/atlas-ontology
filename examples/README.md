# ATLAS Usage Examples

This directory contains ABox (assertional) examples showing how to annotate
documents using the ATLAS ontology. These are illustrative — they demonstrate
correct usage patterns, not a comprehensive corpus.

## Files

- `trivedi-2016.ttl` — Annotating a biofield energy healing paper from a predatory journal, with corpus family assignment.
- `example-queries.sparql` — SPARQL queries demonstrating competency questions against the example data.

## How to Use

1. Import the ATLAS ontology: `owl:imports <https://w3id.org/atlas/ontology/atlas.ttl>`
2. Create individuals of type `atlas:AnnotatedDocument`
3. Assign `atlas:primaryUnreliabilityMode` (exactly one)
4. Optionally assign `atlas:secondaryUnreliabilityMode` (zero or more)
5. Optionally record `atlas:detectionMarkerObserved` (zero or more)
6. Assign `atlas:claimedDomain` to an OpenAlex topic (if domain classification is relevant)
7. Assign `atlas:corpusFamily` if the document belongs to a known group

## Note on Severity

ATLAS provides `atlas:defaultSeverity` on each unreliability mode class.
Downstream projects may override these values for their specific context.
To query the default severity for a document's primary mode:

```sparql
SELECT ?doc ?mode ?severity WHERE {
  ?doc atlas:primaryUnreliabilityMode ?mode .
  ?mode atlas:defaultSeverity ?severity .
}
```
