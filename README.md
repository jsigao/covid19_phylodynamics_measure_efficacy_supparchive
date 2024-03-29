# Supplementary Archive for: Phylodynamic insights on the early spread of the COVID-19 pandemic and the efficacy of intervention measures
This archive contains the necessary materials to replicate the analyses and reproduce the results of our study.
These materials are located within four directories: the [`data`](#data) directory contains the genomic, geographic and sampling data used in our phylodynamic analyses, and also includes data on human-travel volume and intervention measures; the [`analyses`](#analyses) directory contains the XML scripts specifying the phylodynamic analyses we performed using the `BEAST` software package; the [`scripts`](#scripts) directory contains the `R` scripts that we used to curate data and to process the output from our analyses, and; the [`program`](#program) directory contain a modified version of the `BEAST` software package that implements various extensions required to perform some of the phylodynamic analyses in our study.
Each of these directories is detailed below.
Note that we also provide this archive as a [Dryad repository](https://datadryad.org/stash/share/qM_lKSgUqZ9jk3yKU5Fo5JurDb4iichKAnkt4PPh6Wg).

## <a name="data"></a>Data
The `data` directory is divided into three subdirectories, one for each primary type of data used in our study:
* SARS-CoV-2 genomic sequence data ([`data/sequence_data`](#sequence_data)),
* human-travel data ([`data/sequence_data`](#travel_data)), and
* intervention-measure data ([`data/measure_data`](#measure_data)).

### <a name="sequence_data"></a>SARS-CoV-2 Sequence Data
Our study involves analyses of two SARS-CoV-2 genomic datasets: the *reduced dataset*, with 1271 sequences, and the *entire dataset*, with 2598 sequences.
The reduced dataset is based on all SARS-CoV-2 genomic sequences&mdash;collected during the early phase of the COVID-19 pandemic&mdash;that were deposited in [GISAID](https://www.gisaid.org) as of April 19, 2020.
The entire dataset is based on all SARS-CoV-2 sequences collected during the early phase of the COVID-19 pandemic that were deposited in GISAID as of September 22, 2020.

GISAID policy prohibits us from directly sharing the `FASTA` files for the SARS-CoV-2 genomic sequences used in our study.
Instead, we provide the corresponding GISAID accession numbers of those SARS-CoV-2 sequences: sequences comprising the reduced dataset are listed in `data/sequence_data/gisaid_acknowledgement_table_041920_030820.tsv`, and sequences comprising the entire dataset are listed in `data/sequence_data/gisaid_acknowledgement_table_092220_030820.tsv`.
The metadata for each sequence&mdash;including the sampling date and geographic location&mdash;are listed in `data/sequence_data/metainfo_041920.csv` for the reduced dataset, and `data/sequence_data/metadata_092220.tsv` for the entire dataset.
The accession numbers of sequences that we excluded during the data-curation stage (because they are duplicates) are listed in `data/sequence_data/exclude_seqs.tsv` (modified from [Nextstrain ncov exclude list](https://github.com/nextstrain/ncov/blob/master/defaults/exclude.txt).

### <a name="travel_data"></a>Human-Travel Data
Our study also involved three types of data regarding human-travel volume:
* data on the volume of global air travel ([`data/travel_data/global_flights`](#global_flights_data)),
* data on the volume of domestic travel in China ([`data/travel_data/china_domestic`](#china_domestic_data)), and
* data on the volume of domestic travel in the United States ([`data/travel_data/us_domestic`](#us_domestic_data)).

#### <a name="global_flights_data"></a>Global air-travel volume
Data on the daily volume of global air travel&mdash;sourced from FlightAware&mdash;is located in `data/travel_data/global_flights/nflights_daily_byaircraft.csv`; these records include the number of all commercial passenger flights (with the number of flights for each type of aircraft) on each day between December 30, 2019 to March 8, 2020.
We summarize data on the passenger occupancy (number of seats) in each type of aircraft in  `data/travel_data/global_flights/aircraft_nseats.csv`.
We then estimated the daily air-travel volume (assuming maximum capacity for all flights) as follows: first, we compute the product of the number of flights for each type of aircraft times the passenger occupancy for that type of aircraft, then we sum over all aircraft types.

#### <a name="china_domestic_data"></a>Volume of domestic travel in China
Data on daily domestic travel within China&mdash;sourced from [the Baidu Migration platform](https://qianxi.baidu.com/2020) for location-based services&mdash;is located in `data/travel_data/china_domestic/baidu_mobility_index.csv`; these records include the domestic mobility index for each day between January 1 to March 8, 2020.

#### <a name="us_domestic_data"></a>Volume of domestic travel in the United States
We extracted data on domestic travel within the US from two publicly available datasets, [Apple COVID-19 mobility trends reports](https://covid19.apple.com/mobility) and [Google COVID-19 community mobility reports](https://www.google.com/covid19/mobility/).
The Apple location-based services provide data on three types of mobility indices (walking, driving, and transit), which are summarized in `data/travel_data/us_domestic/apple_mobility_indices.csv`.
These records are based on user direction requests in Apple Maps, where each daily mobility index is the relative number of direction requests made on that day compared to the corresponding baseline volume on January 13, 2020 (these indices are only made available by Apple from this date onward to help study mobility trends during the COVID‑19 pandemic).
Google location-based services provide data for six types of mobility indices (transit, retail and recreation, groceries and pharmacy, parks, workplaces, and residential), which are summarized in `data/travel_data/us_domestic/google_mobility_indices.csv`.
These mobility indices are relative values, where the mobility index for a given day is computed as the number of visits on that day divided by the median number of visits on that day measured from January 3 to February 6, 2020 (these indices are only made available by Google from February 6 onward onward to help study mobility trends during the COVID‑19 pandemic).

### <a name="measure_data"></a>Intervention Measures
We focussed on two types of intervention measures involving China during the early phase of COVID-19 pandemic: targeted-containment measures (*i.e.*, international bans on air-travel with China), and the domestic-mitigation measures within China.
The table `data/measure_data/international_airtravelban_withchina.csv` contains a collection of countries or territories that imposed international travel bans with China, as well as the associated initiation date and information source.
The table `data/measure_data/china_domestic.csv` contains a collection of provinces or cities in China that imposed domestic-mitigation measures, as well as the associated implementation period, type of intervention measure, and information source.

## <a name="analyses"></a> Phylodynamic Analyses
The `analyses` directory contains the XML scripts that we used to perform the phylodyamic analyses using the `BEAST` software package.
We include a subdirectory for each stage of analyses that we performed in our study:
* analyses to infer a dated phylogeny for the reduced SARS-CoV-2 dataset ([`analyses/step1_dated_phylogeny_inference`](#step1_dated_phylogeny_inference_analyses));
* analyses to evaluate the fit of candidate biogeographic models to the reduced SARS-CoV-2 dataset ([`analyses/step2_model_evaluation`](#step2_model_evaluation_analyses));
* analyses to infer the phylogeny, divergence times, and biogeographic history for the entire SARS-CoV-2 dataset  ([`analyses/step3_joint_analyses`](#step3_joint_analyses));
* ancillary analyses to estimate daily global viral dispersal rates for the entire SARS-CoV-2 dataset ([`analyses/ancillary1_daily_rate`](#ancillary1_daily_rate)); and
* ancillary analyses to assess the sensitivity of our results to incomplete and non-random sampling of SARS-CoV-2 sequences ([`analyses/ancillary2_reduced_dataset`](#ancillary2_reduced_dataset)).

We performed all analyses using the `BEAST` software package, although the specific version varied among analyses (see details below), with the `BEAGLE` library (compiled from [the `hmc-clock` branch, commit `dd36bf5`](https://github.com/beagle-dev/beagle-lib/tree/dd36bf5b8d88348c77a93eeeef0917d90df71a4f)) enabled to accelerate both CPU and GPU computation.
Note that the XML scripts that we incude in this repository include `?` as placeholders for each sequence (because GISAID policy prohibits us from directly sharing the sequence data). In order to reproduce our analyses, it is therefore necessary to: (1) first, download the sequences from GISAID using the GISAID accession numbers that we provide in the [`data/sequence_data`](#sequence_data) subdirectory; (2) then, run the `R` scripts that we provide in the [`scripts/sequence_data_curation`](#sequence_data_curation_scripts) subdirectory to recreate our curated sequence alignments, and; (3) finally, replace the `?`s with the corresponding sequences to replicate our XML scripts.
To improve clarity, the name of each sequence in the XML scripts comprises three parts, denoting the GISAID accession ID, sampling geographic area, and sampling date, respectively.
For example, the sequence listed under GISAID ID `EPI_ISL_402124` was sampled in China on December 30, 2019, so its name in the XML scripts is thus `402124_China_123019`.

### <a name="step1_dated_phylogeny_inference_analyses"></a>Step 1: Estimating the dated phylogeny for the reduced SARS-CoV-2 dataset
We inferred the dated phylogeny for the reduced SARS-CoV-2 dataset using the XML script `data/step1_dated_phylogeny_inference/coalExp_ucln_posterior_run1.xml`.
We summarized the tree inferred from these analyses as a Maximum Clade Credibility (MCC) summary tree, which is located in `data/step1_dated_phylogeny_inference/coalExp_ucln_posterior_MCC.tre`.
We performed these analyses using [`BEAST` version 1.10.5](https://github.com/beast-dev/beast-mcmc/releases/tag/v1.10.5pre1).

### <a name="step2_model_evaluation_analyses"></a>Step 2: Evaluating candidate geographic models using the reduced SARS-CoV-2 dataset
We explored the relative and absolute fit of the reduced SARS-CoV-2 dataset to 12 candidate biogeographic models corresponding to combinations of:
* symmetric and asymmetric Q matrices;
* default and alternative priors on the total number of dispersal routes;
* default and alternative priors on the average dispersal rate, and;
* models where the Q matrices and average dispersal rates are piecewise constant over 1, 2, or 4 pre-specified intervals.

We assessed the *relative fit* of these candidate biogeographic models to our reduced SARS-CoV-2 dataset by computing Bayes factors from the marginal likelihoods of competing models.
We estimated the marginal likelihood for each candidate biogeographic model by performing power-posterior analyses; these XML scrips are located in the subdirectory `analyses/step2_model_evaluation/marginal_likelihood`.
We also assessed the *absolute fit* of each candidate biogeographic model to our reduced SARS-CoV-2 dataset using posterior-predictive simulation.
We estimated the posterior probability distribution for each candidate biogeographic model by performing MCMC simulation; these XML scrips are located in the subdirectory `analyses/step2_model_evaluation/constant/posterior_predictive`.

Our evaluation of candidate biogeographic models conditioned on the MCC summary phylogeny inferred in [step 1](#step1_dated_phylogeny_inference_analyses).
Our analyses under the time-constant biogeographic models were performed using [`BEAST` version 1.10.5](https://github.com/beast-dev/beast-mcmc/releases/tag/v1.10.5pre1), whereas those under the piece-wise constant biogeographic models were performed using the modified version of `BEAST` (see the [program section](#program) for details).

### <a name="step3_joint_analyses"></a>Step 3: Jointly inferring the phylogeny, divergence times and geographic history for the entire SARS-CoV-2 dataset
We inferred the phylogeny, divergence times and geographic history for the entire SARS-CoV-2 dataset.
The phylodynamic model that we used for these analyses included the previously selected geographic (sub)model (see [step 2](#step2_model_evaluation_analyses)).
We performed these joint phylodynamic analyses using the modified version of `BEAST`; the corresponding XML files are located in `analyses/step3_joint_analyses`.
We provide the inferred posterior distribution of phylodynamic model parameters in our Dryad repository (file-size constraints prevent us from including it in the GitHub repository).

### <a name="ancillary1_daily_rate"></a>Ancillary analyses 1: Estimating daily global viral dispersal rates for the entire SARS-CoV-2 dataset
We estimated the daily global viral dispersal rate for the entire SARS-CoV-2 dataset under a more granular phylodynamic model that allows the average dispersal rate to vary from day to day (while keeping the other biogeographic model components identical to the joint inference) to assess the correlation between these estimates with the daily volume of global air travel during the early phase of the COVID-19 pandemic.
We performed these analyses using the modified version of `BEAST`; the corresponding XML scripts are located in `analyses/ancillary1_daily_rate`.
To accommodate phylogenetic uncertainty, we performed these analyses while averaging over (a subsample of) the marginal posterior distribution of dated phylogenies inferred previously in step 3 ([`joint analyses`](#step3_joint_analyses)); we include this tree file in our Dryad repository (file-size constraints prevent us from including it in the GitHub repository).

### <a name="ancillary2_reduced_dataset"></a>Ancillary analyses 2: Assessing the impact of incomplete viral sampling
We also performed additional series of analyses to assess the sensitivity of our results to incomplete and non-random sampling of SARS-CoV-2 sequences.
Specifically, we replicated the entire series of analyses that we performed on the entire SARS-CoV-2 dataset&mdash;including joint inference of phylogeny, divergence times and biogeographic history, as well as analyses to infer daily global viral dispersal rates&mdash;for the reduced SARS-CoV-2 dataset, which has fewer (1271 vs 2598) sequences and different temporal and spatial sampling intensities.
We performed these sensitivity analyses using the modified version of `BEAST`; the corresponding XML scripts are located in `analyses/ancillary2_reduced_dataset`.

## <a name="scripts"></a>Scripts
The `scripts` directory contains the `R` scripts that we used in this study to:
* curate SARS-CoV-2 genomic sequences sourced from GISAID to generate the sequence alignments, with the sampling dates and geographic locations of each sequence (see ([`scripts/sequence_data_curation`](#sequence_data_curation_scripts)),
* perform both stochastic-mapping and posterior-predictive simulations using the inferred geographic model parameters and dated phylogenies (see [`scripts/history_simulation`](#history_simulation_scripts)), and;
* process `BEAST` output files to generate summaries of various geographic-model parameters (see [`scripts/parameter_summary`](#parameter_summary_scripts)).

### <a name="sequence_data_curation_scripts"></a>Curating SARS-CoV-2 Sequence Data
Curation of the SARS-CoV-2 sequence data involved filtering the raw sequences downloaded from GISAID for various reasons (*e.g.*, because a sequence was incomplete and/or lacked the associated sampling dates or locality metadata).
Our curation procedure differed slightly for the reduced and entire SARS-CoV-2 datasets, as we downloaded unaligned sequences from GISAID to generate the reduced dataset, but downloaded aligned sequences to generate the entire dataset.
Specifically, processing of the reduced dataset (where we started from unaligned sequences), we first applied a preliminary filter using the script `scripts/sequence_data_curation/sequence_preprocess_beforealign.R`, and&mdash;after aligning the sequences&mdash;applied a secondary filter using the script `scripts/sequence_data_curation/sequence_process_aftealign.R`.
By contrast, we processed the entire dataset (where we started from aligned sequences) by applying a single filter using the script `scripts/sequence_data_curation/sequence_process_gisaidalignment.R`.
These scripts also trim the alignment after filtration to generate nucleotide alignments that only retain coding regions.

After the initial curation, we then defined data subsets within the curated alignments using the script `scripts/sequence_data_curation/sequence_process_alignment_foranalysis.R`. Defining subsets of the alignments&mdash;*e.g.*, subdividing sites in the alignment by codon position and/or by gene regions&mdash;allows us to specify independent substitution models for each subset of the alignment to capture variation in the substitution process across the SARS-CoV-2 genomes.
The script `scripts/sequence_data_curation/discretetrait_writer.R` generates a data table that contains the sampling date (in units of days) and sampling locality (according to the discrete geographic areas defined in `scripts/sequence_data_curation/main_config.R`) for each sequence in the alignment.
Finally, the script `scripts/sequence_data_curation/generate_xml_data_head.R` reads in the partitioned sequence alignment and the sampling-information table (*e.g.*, `data/sequence_data/metainfo_041920.csv`) to generate the `taxa` part of the XML scripts that are provided in the `analyses` directory.

### <a name="history_simulation_scripts"></a>Simulating Geographic Histories
We simulated dispersal histories over sampled phylogenies to: (1) infer the posterior distribution of geographic histories (using stochastic mapping), and; (2) assess the adequacy of geographic models (using posterior-predictive simulation).
We used the `scripts/history_simulation/history_simulator_functions.R` script to perform either stochastic mapping or posterior-predictive simulation (by setting the argument `conditional` to true [for stochastic mapping] or false [for posterior-precitive simulation]).
We then used the `scripts/history_simulation/posterior_predictive_teststatistics_functions` script to calculate posterior-predictive summary statistics (including the tipwise multinomial statistic and the parsimony statistic).
Other scripts in this subdirectory include subroutines that are executed by `history_simulator_functions.R` and are not intended to be used directly.

### <a name="parameter_summary_scripts"></a>Summarizing Geographic-Model Parameters
We used the script `scripts/parameter_summary/get_BFs_counts.R` to process the parameter log file output by `BEAST` (and simulated history log file, if available) to summarize the geographic-model parameters for both time-constant and piecewise-constant geographic models.
These summaries include (interval-specific) pairwise dispersal routes support, (interval-specific) global dispersal rates, and (interval-specific) numbers of pairwise dispersal events.
We used the script `scripts/parameter_summary/get_daily_events_nlineages.R` to process a simulated-history log file output by `BEAST` (or the tree log file) to summarize the number of dispersal events that occurred over each dispersal route (or the number of active viral lineages) in each arbitrarily specified time interval.

## <a name="program"></a>Modified BEAST Program
We provide an executable for a modified version of the `BEAST` program; our modifications implement various extensions to the piecewise-constant phylodynamic models.
Our modifications were made to the source code of [the `master` branch, commit `c0e5f8d3a`](https://github.com/beast-dev/beast-mcmc/commit/c0e5f8d3af1f943bed5754d8e6466aab4eb776f4); the modified source code (from which we compiled the executable used in several of our analyses) is located in [the `epochal_model_extension` branch, commit `05faf8706`](https://github.com/jsigao/beast-mcmc/commit/05faf870689144f867f9737545004308cfa1c737) of a forked `BEAST` source-code repository.
Briefly, this executable can be run by invoking:
```
java -jar beast.jar -working ./analyses/parameter_estimation/interval_specific/092220_030820/4interval_exphyper_asymmetric_poissonintermediate_orf1restcp_posterior_run1.xml
```
See [`BEAST` official tutorial website](http://beast.community/index.html) for further details about running `BEAST` analyses using a `Java` executable.
