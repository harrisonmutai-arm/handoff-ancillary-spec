[![Build Status](https://github.com/FirmwareHandoff/ProjectSpecificTransferEntries/actions/workflows/main.yml/badge.svg)](https://github.com/FirmwareHandoff/ProjectSpecificEntries/actions/workflows/main.yml)
[![Daily Status](https://github.com/FirmwareHandoff/ProjectSpecificEntries/actions/workflows/daily.yml/badge.svg)](https://github.com/FirmwareHandoff/ProjectSpecificEntries/actions/workflows/daily.yml)
[![Release Version](https://img.shields.io/github/v/release/FirmwareHandoff/ProjectSpecificEntries?label=release)](https://github.com/FirmwareHandoff/ProjectSpecificEntries/releases)

This repository contains the Transfer Entry definitions that are project-specific.
This is an auxiliary document to theFirmware Handoff specification, which defines a
data structure to transfer essential configuration information between firmware
stages during platform initialization.

The documentation is generated using the Sphinx framework. A version of this
specification, rendered in HTML, is available
[here](https://firmwarehandoff.github.io/ProjectSpecificEntries/).

Project dependencies
====================

For an Ubuntu development machine, install the following packages to build the specification:

- `librsvg2-bin`
- `python3-sphinx`
- `python3-sphinxcontrib.svg2pdfconverter`
- `python3-sphinx-rtd-theme`
- `sphinx-multiversion`
- `latexmk`
- `texlive-latex-extra`

**Note:** This list has been tested on Ubuntu 22.04 LTS and 24.04 LTS running on AArch64 and AMD64.

Building the document
=====================

The following are use to generate the specification:

- pdf:

``` sh
make latexpdf
```

- html:

``` sh
make html
```

The output of these build commands goes into subdirectory `build`.
