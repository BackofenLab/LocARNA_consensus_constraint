# LocARNA consensus constraint generator

Utility R script based on tidyverse packages.

Reads a LocARNA input FASTA file with structure (#S) or fixed
structure (#FS) constraints and a corresponding ClustalW alignment
file produced by LocARNA using the given constraints.
Individual sequence constraints are mapped to respective alignment positions
to generate a consensus constraint that can be used with RNAalifold
to predict a constraint consensus RNA secondary structure of the alignment.

NOTE: requires a NESTED structure constraint FOR EACH aligned sequence!

Supported constraint encodings are `()<>.x`, see LocARNA documentation.
All other encodings are silently ignored.

## Parameters

```
Options:
        -h, --help
                Show this help message and exit

        -a CLUSTALW.FILE, --alignment=CLUSTALW.FILE
                LocARNA Clustal-w alignment output file to map the constaints to

        -c FASTA.FILE, --constraint=FASTA.FILE
                LocARNA FASTA input file with structure constraints used to generate the alignment

        -t S|FS, --type=S|FS
                LocARNA structure constraint type to be used: (S)tructure constraint or (FS) = fixed structure constraint.Default: 'S'

        -m (>0), --min=(>0)
                Minimal number of similar constraints per position to be considered in consensus.Default: 2

        -f [0.6..1], --fraction=[0.6..1]
                Minimal fraction of similar constraints per position to be considered in consensus.Default: 0.7
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
.................<...<<<<.<<<<<<...<..<<<<<.................((.<(((<<(<......(((((<<<<................................................................................................>>)))))....>....>)>>)))>)).>>.............>>>>>>.>>.......>>....................................................................................
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


