# LocARNA consensus constraint generator

Utility R script based on tidyverse packages.

Reads a LocARNA input fasta file with structure constraints (#S)
and a corresponding ClustalW alignment LocARNA output file
and maps the constraints to the alignment positions to
generate a consensus constraint to be used with RNAalifold.

NOTE: requires a structure constraint FOR EACH aligned sequence!
