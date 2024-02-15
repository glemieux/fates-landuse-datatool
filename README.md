# FATES LUH2 data tool README

A python tool for processing LUH2 data for use with FATES landuse run modes

## Purpose

This tool takes the raw Land Use Harmonization (https://luh.umd.edu/), or LUH2, data files as
input and prepares them for use with FATES.  The tool concatenates the various raw data sets into
a single file and provides the ability to regrid the source data resolution to a target
resolution that the user designates.  The output data is then usable by FATES, mediated through
a host land model (currently either CTSM or E3SM).
