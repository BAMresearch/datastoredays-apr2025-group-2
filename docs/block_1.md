
This part of the Data Store Days consists of a slide presentation: Download here. (TODO: add link for slides) We also refer to the [Data Store Wiki](https://datastore.bam.de/en/home) for all the definitions of the main concepts in openBIS and in the Data Store project.

## BAM Data Model and Masterdata definitions

OpenBIS and the data therein is organized following a folder-like structure space/project/collection/object_types/datasets. The **object types** and **datasets** are the main data structures used in openBIS. The object types represent tangible and intangible entities and their attached metainformation. The datasets can also contain metainformation and attached files. The data model in openBIS is defined by the object types and its **assigned properties**, plus the parent-child relationships created between the objects. The assigned properties have different datatypes: they can be strings, floats, etc., and they can also be **controlled vocabularies**. A controlled vocabulary is an enumeration of strings that the assigned property can take. 

We call **Masterdata** (or schema) to all the defined object types, property types, and vocabularies without the actual data populating the assigned properties and without the parent-child relationships. These parent-child relationships allows to connect object types hence representing the full workflow done during the scientific activity.

For example, `Instrument` and `Sample` are object types with a set of assigned properties like `name`, `manufacturer`, `device_model_name`, or `physical_state`, to name a few. `physical_state` is a controlled vocabulary so it can only be defined as `"solid"`, `"liquid"`, and `"gaseous"`. Both object types can be connected via a parent-child relationship, representing perhaps a characterization measurement of an instrument over a synthesized sample.

<div class="click-zoom">
    <label>
        <input type="checkbox">
        <img src="../assets/example-workflow-objects.drawio.svg" alt="Example of an object type and its assigned properties and the workflow which can be constructed in openBIS." width="90%" title="Click to zoom in">
    </label>
</div>

In openBIS, we also distinguish between the ELN and the Inventory components in order to differentiate between objects which exists in a laboratory and can be reused by different team members (Inventory objects) from the actual objects used during the scientific workflow (ELN objects). Despite this distinction, the data model can be defined using objects from both components, so the main concepts of the software apply indistinctly.

## Represent your research workflow in the Data Store

In order to represent your workflow in the Data Store, you need to:

1. Identify the tangible and intangible object types used in the workflow.
2. Explore the currently defined object types and their assigned properties to check if any of them align with the object types that you need to represent your workflow.
3. For those cases where you do not find an object type already defined, extend the current Masterdata (see [Block II - BAM Masterdata](block_2.md)).
4. Once the Masterdata contains all the object types that you need, populate your schema with the actual data (see [Block III - Representing workflows in the BAM Data Store](block_3.md)).

In this block, we will focus on the initial two points. For this example, we will assume that all the object types we need are already defined in the current BAM Masterdata so that we do not need to extend it.


### Identify your object types

You can either use a piece of paper or the drawio document (TODO: add link) that we prepared in the Data Store team.

We are going to construct a [directed acyclic graph (DAG)](https://hazelcast.com/foundations/distributed-computing/directed-acyclic-graph/) representing the workflow in your research. As an example, let's consider we have:

<div class="click-zoom">
    <label>
        <input type="checkbox">
        <img src="../assets/example-xrd-workflow.drawio.svg" alt="Example of an XRD characterization workflow." width="90%" title="Click to zoom in">
    </label>
</div>

We can identify the elements in this workflow with the Inventory and the ELN components. Furthermore, we could identify them with already existing Masterdata and assign the corresponding properties to each object.

<div class="click-zoom">
    <label>
        <input type="checkbox">
        <img src="../assets/example-xrd-workflow-generic.drawio.svg" alt="Example of an XRD characterization workflow with generic BAM Masterdata." width="89%" title="Click to zoom in">
    </label>
</div>


The difference between both workflows is that the second one uses object types already defined in BAM Masterdata: `Chemical`, `Instrument`, `Experimental Step`, and `Sample`. Visit [bam-masterdata](https://github.com/BAMresearch/bam-masterdata) in Github to explore the current Masterdata.

??? note "Improving the users experience"
    An important part of our efforts right now is to improve the experience of exploring the current BAM Masterdata. Our plan is to use WebVOWL or WebProtégé as a tool for visualizing the object types, their properties, and the inheritance relationships between them. See [Block II - Collaborate and define generic entity types](block_2.md/#collaborate-and-define-generic-entity-types) for more information.

??? warning "Real case scenario"
    This example is actually too simplistic. In reality, the chances that the current BAM Masterdata does not have the object types and their assigned properties defined as you need are high. This situation also depends about how fine-grained you want to defined your workflow, and the requirements of your project. A good scientific practice is to represent your workflows as minimal as possible while keeping reproducibility maximize. And as in any data modelling project, we aim to define object types which are generic enough for any field of Materials Science Engineering to use the definitions and extend them for specific use-cases.