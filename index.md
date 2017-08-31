[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat-square)](http://bioconda.github.io/recipes/merfishtools/README.html)
[![Travis](https://img.shields.io/travis/merfishtools/merfishtools.svg?style=flat-square)](https://travis-ci.org/merfishtools/merfishtools)
[![GitHub issues](https://img.shields.io/github/issues/merfishtools/merfishtools.svg?style=flat-square)](https://github.com/merfishtools/merfishtools/issues)
[![](https://img.shields.io/github/issues-pr-raw/merfishtools/merfishtools.svg?style=flat-square)](https://github.com/merfishtools/merfishtools/pulls)
[![license](https://img.shields.io/github/license/merfishtools/merfishtools.svg?style=flat-square)](https://github.com/merfishtools/merfishtools/blob/master/LICENSE.md)


MERFISHtools implement a Bayesian framework for accurately predicting gene or transcript expression from MERFISH data.
On top, differential expression analysis can be performed for two or multiple conditions, including credible intervals for fold change and coefficient of variation, and controlling the expected false discovery rate.

## Citation

If you use MERFISHtools, please cite our preprint

*Köster, Johannes; Brown, Myles; Liu, X. Shirley (2016): A Bayesian model for single cell transcript expression analysis on MERFISH data. figshare. https://dx.doi.org/10.6084/m9.figshare.3438662.v5*

## Installation

MERFISHtools can be installed and updated easily via [Bioconda](http://bioconda.github.io/recipes/merfishtools/README.html).

## Usage

For usage instructions at the command line, please issue

    merfishtools --help

A typical MERFISHtools workflow is as follows.

### Step 1: Estimate transcript expressions
Transcript expressions are estimated from raw MERFISH data via

    merfishtools exp --threads 8 codebook.txt --estimate estimates.txt < data.txt > expression.txt
    
See

    merfishtools exp --help

for additional parameters.

#### Input

The file `codebook.txt` is a MERFISH codebook ([example](https://github.com/merfishtools/merfishtools-evaluation/raw/master/codebook/140genesData.1.txt)), consisting of tab separated columns: 

* feature,
* codeword. 

Note that the codebook should not list misidentification probes (see Chen et al. Science 2015).

The file `data.txt` ([example](https://github.com/merfishtools/merfishtools-evaluation/raw/master/data/140genesData.1.all.txt)) contains MERFISH readouts in tab-separated format. The expected columns are

* cell,
* feature,
* hamming_dist,
* cell_position_x,
* cell_position_y,
* rna_position_x,
* rna_position_y.

#### Output

Results are provided as probability mass functions (PMF) at STDOUT, in the format

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

### Step 2: Estimate differential expression

#### Two conditions

In case of two conditions, you can issue

    merfishtools diffexp --threads 8 expression1.txt expression2.txt > diffexp.txt
 
to calculate differentially expressed transcripts.
See

    merfishtools diffexp --help

for additional parameters.

##### Input

The files `expression1.txt` and `expression2.txt` ([example](https://github.com/merfishtools/merfishtools-evaluation/raw/master/expressions/140genesData.1.all.default.txt)) contain the PMFs of the two conditions to compare, and are obtained by running step 1 on the data for each condition.

##### Output

Results are provided as tab separated table at STDOUT (here piped into the file `diffexp.txt`) with columns

* feature (e.g. gene, transcript),
* posterior error probability (PEP) for differential expression,
* expected FDR when selecting all features down to the current,
* bayes factor (BF) for differential expression,
* expected log2 fold change of first vs second group,
* standard deviation of log2 fold change,
* maximum a posteriori (MAP) log2 fold change,
* lower bound of 95% credible interval of log2 fold change,
* upper bound of 95% credible interval of log2 fold change.

#### Multiple conditions

In case of more than two conditions, you can issue

    merfishtools multidiffexp --threads 8 expression1.txt expression2.txt expression3.txt ... > diffexp.txt
 
to calculate differentially expressed transcripts. Here, the coefficient of variation over the condition means is used as measure for differential expression. See

    merfishtools multidiffexp --help

for additional parameters.

##### Input

The files `expression1.txt` and `expression2.txt`, ... ([example](https://github.com/merfishtools/merfishtools-evaluation/raw/master/expressions/140genesData.1.all.default.txt)) contain the PMFs of the conditions to compare, and are obtained by running step 1 on the data for each condition.

##### Output

Results are provided as tab separated table at STDOUT (here piped into the file `diffexp.txt`) with columns

* feature (e.g. gene, transcript),
* posterior error probability (PEP) for differential expression,
* expected FDR when selecting all features down to the current,
* bayes factor (BF) for differential expression,
* expected coefficient of variation (CV),
* standard deviation of CV,
* maximum a posteriori (MAP) CV,
* lower bound of 95% credible interval of CV,
* upper bound of 95% credible interval of CV.



## Author
[Johannes Köster](https://johanneskoester.bitbucket.org)

## License

[MIT](https://github.com/merfishtools/merfishtools/blob/master/LICENSE.md)
