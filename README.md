# Visual Programming with pyironflow
[![Deploy](https://github.com/pythonworkflow/visual-programming-with-pyironflow/actions/workflows/deploy.yml/badge.svg)](https://github.com/pythonworkflow/visual-programming-with-pyironflow/actions/workflows/deploy.yml)
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/pythonworkflow/visual-programming-with-pyironflow/HEAD)

Explore, execute and debug workflows stored in the [Python Workflow Definition](https://pythonworkflow.github.io/python-workflow-definition/) (PWD) with the visual programming environment [pyironflow](https://github.com/pyiron/pyironflow).

![screenshot](arithmetic/screenshot.png)

The [Python Workflow Definition](https://pythonworkflow.github.io/python-workflow-definition/) is a format for exchanging workflows between workflow engines, including [AiiDA](https://www.aiida.net), [jobflow](https://github.com/materialsproject/jobflow) and [pyiron](https://github.com/pyiron). Each node of a PWD workflow is a Python function, and each edge connects the output of one function to the input of another. This repository adds a graphical perspective to the format. [pyironflow](https://github.com/pyiron/pyironflow) is a visual programming environment built on [ReactFlow](https://reactflow.dev). It runs inside Jupyter and works on top of the [pyiron_workflow](https://github.com/pyiron/pyiron_workflow) workflow engine. The three example workflows of the PWD, from a simple arithmetic example to a file-based finite element simulation and a density functional theory calculation, are loaded, run and inspected in pyironflow.

This repository is published as a website at [pythonworkflow.github.io/visual-programming-with-pyironflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/).

## Key Features
* **See the workflow you discuss.** New workflows are commonly introduced as data flow diagrams. A visual programming environment shows exactly this diagram, generated directly from the machine-readable workflow definition. You can browse through the workflow and zoom in on individual parts to see which data is passed between the steps.
* **Debug the data flow step by step.** Instead of running the full workflow, you can run individual nodes together with the nodes they depend on (*pull*), or run a node and everything downstream of it (*push*). You can then inspect the inputs, outputs and source code of every step directly in the graphical user interface.
* **Share workflows across user groups.** Users with less programming experience can run workflows developed by experts through a graphical interface, without writing Python code. This speeds up the transfer of knowledge between user groups.
* **Stay interoperable.** pyironflow reads and writes the same `workflow.json` files as all other PWD workflow engines. A workflow developed with [AiiDA](https://www.aiida.net) or [jobflow](https://github.com/materialsproject/jobflow) can be explored in pyironflow, and a workflow built with [pyiron_workflow](https://github.com/pyiron/pyiron_workflow) can be run with any other PWD engine.

## Quickstart
Every example is stored in the three files of the Python Workflow Definition: a Python module `workflow.py` with the Python functions, a `workflow.json` file with the connections between them, and a conda environment with the software dependencies. Run the following in the directory of one of the examples to load the workflow into pyironflow:

```python
from pyironflow import PyironFlow
from python_workflow_definition.pyiron_workflow import load_workflow_json

PyironFlow([load_workflow_json(file_name="workflow.json")]).gui
```

`load_workflow_json()` translates the PWD `workflow.json` into a `pyiron_workflow` `Workflow`, and `PyironFlow` displays a list of workflows, each in its own tab of the graphical user interface. To go the other way, export a workflow defined in `pyiron_workflow` to the Python Workflow Definition:

```python
from python_workflow_definition.pyiron_workflow import write_workflow_json

write_workflow_json(graph_as_dict=wf.graph_as_dict, file_name="workflow.json")
```

## Examples
The repository contains the same three example workflows that the [Python Workflow Definition](https://pythonworkflow.github.io/python-workflow-definition/) implements for other workflow engines. Each notebook first reloads the existing `workflow.json` in pyironflow and runs it. It then shows how the workflow is defined with `pyiron_workflow` and exported back to `workflow.json`.

| Example | Description | Software |
|---------|-------------|----------|
| [Arithmetic](arithmetic/pyironflow.ipynb) | The smallest PWD workflow, which combines three Python functions (`get_prod_and_div`, `get_sum` and `get_square`). It introduces the format of `workflow.json` and the main elements of pyironflow. | Python standard library |
| [NFDI4Ing](nfdi/pyironflow.ipynb) | The [NFDI4Ing scientific workflow benchmark](https://github.com/BAMresearch/NFDI4IngScientificWorkflowRequirements). It generates a mesh, solves the Poisson equation with the finite element method, post-processes the solution and compiles a LaTeX paper with the results. This shows how file-based workflows that call external codes in separate conda environments are represented. | [gmsh](https://gmsh.info), [meshio](https://github.com/nschloe/meshio), [FEniCS](https://fenicsproject.org), [ParaView](https://www.paraview.org), [tectonic](https://tectonic-typesetting.github.io) |
| [Quantum ESPRESSO](quantum_espresso/pyironflow.ipynb) | The energy-volume curve of fcc aluminium. The cell is first relaxed, then five strained structures are computed independently and the results are collected in a plot. This shows how parallel branches and the PWD helper functions `get_dict` and `get_list` appear in pyironflow. | [Quantum ESPRESSO](https://www.quantum-espresso.org), [ASE](https://wiki.fysik.dtu.dk/ase/), [OPTIMADE](https://www.optimade.org) |

## Repository Structure
```
.
├── README.md                  # this overview, also the landing page of the website
├── environment.yml            # conda environment for all examples (used by Binder)
├── arithmetic/
│   ├── pyironflow.ipynb       # notebook: reload and define the workflow with pyironflow
│   ├── workflow.py            # Python functions
│   ├── workflow.json          # Python Workflow Definition
│   └── screenshot.png         # pyironflow screenshot for the website
├── nfdi/
│   ├── pyironflow.ipynb
│   ├── workflow.py
│   ├── workflow.json
│   ├── screenshot.png
│   └── source/                # input templates, scripts and conda environments of the benchmark
│       ├── envs/              # preprocessing.yaml, processing.yaml, postprocessing.yaml
│       ├── unit_square.geo    # geometry for gmsh
│       ├── poisson.py         # FEniCS solver for the Poisson equation
│       ├── postprocessing.py  # ParaView plot over line
│       ├── prepare_paper_macros.py
│       ├── macros.tex.template
│       └── paper.tex          # LaTeX paper template
├── quantum_espresso/
│   ├── pyironflow.ipynb
│   ├── workflow.py
│   ├── workflow.json
│   ├── screenshot.png
│   ├── espresso/pseudo/       # pseudopotential for aluminium
│   └── qe_xml_parser/         # parser for the Quantum ESPRESSO XML output
└── .github/
    ├── _config.yml            # Jupyter Book configuration
    ├── _toc.yml               # Jupyter Book table of contents
    └── workflows/             # build (pull requests) and deploy (main) of the website
```

## Installation
The fastest way to try the examples is [Binder](https://mybinder.org/v2/gh/pythonworkflow/visual-programming-with-pyironflow/HEAD), which builds the conda environment from `environment.yml` automatically. To run the examples locally, create the same environment with [conda](https://conda-forge.org):

```
conda env create -f environment.yml
```

The environment includes `pyironflow`, `pyiron_workflow` and `python-workflow-definition`, together with the dependencies of the arithmetic and Quantum ESPRESSO examples. Two of the examples need additional steps:

* **Quantum ESPRESSO:** install the bundled output parser with `pip install ./quantum_espresso/qe_xml_parser`. Quantum ESPRESSO looks for pseudopotentials in the directory named by the `ESPRESSO_PSEUDO` environment variable, so set it to the absolute path of `quantum_espresso/espresso/pseudo`.
* **NFDI4Ing:** the individual steps run in three separate conda environments. `conda_subprocess` selects them by name, so create them with the names `preprocessing`, `processing` and `postprocessing`:
  ```
  conda env create -f nfdi/source/envs/preprocessing.yaml -n preprocessing
  conda env create -f nfdi/source/envs/processing.yaml -n processing
  conda env create -f nfdi/source/envs/postprocessing.yaml -n postprocessing
  ```
  The `source_directory` input in `nfdi/workflow.json` is an absolute path. Regenerate the file with the second part of the [NFDI4Ing notebook](nfdi/pyironflow.ipynb) before running the workflow on a new computer.

The website is built with [Jupyter Book](https://jupyterbook.org). The notebooks are not executed during the build; the stored outputs are rendered as they are.

## Related Projects
* [Python Workflow Definition](https://pythonworkflow.github.io/python-workflow-definition/): the workflow exchange format, with the same examples implemented for [AiiDA](https://www.aiida.net), [jobflow](https://github.com/materialsproject/jobflow), [pyiron_base](https://github.com/pyiron/pyiron_base) and pure Python. It is described in [J. Janssen et al., Digital Discovery (2025)](https://doi.org/10.1039/D5DD00231A).
* [pyironflow](https://github.com/pyiron/pyironflow): the visual programming environment used in this repository.
* [pyiron_workflow](https://github.com/pyiron/pyiron_workflow): the graph-based workflow engine behind pyironflow.
* [NFDI4Ing Scientific Workflow Requirements](https://github.com/BAMresearch/NFDI4IngScientificWorkflowRequirements): the benchmark behind the NFDI4Ing example.

## Documentation
* [Arithmetic Workflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/arithmetic/pyironflow.html)
  * [Reload workflow in pyironflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/arithmetic/pyironflow.html#reload-workflow-in-pyironflow)
  * [Define workflow in pyironflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/arithmetic/pyironflow.html#define-workflow-in-pyironflow)
* [NFDI4Ing](https://pythonworkflow.github.io/visual-programming-with-pyironflow/nfdi/pyironflow.html)
  * [Reload workflow in pyironflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/nfdi/pyironflow.html#reload-workflow-in-pyironflow)
  * [Define workflow with pyironflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/nfdi/pyironflow.html#define-workflow-with-pyironflow)
* [Quantum Espresso Workflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/quantum_espresso/pyironflow.html)
  * [Reload workflow in pyironflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/quantum_espresso/pyironflow.html#reload-workflow-in-pyironflow)
  * [Define workflow with pyiron_workflow](https://pythonworkflow.github.io/visual-programming-with-pyironflow/quantum_espresso/pyironflow.html#define-workflow-with-pyiron-workflow)
