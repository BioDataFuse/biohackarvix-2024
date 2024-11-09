---
title: 'BioHackEU24 report: Expanding FAIR database integration through elucidation and transformation of underlying graph schemas'
title_short: 'BioHackEU24 #18: Graph schemas'
tags:
  - Knowledge Graph
  - Modular query
  - Biomedical data source
  - BioDataFuse
  - RDF-Portal
authors:
  - name: Javier Millan Acosta
    orcid: 0000-0002-4166-7093
    affiliation: 1
  - name: Shuichi Kawashima
    orcid: 0000-0001-7883-3756
    affiliation: 
  - name: Toshiaki Katayama
    orcid: 	ktym@dbcls.jp	ktym
    affiliation:
  - name: Jerven Bolleman
    orcid: 
    affiliation: 
  - name: Dominik Martinat
    orcid: 0000-0001-6611-7883
    affiliation: 
  - name: Harald Detering
    orcid: 0000-0002-0134-7618
    affiliation: 
  - name: Yojana Gadiya*
    orchid: 0000-0002-7683-0452
    affiliation: 4
  - name: Tooba Abbassi-Daloii*
    orcid: 0000-0002-4904-3269
    affiliation: 1
affiliations:
  - name: Department of Translational Genomics, NUTRIM Institute of Nutrition and Translational Research in Metabolism, Maastricht University, the Netherlands; 
    index: 1
  - name: Fraunhofer Institute for Translational Medicine and Pharmacology, Germany
    index: 4
date: 12 November 2024
cito-bibliography: paper.bib
event: BH24EU
biohackathon_name: "BioHackathon Europe 2024"
biohackathon_url:   "https://biohackathon-europe.org/"
biohackathon_location: "Barcelona, Spain, 2024"
group: Project 18
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/BioDataFuse/biohackarvix-2024/
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Abbassi-Daloii, T., Gadiya, Y. \emph{et al.}
---
	
#### corresponding authors: t.abbassidaloii\@maastrichtuniversity.nl and yojana.gadiya\@itmp.fraunhofer.de

#### * These authors contributed equally to this work

