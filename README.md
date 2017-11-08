<p align="center">
  <img src="https://github.com/danielhomola/CorrMapper_pubmic/blob/master/frontend/static/img/logo_medium.png?raw=true" alt="CorrMapper"/>
</p>

# CorrMapper

CorrMapper is an online research tool for the integration and visualisation of
complex biomedical and omics datasets.

It allows users to:
- map clinical metadata onto the omics datasets using an automatically generated
 dashboard interface,
- perform feature selection on the omics datasets using one of the clinical
metadata variables,
- infer robust correlations between the selected features of one or two omics
datasets,
- visualise and analyse the networks of these correlations using highly
interactive modules.

CorrMapper is a data exploration and hypothesis generation tool. It does not try
 to automate statistical inference, or provide predicitve models. It is simply
 making the simultaneous exploration of omics datasets and clinical metadata
 easier by reducing the number of predictors to the clinically relevant ones,
 and by providing novel and interactive visualisation modules.

Very importantly, if you would like to use the features that were selected by
CorrMapper for modelling, you must ensure that the model is built and validated
on new data, that was not included in the feature selection. Otherwise the
generalisation error of your model will be underestimated. See Chapter 7.10.2
 _The Wrong and Right Way to Do Cross-validation_ in The Elements of Statistical
  Learning on page 245.

## Folder structure and code overview

The documentation of all modules and functions of CorrMapper live here[!!! ref]
Here's a rough overview of the organisation of the code in this project. There
are two main bodies of code: frontend and backend.

### Frontend

Frontend holds all the website (HTML, CSS, JS) and Flask app that handles the
views, the database models, the forms and their validation scripts. This part of
the application relies very heavily on [ScienceFlask](https://github.com/danielhomola/science_flask). Please make sure
to read its README and deployment notes. The forms models and views are largely
similar to ScienceFlask, but obivously they have CorrMapper specific components:
- more detailed forms and form checking logic
- a much richer data model for the study/analysis tables
- extra views and extended views that are specific to CorrMapper as a research
tool.

In the following part we quickly go through the frontend folder structure, and
point out the obvious differences between CorrMapper and ScienceFlask.
- __dashboard__: this is the main difference from ScienceFlask's folder
structure. It holds Python scripts for the automatic and programmatic generation
of CorrMapper's interactive metadata explorer.
  - _dashboard.py_: This holds the main functions for transforming the user's
  metadata files, calculating the PCA scores of the scatter-plots, saving and
  loading the dashboard object.
  - _write_dashboard_js.py_: This holds one monumental function (which badly
  needs to be refactored). It generates the 600-700 lines of JavaScript, and
  dc.js code that is required for driving the dashboard based on the user's
  metadata file.
- __static__: holds all the CSS, JavaScript, JPG and font assets of the project.
These are all very similar to what you'll find in ScienceFlask.
  - __demo__: this folder contains the JavaScript files of the three demo
  projects displayed on the opening page of corrmapper.com
- __templates__: These are th HTML templates that Flask uses with the Jinja2
templating engine to build the various views of the website. These are mostly
similar to ScienceFlask's _templates_ folder, except the following:
  - _dashboard.html_: this is the template for the metadata explorer.
  - _demo_dashboard.html_: template for one of the three demo apps.
  - _demo_genomic.html_: template for one of the three demo apps.
  - _demo_network.html_: template for one of the three demo apps.
  - _vis.html_: this is the template for __general__ network explorer of
  CorrMapper.
  - _vis_genomic.html_ this is the tempalte for the __genomic__ network explorer
  of CorrMapper.

### Backend
The backend folder is where CorrMapper's core scientific algorithms and pipeline
live. The whole point of the ScienceFlask project is to make the lives of other
 researchers easier, by allowing them to wrap their scientific tool within the
 template of ScienceFlask. Therefore, the folders and Python scripts within the
 __backend__ folder are all specific to CorrMapper. The majority of these
 functions are well documented, therefroe make sure to check out the docs of
 CorrMapper [here](docsr)ref[!!!].

- __bins__: This folder contains scripts for the binning of genomic datasets
according to the chromosomal map of the studied species.
  - _binning.py_: stores functions for binning a set of genomic features using
  one of the species specific chromosomal maps and the annotation files of the
  genomic features (as provided/uploaded by the user).
  - _get_chromosomes_from_UCSC_: holds a function to download chromosomal map
  information for the most common model organisms.
  - _chromosome_files_: holds the length of each chromosome for a given species
  - _bin_files_: contains the genome of a given species split into 300
  equidistant bins
- __corr__: This folder has modules and functions for the estimation of sparse
conditional independence networks, Spearman correlations and their permuted
p-values. Furthermore it also has funcitons for finding modules within unipartite
and bipartite networks and saving this network information for JavaScript to be
used by CorrMapper's frontend.
  - __gpd__: Holds the algorithm for the precise estimation of permuted p-values through the use of extreme  value approximation with Generalised Pareto
  Distribution.
  - _bivar_modules.R_: Implementation of the bivariate module finding algorithm
  through label propagation, by Steven Beckett.
  - _corr.py_: Main function for the calculation of conditional independence
  networks, Spearman correlations with permuted p-values and finding modules.
  - _hugeR.py_: Python wrapper function are the `huge` R package. This is used For
  the nonparanormal extension of the Graphical Lasso algorithm and the StARS
  network selection/regularisation method.
  - _network.py_: Various functions for the creation, and plotting of unipartite
   and bipartite network objects using `networkX`.
  - _pairplots.py_: Functions for the generation of informative scatter-plots
   in the general and genomic network explorer interfaces of CorrMapper.
  - _permutation.py_: Contains functions for the efficient (vectorised) and
  parallized calculation of Spearman p-values through permutation testing and
  the correction of these using the GPD methid.
  - _utils.py_: Numerous helper funcitons for the ordering, manipulation
  - _write_js_vars.py_:
- __fs__:
  - _fs.py_:
  - _lsvc_cv.py_:
  - _mi.py_:
  - _mifs.py_:
- __genomic__:
  - _genomic.py_:
  - _write_network.py_:
  - _write_tables.py_:
- __util__:
  - _check_uplaoded_files_:
  - _io_params_:



________________________________________________

## Additional folders

### failedAnalyses
This simply collects analyses that failed during the execution of CorrMapper's
data integration pipeline. This is mainly for the admin of the app and for
debugging purposes.

### logs
This is where CorrMapper and celery (the distributed task queue used by
CorrMapper for scheduling the jobs of users) will write their log files.

### supp
Contains code for reproducing the benchmarking experiments of the upcoming
CorrMapper paper. Please read this folder's README for more information.

### userData
The folder where the uploaded datasets and performed analyses of the users are
saved.
