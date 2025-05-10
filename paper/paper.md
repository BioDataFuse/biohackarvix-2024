---
title: 'BioHackEU24 report: Expanding FAIR database integration through elucidation and transformation of underlying graph schemas'
title_short: 'BioHackEU24 #18: Graph schemas'
tags:
  - Knowledge Graph
  - Graph schema
  - Biomedical data source
  - BioDataFuse
  - RDF Portal
authors:
  - name: Javier Millan Acosta
    orcid: 0000-0002-4166-7093
    affiliation: 1
  - name: Shuichi Kawashima
    orcid: 0000-0001-7883-3756
    affiliation: 2
  - name: Toshiaki Katayama
    orcid: 0000-0003-2391-0384
    affiliation: 2
  - name: Jerven Bolleman
    orcid: 0000-0002-7449-1266
    affiliation: 3
  - name: Dominik Martinat
    orcid: 0000-0001-6611-7883
    affiliation: 4
  - name: Harald Detering
    orcid: 0000-0002-0134-7618
    affiliation: 5
  - name: Jose Emilio Labra Gayo
    orcid: 0000-0001-8907-5348
    affiliation: 6
  - name: Yojana Gadiya*
    orchid: 0000-0002-7683-0452
    affiliation: 7
  - name: Tooba Abbassi-Daloii*
    orcid: 0000-0002-4904-3269
    affiliation: 1

affiliations:
  - name: Department of Translational Genomics, NUTRIM Institute of Nutrition and Translational Research in Metabolism, Maastricht University, the Netherlands
    index: 1
  - name:  Database Center for Life Science, Joint Support - Center for Data Science Research, Research Organization
    index: 2
  - name: Swiss-Prot group, Swiss Institute of Bioinformatics, Switzerland
    index: 3
  - name: University of Oviedo, Spain
    index: 6
  - name: Fraunhofer Institute for Translational Medicine and Pharmacology, Germany
    index: 7

date: 12 November 2024
cito-bibliography: paper.bib
event: BH24EU
biohackathon_name: "BioHackathon Europe 2024"
biohackathon_url: "https://BioHackathon-europe.org/"
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

However, achieving this level of integration requires a deep understanding of the underlying graph schemas from each data source. In this BioHackathon, our goal was to refine further and streamline the data integration process, aiming to make it more seamless, adaptable, automatized, and FAIR-compliant. By creating a robust approach, we envision that any biological database adhering to FAIR principles could be readily integrated into the BDF framework and contribute to a unified data ecosystem.

## Goals for the BioHackathon

Our main objective for this BioHackathon was to enhance FAIR data integration by clarifying and transforming graph schemas. To achieve this, we outlined the following tasks:

- Compare and document the synergies among various tools for extracting data models, including RDF-config, VoID generator, and sheXer.
- Translate schemas into annotators to enable future automation.

We also aimed to extend the capabilities of BioDataFuse by:

- Expanding the data model of BioDataFuse with resources from the RDF Portal.
- Integrating BioDataFuse output graphs with LLM models to explore new opportunities for automated reasoning and data interpretation.

# Comparative study of schema extractor tools

## Selection of schema extract tools

A number of data schema extractors exist. In this BioHackathon, we looked at three such extraction tools: VoID-generator, RDF-config, and sheXer **(Figure 1)**.