# Introduction
The integration of life science data across diverse biomedical resources remains a significant challenge, due to fragmented data sources, varying data formats, and the use of multiple ontologies to describe similar contexts. To address these issues, we launched the [BioDataFuse (BDF) project](https://biodatafuse.org) [@AuthorSelfCitation:biodatafuse2023], which applies a modular framework to harmonize and integrate data from various sources into context-specific knowledge graphs. To date, the BDF project has successfully integrated and harmonized data from ten databases, demonstrating the effectiveness of BDF's modular approach in creating unified and interoperable datasets.

However, achieving this level of integration requires a deep understanding of the underlying graph schemas from each data source. In this biohackathon, our goal was to further refine and streamline the data integration process, aiming to make it more seamless, adaptable, automatized, and FAIR-compliant. By creating a robust approach, we envision that any biological database adhering to FAIR principles could be readily integrated into the BDF framework and contribute to a unified data ecosystem.

## Goals for the biohackathon
Our main objective for this biohackathon was to enhance FAIR data integration by clarifying and transforming graph schemas. To achieve this, we outlined the following tasks:
- Compare and document the synergies among various tools for extracting data models, including RDF-Config, VoID generator, and sheXer.
- Translate schemas into annotators to enable future automation.

We also aimed to extend the capabilities of BioDataFuse by:
- Expanding the data model of BioDataFuse with resources from the RDF Portal.
- Integrating BioDataFuse output graphs with LLM models to explore new opportunities for automated reasoning and data interpretation.

# Comparative study of schema extractor tools

## Selection of schema extract tools
A number of data schema extractors exists. In this biohackathon, we looked at three such extraction tools: VoID-generator, RDF-Config, and sheXer **(Figure 1)**.

* [VoID-generator](https://github.com/JervenBolleman/void-generator): The tool enables the generation of [Vocabulary of Interlinked Datasets (VoID)](https://www.w3.org/TR/void/) files from an RDF turtle file. VoID is a metadata expression language for RDF graphs and allows for efficiently describing the distribution of data elements and their links in an RDF graph. Hence, by generating such VoID files, the VoID-generator tool enabled making the data and metadata around the graphs discoverable and interoperable.

* [RDF-Config](https://github.com/dbcls/rdf-config): It is a tool to visualize the RDF data structure in human-readable format based on high quality manual curation of underlying data models. This tool enabled making a structured representation of the underlying graph, thus allowing for RDF data management across resource.

* [sheXer](https://github.com/DaniFdezAlvarez/shexer): It is a library to automatic extract of [shape expressions (ShEx)](https://shex.io/) or [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/) for an RDF graph. ShEx is a schema language, while SHACL is W3C standard for describing an RDF graph. SheXer as a tool enables quality assurance of the underlying graph by checking compliance with pre-defined set of rules and constraints.

![Data model overview](../images/Dataschemas-biohack24.png)
<center>

**Figure 1: Understanding the utility of different.** Here we focused on VoID-generator, RDF-Config, and sheXer with their conversion from the RDF resource directly into a data model. Additionally, we found a tool to convert the VoID file to RDF-Config compliant files for data modelling. 

</center>

## Table 1. Pros and Cons of the schema tools

|  | [VoID generator](https://github.com/JervenBolleman/void-generator) | [RDF-Config](https://github.com/JervenBolleman/void-generator) | [sheXer](https://github.com/JervenBolleman/void-generator) |
|---|---|---|---|
| **Overview of the tool** | Extracts statistics from an RDF endpoint or file | Automates SPARQL and schema generation; creates a GraphQL instance of an RDF graph | Extracts ShEx and SHACL structure from an RDF graph; ensure quality assurance and rule compliance of a graph | 
| **Documentation** | [Incomplete](https://github.com/JervenBolleman/void-generator/blob/main/Tutorial.md) | [Present](https://github.com/JervenBolleman/void-generator/blob/main/Tutorial.md) | [Present](https://github.com/DaniFdezAlvarez/shexer/blob/master/README.md) |
| **Minimal requirement** | Requires an RDF file or SPARQL endpoint; Graph must have triples with `rdf:type` predicates | Requires `Model.yaml` and `Prefix.yaml` files | Requires a Turtle file |
| **Data model representation** | Object class-based file detailing all classes and properties | SVG tree structure representing classes and properties | PNG image of classes and properties |
| **Process** | Automated | Semi-automated | Automated |
| **Interpretability of schema representation** | Difficult; requires programming knowledge to understand organization of classes and properties | Easy; human-readable terms make the tree structure representation easily understandable | Easy; graphical representation facilitates quick interpretation |
| **Ontology representation in schema** | Human-readable terms (mapping ontology to labels); more compatible with programming language formats | Human-readable terms | Uses ontology identifiers, making readability difficult |
| **Error logging** | Errors are not easily readable | Errors are not easily readable or vague error logs | Errors are not readable or understandable; large images may be truncated |
| **Error reporting** | Through Git issues | Through Git issues | Through Git issues |
| **Compiler** | Java (Native binary) | Ruby | Python |
| **Limitation** | Quadratic runtime for generating files (e.g., [IDSM](https://idsm.elixir-czech.cz/), [OrthoDB](https://www.orthodb.org/)); Not applicable for shape subclasses (e.g., In [Rhea](https://www.rhea-db.org/), where compounds are sub-classified into products, reactants, etc.) | Requires manual curation of input (Potential solution: integrate with VoID generators for semi-automation) |

## Utilizing the schema tools

As shown in **Table 1**, each of the data schema tools are written in different programming languages. To enable easy use of these tool with minimal interpreter changes, we built a docker environment and a detailed step-by-step documentation on the utility of these tools with a locally deployed graph. The documentation is available on [Github](https://github.com/BioDataFuse/elixir_biohackathon_2024/tree/main/void2rdf-config#readme) and discussed in detail in the later sections.

# Facilitating the addition of new annotators to *pyBiodatafuse*

## Semi-automating annotation building with data schemas
To extend [pyBiodatafuse](https://github.com/BioDataFuse/pyBiodatafuse.git)’s functionality and support ongoing alignment with related projects, we identified a valuable integration opportunity with [**Biohackathon Project #14**](). This project discussed the generation of a tool named [sparql-void-to-python tool](https://github.com/TRIPLE-CHIST-ERA/sparql-void-to-python.git) which utilizes the VoID file to automatically generate Python APIs that include all classes and properties within an RDF database.

Building on this capability, we developed a "generic" template that enables the streamlined addition of new annotators to *pyBiodatafuse* **(Code 1)**. This template leverages on information provided by the Python API from sparql-void-to-python tool to facilitate the creation of database-specific annotators directly within the BDF framework. By adopting this approach, we anticipate simplifying and fast onboarding the integration of new RDF databases into BDF, making the process more efficient and consistent across different data sources.

```python
import datetime
import os
import warnings
from string import Template
from typing import Any, Dict, Tuple

import pandas as pd
from SPARQLWrapper import JSON, SPARQLWrapper
from SPARQLWrapper.SPARQLExceptions import SPARQLWrapperException
from tqdm import tqdm

from pyBiodatafuse.utils import (
    check_columns_against_constants,
    collapse_data_sources,
    get_identifier_of_interest,
)

# Pre-requisite:
VERSION_QUERY_FILE = os.path.dirname(__file__) + "/queries/wikipathways-metadata.rq"
DATABASE_SPARQL_DICT = {"wikipathways": "https://sparql.wikipathways.org/sparql"}
DATABSE_QUERY_IDENTIFER = "NCBI Gene"
QUERY_LIMIT = 25
QUERY_SPECIFIC_SPARQL_FILE = os.path.dirname(__file__) + "/queries/wikipathways-genes-pathways.rq"
GENE_INPUT_COL = "gene_id"
NEW_DATA_COL = "pathway_id"

# Unique inputs and outputs:
INPUT_OPTIONS = ["gene", "chemical", "disease"]


def read_sparql_file(file_path: str) -> str:
    """Read a SPARQL query file.

    :param file_path: the path to the SPARQL query file
    :returns: the content of the SPARQL query file
    """
    with open(file_path, "r") as fin:
        sparql_query = fin.read()

    return sparql_query


def check_endpoint(db: str) -> bool:
    """Check the availability of the a SPARQL endpoint.

    :param db: the database to query
    :returns: True if the endpoint is available, False otherwise.
    """
    sparql_query = read_sparql_file(VERSION_QUERY_FILE)

    sparql = SPARQLWrapper(DATABASE_SPARQL_DICT[db])

    sparql.setReturnFormat(JSON)
    sparql.setQuery(sparql_query)

    try:
        sparql.queryAndConvert()
        return True
    except SPARQLWrapperException:
        return False


def get_version(db: str) -> dict:
    """Get version of RDF graph.

    :param db: the database to query
    :returns: a dictionary containing the version information
    """
    sparql_query = read_sparql_file(VERSION_QUERY_FILE)

    sparql = SPARQLWrapper(DATABASE_SPARQL_DICT[db])
    sparql.setReturnFormat(JSON)

    sparql.setQuery(sparql_query)
    res = sparql.queryAndConvert()

    version = {"source_version": res["results"]["bindings"][0]["title"]["value"]}

    return version


def get_interactions(
    bridgedb_df: pd.DataFrame, db: str, input: str, input_identifier: str
) -> Tuple[pd.DataFrame, dict]:
    """Query interactions automatically for associated with entities.

    :param bridgedb_df: BridgeDb output for creating the list of gene ids to query
    :param db: the database to query
    :param input: the input type used to query the database
    :param input_identifier: the input identifier used to query the database
    :returns: a DataFrame containing the WikiPathways output and dictionary of the WikiPathways metadata.
    """
    assert db in DATABASE_SPARQL_DICT.keys(), f"{db} is not a valid database."

    assert input in INPUT_OPTIONS, f"{input} is not a valid input."

    # Check if the endpoint is available
    api_available = check_endpoint(db=db)

    if not api_available:
        warnings.warn(
            f"{db} SPARQL endpoint is not available. Unable to retrieve data.",
            stacklevel=2,
        )
        return pd.DataFrame(), {}

    # Step 1: Identifier mapping and harmonization
    data_df = get_identifier_of_interest(bridgedb_df, input_identifier)

    version = get_version(db=db)  # Get the version of the RDF graph

    # Step 2: Aggregating query to avoid the query limit
    gene_list = list(data_df["target"].unique())

    query_gene_lists = []
    if len(gene_list) > QUERY_LIMIT:
        for i in range(0, len(gene_list), QUERY_LIMIT):
            tmp_list = gene_list[i : i + QUERY_LIMIT]
            query_gene_lists.append(" ".join(f'"{g}"' for g in tmp_list))
    else:
        query_gene_lists.append(" ".join(f'"{g}"' for g in gene_list))

    # Step 3: Running the SPARQL query
    sparql_query = read_sparql_file(QUERY_SPECIFIC_SPARQL_FILE)

    start_time = datetime.datetime.now()  # Record the start time

    sparql = SPARQLWrapper(DATABASE_SPARQL_DICT[db])
    sparql.setReturnFormat(JSON)

    intermediate_df = pd.DataFrame()

    for gene_list_str in tqdm(query_gene_lists, desc=f"Querying {db}"):
        sparql_query_template = Template(sparql_query)
        substit_dict = dict(gene_list=gene_list_str)
        sparql_query_template_sub = sparql_query_template.substitute(substit_dict)

        sparql.setQuery(sparql_query_template_sub)

        res = sparql.queryAndConvert()

        res = res["results"]["bindings"]

        df = pd.DataFrame(res)
        for col in df:
            df[col] = df[col].map(lambda x: x["value"], na_action="ignore")

        intermediate_df = pd.concat([intermediate_df, df], ignore_index=True)

    end_time = datetime.datetime.now()  # Record the end time

    # Step 4: Checking if the query returned any results
    if GENE_INPUT_COL not in intermediate_df.columns:
        warnings.warn(
            f"There is no annotation for your input list in {db}.",
            stacklevel=2,
        )
        return pd.DataFrame(), {}

    # Step 5: Dtype conversion and renaming columns
    intermediate_df.rename(columns={GENE_INPUT_COL: "target"}, inplace=True)
    # Examples:
    # intermediate_df["pathway_gene_count"] = pd.to_numeric(intermediate_df["pathway_gene_count"])
    # intermediate_df["pathway_id"] = intermediate_df["pathway_id"].apply(lambda x: f"WP:{x}")
    intermediate_df = intermediate_df.drop_duplicates()

    # Step 6: Generating a metadata dictionary
    current_date = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    time_elapsed = str(end_time - start_time)  # Calculate the time elapsed
    metadata_dict: Dict[str, Any] = {
        "datasource": db,
        "metadata": version,
        "query": {
            "size": len(gene_list),
            "input_type": DATABSE_QUERY_IDENTIFER,
            "time": time_elapsed,
            "date": current_date,
            "url": DATABASE_SPARQL_DICT[db],
        },
    }

    # Step 7: Cataloging the outputs from the resource
    output_dict = {
        "pathway_id": str,
        "pathway_label": str,
        "pathway_gene_count": int,
    }

    # Step 8: Quality check for dtypes
    check_columns_against_constants(
        data_df=intermediate_df,
        output_dict=output_dict,
        check_values_in=list(output_dict.keys()),
    )

    # Step 9: Adding node and edge statistics to metadata
    num_new_nodes = intermediate_df[NEW_DATA_COL].nunique()  # Calculate the number of new nodes
    num_new_edges = intermediate_df.drop_duplicates(subset=["target", NEW_DATA_COL]).shape[
        0
    ]  # Calculate the number of new edges

    if num_new_edges != len(intermediate_df):
        warnings.warn(
            f"The intermediate_df in {db} annotatur should be checked, please create an issue on https://github.com/BioDataFuse/pyBiodatafuse/issues/.",
            stacklevel=2,
        )

    metadata_dict["query"]["number_of_added_nodes"] = num_new_nodes
    metadata_dict["query"]["number_of_added_edges"] = num_new_edges

    # Step 10: Integrating into main dataframe
    merged_df = collapse_data_sources(
        data_df=data_df,
        source_namespace=GENE_INPUT_COL,
        target_df=intermediate_df,
        common_cols=["target"],
        target_specific_cols=list(output_dict.keys()),
        col_name=db,
    )

    return merged_df, metadata_dict
```
<center>

**Code 1**: Generic annotator template in *pyBiodatafuse*.

</center>

# Using LLM to query BDF knowledge graph (@jmillanacosta)
Project #4, 
- concept:

```mermaid
graph TD
    A[Run VoID generator on RDF file or endpoint] --> B[Change directory to rdf_config in this repository]
    B --> |docker compose up -d to start Virtuoso instance and load dataset| C[Create a SHACL Prefix Set and upload to the triplestore]
    C --> |run *serve_rdf_config.sh* and access 0.0.0.0:8000/sparql/test-rdf-config.html| D[Generate RDF Config Files, point to localhost:8899/sparql]
    D --> |run *rdf-config --config . --senbero*| E[Retrieve tree structure]
    D --> |run *rdf-config --config . --schema > schema.svg*| F[Generate SVG diagram]
```

## Evaluating scope of GraphQL interface for RDF-Portal (@Toshiaki) 

Add info here on work done

Overall, the capability to integrate the GraphQL to RDF-Portal opens several opportunities:
* Enabling performing federated queries within the 150 or more data sources in RDF-Portal
* Opening a plugin support for BDF ingestion, expanding its querying capabilities

# Improvement of *pyBiodatafuse*
As part of ongoing efforts to enhance BioDataFuse, some improvements were made during the week:
- **Versioning of IDSM**: With discussion with IDSM developers, we were able to extract and add versioning to the data extracted from this endpoint for better traceability and consistency.
- **Optimized Bgee queries**: With SPARQL experts and Bgee developers, we were able to improve the efficiency of Bgee queries, reducing the query time significantly and enhancing overall performance.
- **Validation of BDF graph**: With data schema tools, we were able to quality check the BDF graph schema and assess the overall quality of our generated RDF graph.
- **Validation of BDF graph**: With data schema tools, we were able to quality check the BDF graph schema and assess the overall quality of our generated RDF graph.

# Improvement of *RDF-Portal* (@Toshiaki and @Shuichi)


# Discussion
- to add points on the discussion with Toshiaki here


# Future Work
With the successful building and understanding of schema alongside cross-project overlaps, we identified a number of future work plan.

### Ingesting more data in BDF

- **Orphanet annotator**: In collaboration with [David Lagorce]() (Orphanet), we aim to develop a specialized annotator for integrating Orphanet data within BDF.

- **VCF file support and variant entity expansion**: Extend BioDataFuse to support variant entities through VCF file integration. This will be explored in collaboration with [Sarang Bhutada]() and [Vibhor Gupta]() (Project #32), and [Alexandrina Bodrug](), enriching BioDataFuse’s handling of variant data.

- **SPARQL-API for annotators**: Leverage the `sparql-void-to-python` package created by Vincent Emonet and Jerven Bolleman (Project #14) to streamline the creation of new BioDataFuse annotators, facilitating the incorporation of diverse RDF data sources.

- **Usecase with Bgee**: Collaborate with Harald Detering (Bgee) to create a use case demonstrating how Bgee gene expression values can be used in spcific research setup.

### Integration with ML models

- **Croissant schema annotation**: Building on insights from [Núria Queralt Rosinach]() (Project #2), we will explore converting and adapting BDF output knowledge graph with the [Croissant schema](https://docs.mlcommons.org/croissant/docs/croissant-spec.html), enhancing data standardization and downstream data analysis with ML tools from Huggingface.

- **LLM integration**: By collaborating with [Tarcisio Mendes De Farias]() (Project #4) to enhance knowledge graph interpretability and usability for a wider range of applications with LLM. Specifically, converting natural language to SPARQL queries for the context specific BDF graph, allowing a chatbot plugin on our graph. 

### Integration with RDF-Portal

- **Using GraphQL interface**: Being the downstream users of the GraphQL interface, we would collaborate with [Toshiaki Katayama]() and [Shuichi Kawashima]() to explore opportunities to build annotators in BDF for RDF-Portal.

## Acknowledgements
This work was supported by ELIXIR, the European research infrastructure for life-science data. We would also like to express our gratitude to the organizers of BioHackathon Europe 2024 for providing travel support for one of the project leads and a participant.

## References
