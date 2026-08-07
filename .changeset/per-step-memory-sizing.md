---
'@platforma-open/milaboratories.redefine-clonotypes.workflow': patch
---

Restore per-step memory and CPU sizing

Every step of the workflow shared one memory and one CPU value, from FASTA assembly through to the main pt workflow. On large single-cell datasets the property export and the main pt workflow both ran out of memory, while smaller steps reserved the large steps' allocation for their whole duration.

This restores the per-step sizing the block previously had — property export at 16 GiB / 1 core, imports at 8 GiB / 1 core, ANARCI at 16 GiB / 4 cores, numbering at 8 GiB / 2 cores — before those values were replaced by a single setting propagated to every component.

The collapse mattered because ptabler runs polars with `max(2, cpu)` threads and each thread holds decode buffers for a whole parquet row group, so the CPU request multiplies peak memory rather than only buying throughput. When the shared CPU value was later raised from 1 to 8 to speed up large datasets, the property export inherited 8 threads and exceeded the limit its own earlier fix had set.

The main pt workflow now gets 32 GiB, and the property export keeps headroom above its original 16 GiB because its cost scales with the number of per-clonotype columns contributed by upstream blocks.

Steps that declare their own sizes — numbering, ANARCI, deanonymization — are no longer overridden. An explicit Memory / CPU override in the block settings still applies to every step, unchanged.
