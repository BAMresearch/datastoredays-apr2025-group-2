
In [Block I - Introduction to the BAM Data Store](block_1.md), we saw what is the BAM Data Store structure, and how it is based on two main ingredients to populate the data model with actual data: the Masterdata definitions (composed of object types, property types, and vocabularies) + the parent-child relationships.

At BAM, we already have defined Masterdata, see [bam-masterdata](https://github.com/BAMresearch/bam-masterdata). In some cases, these Masterdata definitions are not specific enough for your use-case. For those cases, we will show you here how to create your own Github repository to define and work extending current BAM Masterdata. If you are interested about working collaboratively on the generic BAM Masterdata definitions, you can contact us and request access to our ([WebProtégé](https://webprotege.stanford.edu/)) project.

## `bam-masterdata` <img src="../assets/bammasterdata_blue_transparent.png" alt="bam-masterdata logo" width="20px"/>

`bam-masterdata` is a Python package with a set of API functions used to work and manipulate openBIS Masterdata. Our goal is to be able to define Masterdata in the most common formats (Excel, Python, and RDF/XML) while parsing back and forth between them. At BAM, the generic Masterdata definitions are defined in the [datamodel folder](https://github.com/BAMresearch/bam-masterdata/blob/main/bam_masterdata/datamodel/), and they are used as the basis to extend the current BAM Masterdata. The package has some utilities to [check](#check-consistency) the consistency of the defined Masterdata.

This package uses a CLI for using the API functions. For example, if you defined your Masterdata in Excel, you can easily transform this into Python by doing (in a terminal and with `bam-masterdata` installed as a pip package):
```sh
bam_masterdata fill_masterdata --excel-file=<the-path-to-your-Excel-file>
```

As for any CLI, you can read more about each specific option by adding `--help` at the end of any command.
```sh
Usage: bam_masterdata [OPTIONS] COMMAND [ARGS]...

  Entry point to run `bam_masterdata` CLI
  commands.

Options:
  --help  Show this message and exit.

Commands:
  checker          Checks the files specified...
  export_to_excel  Export entities to an...
  export_to_json   Export entities to JSON...
  export_to_rdf    Export entities to a...
  fill_masterdata  Fill the masterdata from...
```

In Excel, we have manually defined inheritance between object types for paving the way to implement ontologies in the platform. Such inheritance is specified for each object type by defining the `code` of such object as a path of codes (starting from the parent) separated by dots. For example, if we have to define `SuperProcessedSample` as inheriting from `ProcessedSample`, which at the same time is inheriting from `Sample`, then the `code` for all these object types will be: `SAMPLE`, `SAMPLE.PROCESSED_SAMPLE`, and `SAMPLE.PROCESSED_SAMPLE.SUPER_PROCESSED_SAMPLE`.

??? warning "bam-masterdata documentation"
    As of April 2025, the `bam-masterdata` package is in a alpha testing phase. Unfortunately, we have not written any documentation for all the API of the package. This is partly because we want feedback from potential users and collaborators. 


## Create and define your own Masterdata

### Create a new Github repository

Go to [BAMresearch/masterdata-template](https://github.com/BAMresearch/masterdata-template) and click on the top-right button, **Use this template > Create a new repository**.

This will take you to another page where you can decide where to host the new Github repository and some other settings. We recommend starting the name of your repository with `masterdata-`, so it can be easily cataloged.

After creating your new repository, you can either clone it locally (we will follow this approach) or open it in Github Codespace:

You also need to [install `cruft`](https://cruft.github.io/cruft/#installation).

After cloning your repository, move into it:
```sh
git clone <path-to-your-repository>
cd <path-to-your-repository>
```
And use `cruft` with the following command to create the structure of the new Masterdata project:
```sh
cruft create https://github.com/BAMresearch/masterdata-generator
```

This will prompt you with some questions to be filled in:
```sh
  [1/9] author (Erika Mustermann): 
  [2/9] email (erika.mustermann@bam.de): 
  [3/9] Github organization or profile name (BAMResearch): 
  [4/9] project_name (masterdata-example): 
  [5/9] The name of the folder src/<module_name>, default is the 
<project_name> separated with underscores (masterdata_example): 
  [6/9] short_description (A short description for the masterdata project.): 
  [7/9] include_python_masterdata [y/n] (y): 
  [8/9] include_excel_masterdata [y/n] (y): 
  [9/9] include_rdfxml_masterdata [y/n] (y): 
```

!!! warning
    Not filling anything on the questions above will result on choosing the default. The default value for each question is defined in between parenthesis next to the question.

The files generated by `cruft` are created under `<your-repository-name>/<project-name>`. To move all the structure one level up, from the repository local location run:
```sh
sh <project-name>/move_generated_files.sh
```

After filling the questionnaire and running the `move_generated_files.sh` script, you will have a new repository with:
```sh
masterdata-<name>/
├── LICENSE (MIT)
├── README.md
├── pyproject.toml
├── src/
│   └── masterdata-<name>/
#        if Python is selected as an option
│       ├── __init__.py
│       ├── object_types.py
│       ├── vocabulary_types.py
#       if Excel is selected as an option
│       ├── masterdata.xlsx
#       if RDF/XML is selected as an option
│       └── masterdata.owl
└── tests/
    ├── __init__.py
    └── test.py
```

If you are using Python, you can install the package and its dependencies by creating a virtual environment (either with `conda` or `venv`) and running:
```sh
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install uv
uv pip install -e '.[dev]'
```

The newly created Github repository is ready for packaging and create releases if you want. It has a Github workflow defined (amongst them there is the [`checker` action](#check-consistency)). If you choose to keep your repository private, you will need to setup your own runners or run locally each action everytime you push changes to the repository.

### Define Masterdata

Once you have the skeleton of the Masterdata repository, you can define new classes in the corresponding file under `src/<module-name>/`. In case of Python, these are different modules for Object Types and Vocabularies. For Excel or RDF/XML, this is a single file named `masterdata.xlsx`/`masterdata.owl`, respectively. In all the cases, a dummy entity called `ExampleObjectType` is created to serve as an example.

Depending on the format chosen (Excel, Python, RDF/XML), you can follow the example to create new object types and vocabularies. A full list of datatypes for the assigned properties can be found in the [openBIS documentation](https://openbis.readthedocs.io/en/20.10.x/user-documentation/general-admin-users/admins-documentation/new-entity-type-registration.html#property-data-types). We also defined in our [Wiki page](https://datastore.bam.de/en/datastore/stewards/definition-of-masterdata) the process of how to define Masterdata in Excel.


### Check consistency

One of the main API functions of `bam-masterdata` is the `checker`. This functionality is integrated in the Github workflow actions of the repository created before. Its main role is to check that the defined object types, property types, and vocabularies are consistent and there are no conflicts in the repository itself but also with respect to the generic types defined in `bam-masterdata`. It also validates the Masterdata definitions format and ensures that it is compliant with the openBIS database structure.

We also deployed a website to check the Masterdata definitions in an Excel. You can visit the [Checker website](https://mdc.datastore.bam.de/) and upload an Excel file. Note that this website is a bit outdated and will soon be improved with the current `check` API functionality.


## Collaborate and define generic entity types

The current Masterdata definitions in the BAM Data Store can be better visualized in WebProtégé. Protégé is a software used to work and develop ontologies, but our aim is simpler: we want a tool to visualize the current Masterdata definitions, the relationships between object types, and the defined property types. This software allows us to export the Masterdata into a ontology-friendly format like owl, rdf/xml, or ttl. It also interfaces easily with Github pages and [WebVOWL](https://github.com/VisualDataWeb/WebVOWL).

If you are interested about working collaboratively on the generic BAM Masterdata definitions or on visualizing the current Masterdata definitions, you can contact us and request access to our WebProtégé project.