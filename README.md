# Centroid-Boost
Codebase for using user query variations (UQVs) to improve efficiency and effectiveness of online rank fusion.

These algorithms were presented in the following paper. Please cite it if you
extend our work:

Rodger Benham, Joel Mackenzie, Alistair Moffat, and J. Shane Culpepper:
**Boosting Search Performance Using Query Variations.**
In ACM Transactions on Information Systems (TOIS), October, 2019.


```
@article{bmmc19-tois,
  author = {R. Benham and J. Mackenzie and A. Moffat and J. S. Culpepper},
  title = {Boosting Search Performance Using Query Variations},
  journal = "ACM Trans. on Information Systems",
  year = {2019},
  volume = {37},
  number = {4},
  pages = {41.1--41.25},
}

```

## Query format
Assume, for the sake of this example, that we have the three following UQVs
for a single topic, with a topic ID of 1:
 - *raspberry pi*
 - *raspberry pi price*
 - *raspberry pi cost*

The input query format will then be:
```
1:raspberry pi
1:raspberry pi price
1:raspberry pi cost
```

For our experiments, we use the [UQV100](https://dl.acm.org/doi/10.1145/2911451.2914671)
collection. The resources can be found [here](https://melbourne.figshare.com/articles/UQV100_An_IR_Test_Collection_With_Query_Variability/3180694).

## Codebase
The original codebase was based on the variable block-max wand code, which can
be found [here](https://github.com/rossanoventurini/Variable-BMW/). However,
as this codebase is not maintained, we have ported our implementations to
the successor library, known as [PISA](https://github.com/pisa-engine/pisa).

The modifications to PISA are hosted on a [fork](https://github.com/JMMackenzie/pisa-multi-query)
which is included here as a submodule.


## Added Algorithms

### Single-pass CombSUM (SP-CS)
The idea behind SP-CS processing is to treat each multi-query as one large
weighted query, where the weight of each term corresponds to how many query
variations the term was present in. Take our example query above - the
SP-CS query can be thought of as *3 raspberry 3 pi 1 price 1 cost*, where each 
term  is preceded by its weight.

- Only works with additive scoring regimes, and will output CombSUM fusion
results by default. Can be extended to any other additive fusion method with
little effort.

Binaries: `evaluate_single_pass_combsum` for TREC outputs, 
`single_pass_combsum` for efficiency experiments.


### Parallel Fusion (PF)
Parallel fusion simply spins off a thread for every query, and fuse the resultant
heaps via CombSUM (or any other fusion method -- we implement CombSUM here).

Binaries: `evaluate_parallel_combsum` for TREC outputs, `parallel_combsum` for
efficiency experiments.

