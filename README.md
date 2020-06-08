# QuickLogic examples

This repository provides example FPGA designs that can be built using QuickLogic open source toolchain.
The examples target the EOS S3 devices.

The repository includes:

* Travis CI configuration file
* Build scripts to generate the environment:

  * Conda configurations
  * Python requirements
  * Environment setup

* Example FPGA designs including:

  * Verilog code
  * Pin constraints files
  * Timing constraints files
  * Makefiles for running the QuickLogic toolchain

## Description

Travis-based CI in this repository runs all the steps required to build the example designs and generate bitstreams for programming the FPGA devices.

The CI performs the following steps:

* [Miniconda](https://docs.conda.io/en/latest/miniconda.html) installation and configuration
* Installation of the required conda packages (toolchains and Python modules). Note that Python packages can be installed using any Python package manager:

    * [VTR](https://anaconda.org/antmicro/vtr)
    * [Yosys](https://anaconda.org/antmicro/yosys)
    * [Yosys-plugins](https://anaconda.org/antmicro/yosys-plugins)
    * [lxml](https://anaconda.org/conda-forge/lxml), [simplejson](https://anaconda.org/conda-forge/simplejson), [intervaltree](https://anaconda.org/conda-forge/intervaltree), [python-constraint](https://anaconda.org/conda-forge/python-constraint), [git](https://anaconda.org/conda-forge/git), [pip](https://anaconda.org/conda-forge/pip), [fasm](https://github.com/SymbiFlow/fasm), [quicklogic-fasm]() and [quicklogic-fasm-utils]()

## Toolchain installation

This block of code regards the toolchain installation. It is divided in three main steps:

* Conda setup
* Conda packages installation
* Architecture definitions installation

```bash
INSTALL_DIR=/opt/quicklogic
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
bash miniconda.sh -b -p $INSTALL_DIR/miniconda && rm miniconda.sh
source "$INSTALL_DIR/miniconda/etc/profile.d/conda.sh"
conda config --set always_yes yes --set changeps1 no
conda update -q conda
conda config --add channels conda-forge
conda config --add channels antmicro/label/ql
conda activate
conda install -c antmicro/label/ql yosys
conda install -c antmicro/label/ql yosys-plugins
conda install -c antmicro/label/ql vtr
conda install lxml simplejson intervaltree python-constraint git pip
pip install git+https://github.com/symbiflow/fasm
pip install git+https://github.com/antmicro/quicklogic-fasm
pip install git+https://github.com/antmicro/quicklogic-fasm-utils
conda deactivate
```

## Build Example Designs

With the toolchain installed, you can build the example designs.

The example designs are provided in separate directories:

1. `quicklogic` - simple 4-bit counter driving LEDs. The design targets the [EOS S3 board](https://www.quicklogic.com/products/eos-s3/).

To build the examples, run following commands:

```bash
INSTALL_DIR=/opt/quicklogic
source "$INSTALL_DIR/miniconda/etc/profile.d/conda.sh"
git clone https://github.com/antmicro/symbiflow-examples.git
cd symbiflow-examples
tar -xf ./arch-defs-install.tar.xz -C $INSTALL_DIR
# adding quicklogic toolchain binaries to PATH
export PATH=$INSTALL_DIR/install/bin:$PATH
conda activate
pushd quicklogic_demo && make && popd
```
