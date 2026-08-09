---
'@platforma-open/milaboratories.redefine-clonotypes.workflow': patch
---

Restore per-step memory and CPU sizing

Every step of the workflow shared one memory and one CPU value, from FASTA assembly through to the main pt workflow. On large single-cell datasets the property export and the main pt workflow both ran out of memory, while smaller steps reserved the large steps' allocation for their whole duration.

This restores the per-step sizing the block previously had — property export at 16 GiB / 1 core, imports at 8 GiB / 1 core, ANARCI at 16 GiB / 4 cores, numbering at 8 GiB / 2 cores.

Adding a single Memory / CPU setting and propagating it to every component changed no value by itself: every step was already at 16 GiB / 1 core, and the imports gained memory. What broke it was raising that shared CPU from 1 to 8 to speed up large datasets. ptabler runs polars with `max(2, cpu)` threads and each thread holds decode buffers for a whole parquet row group, so for a step that reads a p-frame the CPU request multiplies peak memory rather than only buying throughput. The property export inherited 8 threads and exceeded the limit its own earlier fix had set.

The main pt workflow now gets 32 GiB and keeps its 8 cores — it reads TSVs rather than a p-frame, so it never decodes a parquet row group and the thread multiplier does not apply to it. Only the p-frame readers need their thread count held down. The property export keeps headroom above its original 16 GiB because its cost scales with the number of per-clonotype columns contributed by upstream blocks.

ANARCI and deanonymization go back to the sizes they declare for themselves — 16 GiB / 4 cores and 8 GiB / 2 cores. ANARCI's 4 is the value its `--ncpu` argument was chosen to match; the 8 cores it inherited from the shared setting were collateral from a change aimed at polars, which ANARCI does not use. Numbering with ANARCI will therefore run on 4 cores rather than 8. The numbering TSV builders fall back to the SDK's data-derived sizing formula.

An explicit Memory / CPU override in the block settings still applies to every step, unchanged.
