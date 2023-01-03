# OTAR-BioCypher

This is a collection of BioCypher adapters and corresponding scripts for Open
Targets data. It is a work in progress.

## Installation
The project uses [Poetry](https://python-poetry.org). You can install it like
this:

```
git clone https://github.com/saezlab/OTAR-BioCypher.git
cd OTAR-BioCypher
poetry install
```

Poetry will create a virtual environment according to your configuration (either
centrally or in the project folder). You can activate it by running `poetry
shell` inside the project directory. Alternatively, you can use a different
package manager to install the dependencies listed in `pyproject.toml`.

## Open Targets target-disease associations
Target-disease association evidence is available from the Open Targets website
at https://platform.opentargets.org/downloads. The data can be downloaded in
Parquet format, which is a columnar data format that is compatible with Spark
and other big data tools. Currently, the data have to be manually downloaded 
(e.g. using the wget command supplied on the website) and placed in the
`data/ot_files` directory. The adapter was created using version 22.11 of the
data.

To transfer the columnar data to a knowledge graph, we use the adapter in
`adapters/target_disease_evidence_adapter.py`, which is called from the script
`scripts/target_disease_script.py`. This script produces a set of
BioCypher-compatible files in the `biocypher-out` directory. To create the
knowledge graph from these files, you can find a version of the neo4j-admin
import command for the processed data in each individual output folder, under
the file name `neo4j-admin-import-call.sh`, which simply needs to be executed in
the home directory of the target database. More information about the BioCypher
package can be found at https://biocypher.org.

Please note that, by default, the adapter will be in `test mode`, which means
that it will only process a small subset of the data. To process the full data,
you can set the `test_mode` parameter in the adapter to `False` (or remove it).

## Barrio-Hernandez et al. 2021 graph dump
Barrio-Hernandez and colleagues used interaction data from the Open Targets
platform to implement their method of network expansion 
(https://www.biorxiv.org/content/10.1101/2021.07.19.452924v1). A dump file of
the Neo4j knowledge graph they used is available at
http://ftp.ebi.ac.uk/pub/databases/intact/various/ot_graphdb/current/.

Once successfully installed, porting the OTAR graph can be attempted by running
a local (or remotely accesssible) instance of the OTAR graph dump in Neo4j and
executing the Python script at `scripts/barrio_hernandez_script.py`. This will
connect to BioCypher using the adapter (from
`adapters/barrio_hernandez_adapter.py`) and write the BioCypher-compatible
structured data to the `biocypher-out` directory. You can find a version of the
neo4j-admin import command for the processed data in each individual output
folder, under the file name `neo4j-admin-import-call.sh`, which simply needs to
be executed in the home directory of the target database. More information about
the BioCypher package can be found at https://biocypher.org.
