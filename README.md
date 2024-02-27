# LocARNA consensus constraint generator

Utility R script based on tidyverse packages.

Reads a LocARNA input fasta file with structure constraints (#S)
and a corresponding ClustalW alignment LocARNA output file
and maps the constraints to the alignment positions to
generate a consensus constraint to be used with RNAalifold.

NOTE: requires a NESTED structure constraint FOR EACH aligned sequence!

## Parameters

```sh
Options:
        -h, --help
                Show this help message and exit

        -a CLUSTALW.FILE, --alignment=CLUSTALW.FILE
                LocARNA Clustal-w alignment output file to map the constaints to

        -c FASTA.FILE, --constraint=FASTA.FILE
                LocARNA FASTA input file with structure constraints used to generate the alignment

        -t S|FS, --type=S|FS
                LocARNA structure constraint type to be used: (S)tructure constraint or (FS) = fixed structure constraint

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

that we can use to call RNAalifold to get a [constrained folding of the alignment](https://www.tbi.univie.ac.at/RNA/ViennaRNA/refman/man/RNAalifold.html#structure-constraints).

```sh
# pipe constraint via STDIN into RNAalifold
Rscript --vanilla consensus-constraint.R -a result.aln -c input-constraints.fa | \
RNAalifold --aln --color --ribosum_scoring --cfactor 0.6 --nfactor 0.5 --mis -t 0 -C --enforceConstraint --noLP result.aln
```

## Dependencies

Requires 

- R
- `tidyverse` packages  (run in R `install.packages("tidyverse")`)
- `optparse` package

If you are using conda and you dont have R installed, you can do so via conda

```sh
conda install conda-forge::r-tidyverse conda-forge::r-optparse
```

which should install all needed dependencies.


