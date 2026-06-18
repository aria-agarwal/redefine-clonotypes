---
'@platforma-open/milaboratories.redefine-clonotypes.model': patch
'@platforma-open/milaboratories.redefine-clonotypes.ui': patch
'@platforma-open/milaboratories.redefine-clonotypes.workflow': patch
'@platforma-open/milaboratories.redefine-clonotypes.anarci-numbering': patch
'@platforma-open/milaboratories.redefine-clonotypes.assembling-fasta': patch
---

Migrate block onto the structurer (block-tools structure) and upgrade the SDK toolchain: workflow-tengo 6.6.1, tengo-builder 4.0.8, model/ui-vue 1.79.6, test 1.79.10. Replaces hand-maintained config (eslint) with the tool-managed layout (oxlint/oxfmt, ts-builder check). No functional changes.
