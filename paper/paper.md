---
title: 'BioHackEU24 report: Expanding FAIR database integration through elucidation and transformation of underlying graph schemas'
title_short: 'BioHackEU24 #18: BioDataFuse'
tags:
  - Knowledge Graph
  - Modular query
  - Biomedical data source
authors:
  - name: Yojana Gadiya*
    orchid: 0000-0002-7683-0452
    affiliation: 1
  - name: Javier Millan Acosta
    orcid: 0000-0002-4166-7093
    affiliation: 2
  - name: Jerven Bolleman
    orcid: 
    affiliation: 
  - name: Dominik Martinat
    orcid: 0000-0001-6611-7883
    affiliation: 
  - name: Harald Detering
    orcid: 0000-0002-0134-7618
    affiliation: 
  - name: Shuichi Kawashima
    orcid: 0000-0001-7883-3756
    affiliation: 
  - name: Toshiaki Katayama
    orcid: 	ktym@dbcls.jp	ktym
    affiliation: 
  - name: Tooba Abbassi-Daloii*
    orcid: 0000-0002-4904-3269
    affiliation: 2
affiliations:
  - name: Fraunhofer Institute for Translational Medicine and Pharmacology, Germany
    index: 1
  - name: Department of Translational Genomics, NUTRIM Institute of Nutrition and Translational Research in Metabolism, Maastricht University, the Netherlands; 
    index: 2
date: XX November 2024
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

# Introduction (@tabbassidaloii)
The integration of life science data from different biomedical resources has been a major challenge attributed to fragmented data sources, the use of multiple data formats, and the existence of multiple ontologies for a single context among others. To address this problem, we launched the [BioDataFuse (BDF) project](https://biodatafuse.org) [@AuthorSelfCitation:biodatafuse2023], which employs a modular framework for integrating data from different sources into context-specific knowledge graphs. Through this project, we have currently been able to integrate and harmonise data from ten databases. However, the integration of such resources requires a detailed understanding of underlying graph schemas. 
In this biohackathon, we aimed to streamline the data integration process such that any FAIR-compliant biological database can be easily converted to a graph. This robust process would involve two steps: first, understanding of the underlying graph schemas of data resources using the RDF-config (https://github.com/dbcls/rdf-config/) and VoID generator (https://github.com/JervenBolleman/void-generator) and second, the conversion of graph data into multiple compatible formats for improving accessibility and usability using G2G Mapper (https://g2gml.readthedocs.io/), LinkML (https://linkml.io/) and BDF (https://github.com/BioDataFuse/pyBiodatafuse). Moreover, we would test the resilience of the process by demonstrating the ease-of-integration of multiple data sources within the RDF Portal (https://rdfportal.org) and beyond. Through this test, we would essentially attract database owners to include additional biomedical data sources in BDF, thus expanding the applicability of their resource beyond the “yet-another-resource” paradigm.

## Goals for the biohackathon (@tabbassidaloii)
One of the main aims of our project was to enhance FAIR data integration by clarifying and transforming graph schemas. To achieve this we defined the tasks below
- Comparison and documentation of the synergies among different tools for extracting data models: RDF-Config, VoID generator and sheXer.
- Translate schemas into annotators to enable future automation.
We also wanted to improve the BioDataFuse functionality through:
- Expand the data model of BioDataFuse using resources from the RDF Portal.
- Integrate the output graph with LLM models.

# Comparision of schema extractor tools (@YojanaGadiya)

## Table 1. Overview of tools compared during the hacking week

|  | [VoID generator](https://github.com/JervenBolleman/void-generator) | [RDF-Config](https://github.com/JervenBolleman/void-generator) | [sheXer](https://github.com/JervenBolleman/void-generator) |
|---|---|---|---|
| **Summary / What does the tool do?** | Extracts statistics from an RDF endpoint or file | Automates SPARQL and schema generation; creates a GraphQL instance of an RDF graph |  |
| **Documentation** | [Incomplete](https://github.com/JervenBolleman/void-generator/blob/main/Tutorial.md) | [Present](https://github.com/JervenBolleman/void-generator/blob/main/Tutorial.md) | [Present](https://github.com/DaniFdezAlvarez/shexer/blob/master/README.md) |
| **Minimal requirement** | Requires an RDF file or SPARQL endpoint; Graph must have triples with rdf:type predicates | Requires Model.yaml and Prefix.yaml files | Requires a Turtle file |
| **Data model representation (output)** | Object class-based file detailing all classes and properties | SVG tree structure representing classes and properties | PNG image of classes and properties |
| **Process** | Automated | Semi-automated | Automated |
| **Interpretability of schema representation** | Difficult; requires programming knowledge to understand classes and properties | Easy; human-readable terms make tree structure easily understandable | Easy; graphical representation facilitates quick interpretation |
| **Ontologies** | Human-readable terms; more compatible with programming language formats | Human-readable terms | Uses ontology identifiers, making readability difficult |
| **Error logging** | Errors are not easily readable | Vague error logs | Errors are not readable or understandable; large images may be truncated |
| **Error reporting** | Through Git issues | Through Git issues |  |
| **Compiler** | Java-based; Native binary | Ruby-based | Python-based |
| **Limitation** | Quadratic runtime for generating files (e.g., IDSM, OrthoDB) | Not applicable for shape classes (e.g., Rhea) | Requires manual curation of input (Solution: integrate with VoID generators for curation) |

# Fatilitate the addition of new annotators to pyBiodatafuse (@tabbassidaloii)
Project #14


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

# Improvement in the pyBiodatafuse (@tabbassidaloii)
- adding the version to data extracted from IDSM
- improving the Bgee query
- 
# Discussion (@tabbassidaloii)
- standard data models 


# Future works
- learning from project #2, we will explore annotating the output knowledge graph with the [Croissant schema](https://docs.mlcommons.org/croissant/docs/croissant-spec.html)
- Support VCF (project 35)
- LLM integration to generate SPARQL queries from natural language (Project #4)


## Acknowledgements

This work was supported by ELIXIR, the research infrastructure for life-science data. We thank the organizers of the BioHackathon Europe 2024 for providing travel support for the project leads.

## References