- [VoID-generator](https://github.com/JervenBolleman/void-generator): The tool enables the generation of [Vocabulary of Interlinked Datasets (VoID)](https://www.w3.org/TR/void/) files from an RDF turtle file or a SPARQL endpoint. VoID is a metadata expression language for RDF graphs and allows for efficiently describing the distribution of data elements and their links in an RDF graph. Hence, by generating such VoID files, the VoID-generator tool enabled making the data and metadata around the graphs discoverable and interoperable.

- [RDF-config](https://github.com/dbcls/rdf-config): It is a tool to describe and visualize the RDF data structure in a human-readable format based on high quality manual curation of underlying data models. This tool enables making a structured representation of the underlying graph, thus allowing for RDF data management across resources.

- [sheXer](https://github.com/DaniFdezAlvarez/shexer): It is a library to automatic extract of [shape expressions (ShEx)](https://shex.io/) or [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/) from an RDF graph or SPARQL endpoint.
ShEx is a concise and human-readable schema language for RDF,
while SHACL is a W3C recommendation defined as an RDF vocabulary that can be used to validate and define constraints on RDF data.
SheXer as a tool enables quality assurance of the underlying graph by checking compliance with a pre-defined set of rules and constraints.

![Data model overview](../images/Dataschemas-biohack24.png)
<center>

**Figure 1: Understanding the utility of tools.** Here we focused on VoID-generator, RDF-config, and sheXer with their conversion directly from the RDF resource into a data model. Additionally, we used a tool to convert the VoID file to RDF-config compliant files for data modeling.

</center>

## Table 1. Pros and Cons of the schema tools

|  | [VoID generator](https://github.com/JervenBolleman/void-generator) | [RDF-config](https://github.com/dbcls/rdf-config) | [sheXer](https://github.com/DaniFdezAlvarez/shexer) |
|---|---|---|---|
| **Overview of the tool** | Extracts statistics from an RDF endpoint or a file | Automates SPARQL and schema diagram generation | Extracts ShEx and SHACL structure from an RDF graph |
| **Documentation** | [Incomplete](https://github.com/JervenBolleman/void-generator/blob/main/Tutorial.md) | [Present](https://github.com/dbcls/rdf-config/tree/master/doc) | [Present](https://github.com/DaniFdezAlvarez/shexer/blob/master/README.md) |
| **Minimal requirement** | Requires an RDF file or SPARQL endpoint; Graph must have triples with `rdf:type` predicates | Requires manually curated `model.yaml` and `prefix.yaml` files | Requires a Turtle file or a SPARQL endpoint |
| **Data model representation** | Object class-based file detailing all classes and properties | A set of tree structures representing classes and properties in YAML which can be exported as SVG | A SHACL or ShEx schema, optionally a UML diagram in PNG representing the shape graph |
| **Process** | Automated | Semi-automated | Automated, and optionally one can change the threshold used to accept patterns in the graph as shapes and I/O settings |
| **Interpretability** | Difficult; requires programming knowledge to understand organization of classes and properties | Easy; human-readable terms and graphical representation make the graph structure easily understandable | Easy; graphical representation facilitates quick interpretation |
| **Readability** | Human-readable terms (mapping ontology to labels); more compatible with programming language formats | Human-readable terms assigned for classes and properties | The diagrams and graphs are rendered with their URIs and no labels |
| **Error reporting** | Through Git issues | Through Git issues | Through Git issues |
| **Implementation** | Java (Native binary) | Ruby | Python |
| **Limitation** | Quadratic runtime for generating files (e.g., [IDSM](https://idsm.elixir-czech.cz/), [OrthoDB](https://www.orthodb.org/)); Not applicable for shape subclasses (e.g., In [Rhea](https://www.rhea-db.org/), where compounds are sub-classified into products, reactants, etc.) | Requires manual curation of input (Potential solution: integrate with VoID generators for semi-automation, for which a prototype exists) | The current algorithm is unable to detect specific patterns within the data; however, the developers are aware of this limitation. |

## Utilizing the schema tools

As shown in **Table 1**, each of the data schema tools is written in different programming languages. To enable easy use of these tools with minimal interpreter changes, we built a docker environment and detailed step-by-step documentation on the utility of these tools with a locally deployed graph. The documentation is available on [Github](https://github.com/BioDataFuse/elixir_BioHackathon_2024/tree/main/void2rdf-config#readme) and discussed in detail in the later sections.

# Facilitating the addition of new annotators to *pyBiodatafuse*

## Semi-automating annotation building with data schemas

To extend [pyBiodatafuse](https://github.com/BioDataFuse/pyBiodatafuse.git)’s functionality and support ongoing alignment with related projects, we identified a valuable integration opportunity with [**BioHackathon Project #14**](). This project developed a tool named [sparql-void-to-python tool](https://github.com/TRIPLE-CHIST-ERA/sparql-void-to-python.git) which utilizes the VoID file to automatically generate Python APIs that include all classes and properties within an RDF database and we collaborated the authors of [rudof](https://rudof-project.github.io/) which are implementing a similar feature.

Building on this capability, we developed a "generic" template that enables the streamlined addition of new annotators to *pyBiodatafuse* (available on [Github](https://github.com/BioDataFuse/pyBiodatafuse/blob/main/src/pyBiodatafuse/annotators/rdf_annotator_template.py)). This template leverages information provided by the Python API from the sparql-void-to-python tool to facilitate the creation of database-specific annotators directly within the BDF framework. By adopting this approach, we anticipate simplifying and fast onboarding the integration of new RDF databases into BDF, making the process more efficient and consistent across different data sources.

## Improvement of *pyBiodatafuse*

As part of ongoing efforts to enhance BioDataFuse, some improvements were made during the week:

- **Versioning of IDSM**: With discussion with IDSM developers, we were able to extract and add versioning to the data extracted from this endpoint for better traceability and consistency.
- **Optimized Bgee queries**: With SPARQL experts and Bgee developers, we were able to improve the efficiency of Bgee queries, reducing the query time significantly and enhancing overall performance.
- **Validation of BDF graph**: With data schema tools, we were able to quality check the BDF graph schema and assess the overall quality of our generated RDF graph. The current implementation of the `BDFGraph()` class wraps `shexer` methods to generate both ShEx and SHACL graphs representing its schema, as showcased in the [example workflow notebook]().

## Improvement of *RDF Portal*

The [RDF Portal](http://rdfportal.org/) [@AuthorSelfCitation:rdfportal], operated by the Database Center for Life Science (DBCLS) in Japan, is a repository of knowledge graphs in life sciences that hosts over 60 key bioinformatics databases and ontologies. In this BioHackathon, we explored methods to enable the use of RDF Portal data within the BDF framework. By leveraging RDF-config, SPARQL queries corresponding to each dataset in the RDF Portal can be automatically generated. Moreover, RDF-config can produce configuration files for [Grasp](http://github.com/dbcls/grasp/), a tool developed by DBCLS that exposes SPARQL endpoints as a GraphQL interface.

We created concise `model.yaml` configuration files for selected databases to be used with RDF-config, and subsequently constructed a [GraphQL endpoint](https://dx.dbcls.jp/eubh24/grasp) for this BioHackathon using Grasp. Although individual RDF datasets often have highly complex schemas, this curated GraphQL endpoint allows for intuitive cross-database queries through simplified syntax. For example:

```
query {
  ChEMBL(id: "CHEMBL941") {
    label
    smiles
    atc
    alogp
    pchembl
    ChEBI {
      charge
    }
    UniProt {
      mnemonic
      label
      organism
    }
  }
}
```

This effort lays the groundwork for opening plugin support for BDF ingestion and expanding its querying capabilities.

# Discussion

In this work, we identified the importance of establishing standards for identifiers and data schemas to enable the seamless integration of multiple databases. A key challenge lies in developing interoperable and widely applicable data models, which requires not only technical solutions but also collaborative efforts among domain experts. To address this, we emphasize the need to foster a community of specialists in data schema modeling who can collaboratively define best practices and contribute to the evolution of shared standards. Furthermore, we found that enriching data schema models with clear descriptions for classes and properties significantly enhances the interpretability of the schema—an aspect that becomes increasingly important in the context of large language model (LLM) applications.

Project #4 highlighted the potential of using custom LLMs, fine-tuned to generate SPARQL queries for open data endpoints, as a promising approach to bridge the gap between data consumers and RDF-based resources. In order to minimize errors and improve the quality of generated queries, such models can be fine-tuned using formal schema representations, such as SHACL or ShEx, along with curated pairs of SPARQL and natural language queries. This strategy not only increases compatibility with individual datasets but also lays the groundwork for scalable, intelligent query interfaces for semantic web resources.

# Future Work

With the successful building and understanding of schema alongside cross-project overlaps, we identified a number of future work plans.

### Ingesting more data in BDF

- **Orphanet annotator**: In collaboration with [David Lagorce]() ([Orphanet](https://www.orpha.net/)), we aim to develop a specialized annotator for integrating the Orphanet data within BDF.

- **VCF file support and variant entity expansion**: Extend BioDataFuse to support variant entities through VCF file integration. This will be explored in collaboration with [Sarang Bhutada]() and [Vibhor Gupta]() (Project #32), and [Alexandrina Bodrug](), enriching BioDataFuse’s handling of variant data.

- **SPARQL-API for annotators**: Leverage the `sparql-void-to-python` package created by [Vincent Emonet]() and Jerven Bolleman (Project #14) to streamline the creation of new BioDataFuse annotators, facilitating the incorporation of diverse RDF data sources.

- **Usecase with Bgee**: Collaborate with Harald Detering (Bgee) to create a use case demonstrating how Bgee gene expression values can be used in a specific research setup.

### Integration with ML models

- **Croissant schema annotation**: Building on insights from [Núria Queralt Rosinach]() (Project #2), we will explore converting and adapting the BDF output knowledge graph with the [Croissant schema](https://docs.mlcommons.org/croissant/docs/croissant-spec.html), enhancing data standardization and downstream data analysis with ML tools from Huggingface.

- **LLM integration**: By collaborating with [Tarcisio Mendes De Farias]() (Project #4) to enhance knowledge graph interpretability and usability for a wider range of applications with LLM. Specifically, converting natural language to SPARQL queries for the context-specific BDF graph, allowing a chatbot plugin on our graph.

### Integration with RDF Portal

- **Using GraphQL interface**: Being the downstream users of the GraphQL interface, we would collaborate with [Toshiaki Katayama]() and [Shuichi Kawashima]() to explore opportunities to build annotators in BDF for RDF Portal.

## Acknowledgements

This work was supported by ELIXIR, the European research infrastructure for life-science data. We would also like to express our gratitude to the organizers of BioHackathon Europe 2024 for providing travel support for one of the project leads and a participant.

## References
