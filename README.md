# party

The domain hub for parties — organizations and people — in the Semvia demo
quiver.

It ships no data, no mappings, and no connection descriptor. What it ships is a
small vocabulary that other packages align **into**, so that independently
authored packages answer one another's queries without either one naming the
other's IRIs.

## Why a hub

Align five packages to each other pairwise and you author ten bridges; a sixth
package costs five more. Align each to a hub and you author five, and the sixth
costs one. The arithmetic is the smaller half of the argument — the larger half
is coupling. A pairwise bridge pins two packages to each other's IRIs, so
neither can re-version without breaking the other. A hub alignment pins each
package to one vocabulary that is deliberately small and deliberately stable.

## The identifier does most of the work

Two packages that both carry the LEI stop needing a bridge at all once both
align their LEI predicate to `party:lei`. `owl:equivalentProperty` is a
production-default lattice edge in Semvia and is walked in both directions, so
one pattern on `party:lei` reaches both datasets — joined on a value both
sources already publish, not on a mapping table somebody maintains.

That is the test for admitting an identifier here: it must be issued by a
standards body, not by one of the sources. ISO 17442 qualifies. A vendor's
internal customer id does not.

## What it deliberately omits

No addresses, no jurisdictions, no relationships, no roles, and no SHACL
constraints on hub classes. Sources differ enough on all of these that a hub
term would be either too loose to say anything or too tight to be true of one
side. A hub class is reached by alignment from data this package does not
control and cannot validate, so constraining it would turn another package's
ordinary data into violations here.

Terms are added when a **second** package needs them — never in anticipation.

## Install

```json
{
  "requires": {
    "https://github.com/SemviaIO/party": "SemviaIO/party#v0.1.0"
  }
}
```

## Aligning a package to it

Keep the alignment in its own file, so a consumer can install your package
without it. Point your top-level class at the hub with the edge that is actually
true — usually `rdfs:subClassOf`, not `owl:equivalentClass`, since a source's
top class is rarely coextensive with the hub's:

```turtle
@prefix party: <https://github.com/SemviaIO/party/ontology#> .

yours:LegalEntity   rdfs:subClassOf       party:Organization .
yours:lei           owl:equivalentProperty party:lei .
yours:registeredName rdfs:subPropertyOf   party:name .
```

Declare the hub terms you name minimally — a bare `rdfs:Class` or
`rdf:Property`, no label, no comment. When the hub is installed it supplies
those itself, and a second copy asserted from your package would duplicate
annotations on a term you do not own.
