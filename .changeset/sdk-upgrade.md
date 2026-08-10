---
'@platforma-open/milaboratories.redefine-clonotypes.workflow': patch
'@platforma-open/milaboratories.redefine-clonotypes.model': patch
'@platforma-open/milaboratories.redefine-clonotypes.ui': patch
'@platforma-open/milaboratories.redefine-clonotypes': patch
---

Upgrade the Platforma SDK

workflow-tengo 6.6.1 → 6.8.2, model 1.79.6 → 1.80.13, ui-vue 1.79.6 → 1.80.15, block-tools 2.10.19 → 2.12.11, package-builder 3.13.0 → 3.14.2, tengo-builder 4.0.8 → 4.0.21, test 1.79.10 → 1.80.16, ts-builder 1.5.2 → 1.6.1, ts-configs 1.2.3 → 1.3.1.

workflow-tengo 6.8.2 brings ptabler 2.1.8, whose PFrame reader streams a parquet row group once rather than re-slicing it per batch.
