# LocARNA consensus constraint generator

Utility R script based on tidyverse packages.

Reads a LocARNA input fasta file with structure constraints (#S)
and a corresponding ClustalW alignment LocARNA output file
and maps the constraints to the alignment positions to
generate a consensus constraint to be used with RNAalifold.

NOTE: requires a structure constraint FOR EACH aligned sequence!

## Parameters

```sh
Options:
        -h, --help
                Show this help message and exit

        -a CLUSTALW.FILE, --alignment=CLUSTALW.FILE
                LocARNA Clustal-w alignment output file to map the constaints to

        -c FASTA.FILE, --constraint=FASTA.FILE
                LocARNA FASTA input file with '#S' structure constraints used to generate the alignment
```

## Example

Using the provided 

- LocARNA input FASTA file with structure constraints [input-constraints.fa](input-constraints.fa) and
- the corresponding LocARNA CLUSTAL-W alignment output file [result.aln](result.aln)

we can all the script like this

```sh
Rscript --vanilla consensus-constraint.R -a result.aln -c input-constraints.fa
```

and get the following output of a consensus constraint

```sh
............<<<<<<<..<<<<<<<<<<<<<.<<<<<<<<<<<...........<<<<<<<<<<<<<<.<<<<<(((((<<<<<<<.....<<<<...........................<<<<<<...........................>>>>>>>>>>..>>>>>.....>>>>)))))..>>>>...>>>>>>>>>>>>>>>>>>>>.>>.>>>>>>>>>>>.......>>>.......>>>>>>>.>>>>>>..............................................................
```

that we can use to call RNAalifold to get a [constrained foldign of the alignment](https://www.tbi.univie.ac.at/RNA/ViennaRNA/refman/man/RNAalifold.html#structure-constraints).

```sh
# store constraint in temporary file
Rscript --vanilla consensus-constraint.R -a result.aln -c input-constraints.fa > result.con
# call fold alignment with constraint file
RNAalifold --aln --color --ribosum_scoring --cfactor 0.6 --nfactor 0.5 --mis -t 0 --constraint=result.con result.aln
```
