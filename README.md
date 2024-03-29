QC Web Reports
========

This repository contains a set of Quality Control utilities that can be used to analyze Variant Callsets
generated by the Broad Institute's Genomics Platform.

## Background

The need for this repository was identified by Daniel MacArthur and Mark Daly.
The Genomics Platform provides many advanced QC metrics for use in large scale sequencing projects,
but they can be inaccessible to end users without extensive experience in sequence analysis.
Notably, this includes many users of xBrowse, such as clinical geneticists and wet lab biologists.
So, Daniel and Mark have provided a mandate for us to build intuitive, easy to use QC utilities.
We have agreed that a dynamic website is a better platform to meet these needs than a static PDF report or a command line utility.
This repository is Step 1.

## Goals

There are a few goals with this repository:

- **Web reports**: The primary product in this repository is a website that can be used to explore QC metrics in an arbitrary sequencing project.
We refer to this as a *web report*, since it is a replacement for a traditional PDF, but with the interactivity of a dynamic website.

- **Basic inference**: A web report includes simple heuristics for identifying clear examples of failed QC.
For example, we "flag" a sample where the % of novel variants is outside a certain range.
However, note that this repo does *not* aim to ever replace formal QC performed by a qualified analyst.
The inference methods here will remain very basic.

- **Data definition**: The data used to generate a web report is organized in a standard file structure that we will
clearly document and version here. This fileset can be used for further QC.

## Big Picture

The goal is to

## Workflow

Generating a QC Web Report takes the following high level workflow.

#### Define The Project

A report is specific to a *project*, which is just a set of samples that are analyzed together.
The one restriction is that a project must be entirely contained in one Picard variant calling project
(ie. all samples must be included in the same VCF).
However, a project can only contain a subset of samples from that VCF, as will often be the case in large scale joint calling exercises.

To define a project, the user must fill out a `project.yaml` file. This file contains metadata such as:

- Basic info like name and description, which samples to include

- Locations of all relevant files, including the Picard output directory and QC output directory.

- *Data classification* - is this a Mendelian or common disease study, exome or genome, etc.

The precise format of project.yaml will evolve rapidly. We suggest users keep their project.yaml files in version control.

#### Create QC Report

After the project is specificed, the user needs to run a single command: `python web_qc_report.py /path/to/project.yaml`.
This reads the project specification and indexes some QC data in a file `web_qc_report.index` that sits next to `project.yaml`.
It also starts a local web server to server the report.

If a user wants to recreate the report, she can just delete `web_qc_report.index` and run that script again;
the index file will be created again.

#### View Report in Browser

Open browser to http://localhost:7777 and the report is available there.

#### Sharing the Report

Again, this is a *development* repository - this code is not meant to be deployed in the wild as a QC portal.
But we also want people to be able to review these reports while they are under development.

The plan is to do this by requesting a new VM within the Broad network that has access to the the Broad filesystem,
and running `web_qc_report.py` in a tmux session.
The site will likely only be available within the Broad network.

I haven't tried this, but it should be straightforward, and will update with more instructions if we go this route.

## References

- Much of the design here was informed by Mauricio Carneiro's work.
He has written a white paper on variant QC that is available here: https://github.com/broadinstitute/variant-qc

- *TODO: need to add more references to other resources around Broad*

## Contact

With any questions, comments, etc., contact Brett Thomas at bthomas@broadinstitute.org.
This repository will remain open source and we'd be thrilled for others to contribute.