# FATES land use data processing tool

A python tool for processing LUH2 data for use with FATES landuse run modes

## Purpose

This tool creates the necessary land use datasets to be used as inputs for running FATES with land use cover and change.  The tool has two major options, i.e. python subcommands, to provide the data set of interest:

- land use timeseries data
- land use association to plant functional type (land use x pft) static mapping data

The tools provides these data sets as netcdf files by concatenating the various raw input data sets into a single file.  The tool provides the ability to regrid the source data resolution to a target
resolution that the user designates for either output option.  The output data is then usable by FATES, mediated through a host land model (currently either CTSM or E3SM).

takes the raw  data files as

## Input data sources

The tool requires input data from two primary sources: the [Land Use Harmonization](https://luh.umd.edu/), or LUH2, historical data sets and the [THESIS Tools Datasetes](https://doi.org/10.5065/29s7-7b41).  A static mapping file from the LUH2 data is required for both output options.  The historical LUH2 transient, state, and management data is used as input for the FATES land use timeseries data output option, whereas the THEMIS tools forest, pasture, "other" and current surface data sets are necessary for the land use x pft static map output option.  These data sets can be downloaded directly from their respective sources and are not provided as part of this package.

## Installation

This package is available through the [ngeetropics anaconda channel](https://anaconda.org/ngeetropics/tools-fates-landusedata):

To install in an existing environment:
``` sh
conda install ngeetropics::tools-fates-landusedata
```

To install the tool in a new environment:

``` sh
conda create -n <new-env-name> ngeetropics::tools-fates-landusedata
```

## Usage

This tool is meant to be utilized from the command line.  The top level call for the command line with help is shown below:
``` sh
$ fates-landusedata -h
usage: fates-landusedata [-h] {luh2,lupft} ...

FATES landuse data tool

options:
  -h, --help    show this help message and exit

subcommands:
  {luh2,lupft}  landuse data tool subcommand options
    luh2        generate landuse harmonization timeseries data output
    lupft       generate landuse x pft static data map output
```

The `luh2` subcommand call tells the tool to build the combined landuse timeseries data:

``` sh
$ fates-landusedata luh2 -h
usage: luh2 [-h] [-w REGRIDDER_WEIGHTS] [-b BEGIN] [-e END] [-o OUTPUT] regrid_target_file luh2_static_file luh2_states_file luh2_transitions_file luh2_management_file

positional arguments:
  regrid_target_file    target surface data file with desired grid resolution
  luh2_static_file      luh2 static data file
  luh2_states_file      full path of luh2 raw states file
  luh2_transitions_file
                        full path of luh2 raw transitions file
  luh2_management_file  full path of luh2 raw management file

options:
  -h, --help            show this help message and exit
  -w REGRIDDER_WEIGHTS, --regridder_weights REGRIDDER_WEIGHTS
                        filename of regridder weights to save
  -b BEGIN, --begin BEGIN
                        beginning of date range of interest
  -e END, --end END     ending of date range to slice
  -o OUTPUT, --output OUTPUT
                        output filename
```

The `lupft` subcommand tells the tool to build the landuse x pft mapping file:

``` sh
$ fates-landusedata lupft -h
usage: lupft [-h] [-o OUTPUT] regrid_target_file luh2_static_file clm_luhforest_file clm_luhpasture_file clm_luhother_file clm_surface_file

positional arguments:
  regrid_target_file    target surface data file with desired grid resolution
  luh2_static_file      luh2 static data file
  clm_luhforest_file    CLM5_current_luhforest_deg025.nc
  clm_luhpasture_file   CLM5_current_luhpasture_deg025.nc
  clm_luhother_file     CLM5_current_luhother_deg025.nc
  clm_surface_file      CLM5_current_surf_deg025.nc

options:
  -h, --help            show this help message and exit
  -o OUTPUT, --output OUTPUT
                        output filename

```

