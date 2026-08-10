---
'@platforma-open/milaboratories.redefine-clonotypes.workflow': patch
---

MILAB-6736: recompute clonotype-level abundance aggregates when clonotypes are merged

Property columns marked `pl7.app/isAbundance` are themselves aggregates over samples — Supporting Reads, Supporting UMIs, Supporting Cells, Number of Samples, Mean Fraction of Reads, Mean Fraction of UMIs. They were carried over from the most abundant source clonotype, so a merged clonotype reported a single member's value while its per-sample abundances were correctly summed, and the two contradicted each other in the same table row.

These columns are now recomputed from the block's own redefined abundances: counts are summed over samples, normalized values averaged over the samples where the clonotype is present, and `Number of Samples` taken as the number of distinct samples. Dispatch is driven by `pl7.app/abundance/unit` and `pl7.app/abundance/normalized`, so reads, molecules and cells are all covered without naming individual columns. Property columns that are not abundances keep the representative-value semantics.
