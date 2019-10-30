Auto_MARTINI
============
## What is Auto_MARTINI?
A toolkit that enables automatic generation of MARTINI forcefields for small organic molecules. 

For a detailed account of the software, see:

Bereau and Kremer, *J Chem Theory Comput*, DOI:10.1021/acs.jctc.5b00056 (2015)

[![DOI for Citing auto_martini](https://img.shields.io/badge/DOI-10.1021%2Facs.jctc.5b00056-blue.svg)](http://dx.doi.org/10.1021/acs.jctc.5b00056)

Please consider citing the paper if you find `auto_martini` useful in your research.
```
@article{bereau2015automartini,
author = {Bereau, Tristan and Kremer, Kurt},
title = {Automated parametrization of the coarse-grained MARTINI force field 
    for small organic molecules},
journal = {J Chem Theory Comput},
year = {2015},
volume = {11},
number = {6},
pages = {2783-2791},
doi = {10.1021/acs.jctc.5b00056}
}
```

For full documentation, click [here](https://tbereau.github.io/auto_martini/docs/html/index.html).

## Developers 
* Tristan Bereau (Max Planck Institute for Polymer Research, Mainz, Germany)   
* Kiran Kanekal (Max Planck Institute for Polymer Research, Mainz, Germany)     
* Andrew Abi-Mansour (Molecular Science Software Institute, Virginia Tech, Blacksburg, US)

## Installation & Testing
`auto-martini` requires a number of dependencies:
* [numpy](http://docs.scipy.org/doc/numpy/user/install.html)
* [rdkit](http://www.rdkit.org/docs/Install.html)
* [bs4](http://www.crummy.com/software/BeautifulSoup)
* [requests](http://docs.python-requests.org/en/latest/user/install)
* [lxml](https://github.com/lxml/lxml)

For optimal performance, we recommend installing cython as well. We also recommend installing the latest version of rdkit with conda. If no rdkit installation is found, auto_martini
will attempt to compile it from its source code. For a detailed installation, see the documentation.

Once all the dependencies are correctly installed, auto_martini can be run via:
```
python -m auto_martini [mode] [options]
```
By default, mode is set to 'run', which computes the MARTINI forcefield for a given molecule. In order to make sure auto_martini runs correctly on your system, run:
```
python -m auto_martini test
```
To display the usage-information (help), either supply -h, --help, or nothing to auto_martini:
 
```
usage: auto_martini [-h] [--mode {run,test}] [--sdf SDF | --smi SMI]
                    [--mol MOLNAME] [--aa AA] [--cg CG] [--top TOPFNAME] [-v]
                    [--fpred]

Generates Martini force field for atomistic structures of small organic molecules

optional arguments:
  -h, --help         show this help message and exit
  --mode {run,test}  mode: run (compute FF) or test (validate)
  --sdf SDF          SDF file of atomistic coordinates
  --smi SMI          SMILES string of atomistic structure
  --mol MOLNAME      Name of CG molecule
  --aa AA            filename of all-atom structure .gro file
  --cg CG            filename of coarse-grained structure .gro file
  --top TOPFNAME     filename of output topology file
  -v, --verbose      increase verbosity
  --fpred            verbose

Developers:
===========
Tristan Bereau (bereau [at] mpip-mainz.mpg.de)
Kiran Kanekal (kanekal [at] mpip-mainz.mpg.de)
Andrew Abi-Mansour (andrew.gaam [at] gmail.com)
```

## Example
To coarse-grain a molecule, simply provide its SMILES code (option `--smi SMI`) or a .SDF file (option `'--sdf file.sdf`). You also need to provide a name for the CG molecule (not longer than 5 characters) using the `--mol` option.  For instance, to coarse grain [guanazole](http://pubchem.ncbi.nlm.nih.gov/summary/summary.cgi?cid=15078), you can either obtain/generate (e.g., from Open Babel) an SDF file:
```
python -m auto-martini --sdf guanazole.sdf --mol GUA --top GUA.itp
```
(the name GUA is arbitrary) or use its SMILES code within double quotes
```
python -m auto-martini --smi "N1=C(N)NN=C1N" --mol GUA --top GUA.itp
```
In case no problem arises, it will output the gromacs GUA.itp file:
```
;;;; GENERATED WITH auto-martini
; INPUT SMILES: N1=C(N)NN=C1N
; Tristan Bereau (2014)

[moleculetype]
; molname       nrexcl
  GUA           2

[atoms]
; id    type    resnr   residu  atom    cgnr    charge  smiles
  1     SP2     1       GUA     S01     1       0     ; Nc1ncnn1
  2     SP2     1       GUA     S02     2       0     ; Nc1ncnn1

[constraints]
;  i   j     funct   length
   1   2     1       0.21
```
Optionally, the code can also output a corresponding `.gro` file for the coarse-grained coordinates
```
python -m auto-martini --smi "N1=C(N)NN=C1N" --mol GUA --gro gua.gro --top GUA.itp
```
Atomistic coordinates can be output in XYZ format using the `--xyz output.xyz` option.

## Caveats

For frequently encountered problems, see [FEP](FEP.md)

