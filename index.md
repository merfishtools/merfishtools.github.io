[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat-square)](http://bioconda.github.io/recipes/merfishtools/README.html)
[![Travis](https://img.shields.io/travis/merfishtools/merfishtools.svg?style=flat-square)](https://travis-ci.org/merfishtools/merfishtools)

MERFISHtools implement a Bayesian framework for accurately predicting gene or transcript expression from MERFISH data.
On top, differential expression analysis can be performed for two or multiple conditions, including credible intervals for fold change and coefficient of variation, and controlling the expected false discovery rate.

## Citation

If you use MERFISHtools, please cite our preprint

> Köster, Johannes; Brown, Myles; Liu, X. Shirley (2016): A Bayesian model for single cell transcript expression analysis on MERFISH data. figshare. https://dx.doi.org/10.6084/m9.figshare.3438662.v1

## Installation

MERFISHtools can be installed and updated easily via [Bioconda](http://bioconda.github.io/recipes/merfishtools/README.html).

## Usage

For usage instructions at the command line, please issue

    merfishtools --help

A typical MERFISHtools workflow is as follows.

### Step 1: Estimate transcript expressions
Transcript expressions are estimated from raw MERFISH data via

    merfishtools exp codebook.txt --estimate estimates.txt < data.txt > expression.txt

#### Inputs

The file `codebook.txt` is a MERFISH codebook, consisting of tab separated columns: 

* feature,
* codeword,
* expressed. 

The last column denotes if a codeword is assigned to e.g. a gene for which expression can be expected. Unless you have misidentification probes (see Chen et al. Science 2015), you will have only ones in this column.

The file `data.txt` contains MERFISH readouts in tab-separated format. The expected columns are

* cell,
* feature,
* hamming_dist,
* cell_position_x,
* cell_position_y,
* rna_position_x,
* rna_position_y.

#### Outputs

Results are provided as probability mass functions at STDOUT, in the format

* cell,
* feature (e.g. gene, transcript),
* expression,
* posterior probability.

Further, the optional flag `--estimate estimates.txt` results in a table with expression estimates of the form:

* cell,
* feature,
* expected value,
* standard deviation,
* maximum a-posteriori probability estimate (MAP),
* lower bound of 95% credible interval,
* upper bound of 95% credible interval.

### Step 2:







## Author
[Johannes Köster](https://johanneskoester.bitbucket.org)

## License

[MIT](https://github.com/merfishtools/merfishtools/blob/master/LICENSE.md)
