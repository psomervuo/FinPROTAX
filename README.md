# FinPROTAX

Taxonomy, reference sequences, and models needed to run probabilistic taxonomic classifier PROTAX for Finnish insects are in directories modelCOIfull and modelCOIleray. Three programs are needed to run the classification and they should be located in directory 'progs':

1) Binary 'hmmalign' from HMMER package that can be downloaded from hmmer.org
2) Perl script 'removeinserts.pl' that is already included
3) Binary 'classify_mxcode'. The existing binary in the directory is compiled for Linux. Source code can be obtained from github psomervuo/protaxA in case there is need to run the software in other OS

In order to run the software and get the probabilistic classifications for user-defined input sequences, three items need to be defined:

1) Input file: COI sequences to be classified in FASTA format (input.fa)
2) Probability threshold (e.g. 0.1). For each input sequence, those taxa are reported in the output whose probability exceeds the threshold.
3) Model: modelCOIfull or modelCOIleray. COIfull is for full-length (658 bp) COI region and COIleray is the 313bp-region defined by Leray primers.

NOTE: In general, full-length model should be used unless user knows what he/she is doing.

Below gives an example where $M specifies path to the selected model, $P is path to directory progs, and $TH is the probability threshold defined by user.
First input sequences need to get aligned corresponding to the reference sequences. This is done using hmmalign. After that the insertions are removed and the remaining parts of the alignment are used to calculate p-distances against reference sequences and by means of PROTAX, the distances are converted into taxon membership probabilities.

```
1) ${P}/hmmalign --outformat A2M -o tmp.a2m ${M}/refs.hmm input.fa
2) perl ${P}/removeinserts.pl tmp.a2m > tmp_aligned.fa
3) ${P}/classify_mxcode ${M}/taxonomy.priors ${M}/refs.aln ${M}/model.rseqs.numeric ${M}/model.pars ${M}/model.scs $TH tmp_aligned.fa > output
```

Temporary files 'tmp.a2m' and 'tmp_aligned.fa' can be removed after classification.
In the output file each input sequence is spread into multiple lines, columns of each line being:

1) Seqid
2) Taxonomy Level
3) Taxon Code
4) Taxon Name
5) Probability

---

This project has received funding from the European Research Council (ERC) under the European Union’s Horizon 2020 research and innovation programme (grant agreement No 856506).
