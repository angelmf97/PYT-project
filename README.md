# Protein-DNA Interaction Builder (PDIB) 
*María Torralvo, Ángel Monsalve and Mireia Codina*


## Software requirements
Python version:
* Python 3.8.5

### Required packages:
* BioPython
* Tkinter

### Optional requirements
For refining the models:
* PyRosetta [python3.8 version]

For computing the Z-Scores:
* ProSa2003

## Installation

This file provides a usage guideline to run PDIB. The program can be installed from the parent directory using the following command:

```
python setup.py install
```

## Command line arguments

```
usage: pdib.py [-h] [-i INPUT_DIRECTORY] [-s STOICHIOMETRY] -o
                              OUTPUT_DIRECTORY [-f] [-v]

Make protein complexes from protein-protein and protein-DNA interactions.

optional arguments:
  -h, --help
Show this help message and exit
  -i INPUT_DIRECTORY, --input-directory INPUT_DIRECTORY
                        Input directory containing PDB files
  -o OUTPUT_DIRECTORY, --output-directory OUTPUT_DIRECTORY
                        Output directory to store the models
  -f, --force	
Overwrites of the output directory if it
                        already exists
  -v, --verbose	
Gives information during the execution of the script
  -all, --all_models
		Try all the models. It might take a long time! It is not recommended (specially when the complex is large) unless you suspect that more than one model is possible
  -n_models, -number-of-models
		Number of models (an integer) to be generated by the pipeline
  -sto, --stoichiometry
		File with sequence ID and stoichiometry
  -n_chains, --number-of-chains
		Expected number of chains
  -e, --energies
		Compute the energies of the resulting models and their z-scores
  -r, --refine
		Uses PyRosetta in order to improve the obtained models


```

## Usage example

An example is provided with the program and can be run by using the following command:
```
pdib.py -i examples/1gzx/ -o . -f -v > log.txt
```
This will create two folders inside the current directory: "Structures" and "Analysis". The first one contains all the structures that PDIB could find meeting the requirements of the user. The latter contains (if the option is selected) a file with the Z-Score of the generated models, so the user can choose the one with the best score.

As the verbose (-v) option is enabled, the program will print information about the process to the standard output, that is redirected to a new file named "log.txt" in this example. With the -f option, the files stored in the output directory specified are going to be overwritten. 


## Program overview

Protein-DNA Interaction Builder (PDIB) is a python program that aims to generate protein macrocomplexes from known interactions between pairs of chains. The program is able to model both protein-protein, protein-DNA and protein-RNA interactions.

As an input, it takes PDB files containing pairs of interacting chains that will form the final structure, in order to know the relative position of each of the subunits. By making use of a recursive approach, PDIB explores different ways of arranging the specified chains, yielding several models that preserve the interactions between pairs of chains as specified in the interaction files.

The program also allows the user to use an stoichiometry file, specifying the number of times that each chain should appear in the final complex, and the program will try to find solutions that meet the user requirements.

In order to yield better and more reliable results, the program can use PyRosetta to perform a relax pipeline that optimizes the energy of the models. In addition, ProSa2003 can be runned from PDIB to retrieve the z-scores of the generated model, so the user can identify those with a better energy profile.

The program can be run from the command line or using a graphical user interface (GUI) that allows to run PDIB with all its functionalities in a more user-friendly way.


## Input files

PDIB will look for all the files having a .pdb extension in the directory specified by -i. 

Optionally, the user may provide an stoichiometry file, indicating the number of times each chain appears in the final complex. The stoichiometry file must have the following format:

```
Chain1: 1
Chain2: 1
```
Where the number at the right indicates the number of appearances of that chain. The colons (":") can be replaced with an equal sign ("=") if the user pleases. The chain id in the stoichiometry should be concordant with those in the pairwise interaction pdb files.

## Generating several models

The program provides several options for controlling the number of models generated. The argument -n_models allows the user how many different models wants to obtain. PDIB will try to generate that number of models if it is possible, although it will avoid returning identical models, so if the number of different possible models is less than n_models the user may receive less models than specified by n_models.
Another way of limiting the number of outputs is by specifying the number of chains that the final models will have, which can be done through the argument -n_chains. The program will yield only models having that number of chains.

Finally, PDIB allows to generate all the possible models with the option -exh. The program will make an exhaustive search for all the possible combinations of chains, which expands considerably the executing time of the program. Using this option is not recommended for big complexes. 


## Graphical User Interface

We have developed a GUI to run PDIB without having to use the command line. It has different button blocks to select inputs that are mandatory for the program to run properly and another one to select the optional ones. 

\includegraphics[width=7cm]{images/GUI_default.png}

In the required inputs frame, the input and the output folders can be browsed from the computer. Here, it is important that the output folder already contains inside two directories named as follows: structures and analysis. Moreover, it has a list box where the PDB files found in the input directory are displayed. The next image shows how the files from the 1gzx folder are represented in the GUI. 

\includegraphics[width=7cm]{images/GUI_options.png}

Then, the user can choose to either specify the number of models that wants PDIB to generate or select the option of generating all possible models. 

In the optional inputs frame, there are buttons to select if the user wants the models to be refined or/and obtain the Z-scores file to analyse the energies. Below these options, the user can select a .txt file with the desired stoichiometry in the format already mentioned before or change the number of maximum chains that the models should have. 

The Run PDIB button runs the program with the specified options.

We are still progressing in the development of the GUI. However, until now, we have been able to run it with the required input options and also with the optional input of selecting to refine the models. 



