**Background:**
This block was originally created to group original clonotypes based on a new definition such as V gene, J gene, or CDR3 region, and recalculate abundances. 
It also originally let the user reannotate sequences based on a specific numbering scheme (IMGT, Kabat, or Chothia) to standardize sequence values for a
dataset that already had these region columns. 

**Goal:**
The gaps the edited block aims to address are that users could not input and annotate full length sequences, as the block originally expected those
feature columns (CDR/FWR) to already exist. The new block enables users to input full length sequences, select 1 or more clonotype definitions, get 
annotated sequences back. 

**Key Changes Made:**
1. Edited index.ts file, which defines the blocks UI outputs, such as which options appear in the dropdown and wether the numbering scheme dropdown
   appears. Made changes to this file to add a pairedRole check so the numbering scheme selector appears when full length sequences are imported,
   for example from the custom import formats in the edited import VDJ block.
2. The next file, main.tpl.tengo collects upstream columns into named bundles that get passed into process.tpl.tengo. Changes included adding domain
   based bundle queries so full length paired sequences are recognized.
3. The next key file is process.tpl.tengo, which contains logic to detect chains, run ANARCI, redefine clonotypes, and build the output pframe. The
   changes made in this file include fixing chain detection for paired full length sequences and added them as an allowed feature, and adding numbered
   columns to the output spec and aggregation so they are retained for the output pframe.
4. The next file, numbering-prep.tpl.tengo builds the input TSV and exports to the ANARCI template. The main change in this file was changing the
   bulk chain logic so when both H (heavy chain) and light (KL) chains exist in the same row (paired sequences), both chains are flagged for numbering
   instead of just the heavy chain.
5. The last key file anarci-numbering-tpl.tengo runs ANARCI and numbering python script on the input sequences, and extended the single cell
   branch to correctly trigger both the heavy and light chain to be numbered. 
