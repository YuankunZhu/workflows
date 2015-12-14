# CWL Tools & Workflows


[![Build Status](https://travis-ci.org/common-workflow-language/workflows.svg?branch=master)](https://travis-ci.org/common-workflow-language/workflows)


"CWL Tools & Workflows" is the community's best practices for CWL tool and workflow descriptions. 

To submit a new workflow or tool description please make a pull request against this repository.

Any contribution submitted for inclusion in this repository shall be under the
terms and conditions of the Apache License version 2.0 (see LICENSE.txt).

## Pull request

Please follow these recommendations before pull request:

* One file per tool or per logical part of the tool (those with subcommands)
* CWL tool description filename should have the same name as the tool (Tool’s name *STAR* CWL tool description filename ```STAR.cwl```) if possible (no conflicts with existing files) and placed into *tools* directory
* If a tool has subcommands, like [samtools]( http://www.htslib.org/doc/samtools.html) or [bwa]( https://github.com/lh3/bwa/blob/master/README.md), use tool’s name dash subcommand name. For example, ```bwa-mem.cwl``` or ```samtools-index.cwl```
* Each CWL tool description should be provided with job and test files and placed into *test* directory
 * Job’s filename has to be suffixed CWL file basename plus *-job.json* (bwa-mem-job.json, samtools-index-job.json)
 * Test’s filename has to be suffixed CWL file basename plus *-test.yaml* (bwa-mem-test.yaml, samtools-index-test.yaml)
* Use docker for the tool your are describing
  * If the tool has subcommands and you are going to use the same Docker image, move the DockerRequirement class into separate file (```samtools-docker.cwl```) and *$import* it
  * If you are a maintainer of the Docker image, it is good to provide content of Dockerfile in class DockerRequirement (dockerFile)

Incomplete descriptions are welcome as long as they are usable. Generally sharing early & often is encouraged.

## SPARQL

For your convinience [Apache Jena Fuseki](https://jena.apache.org/documentation/fuseki2/) SPARQL server is provided. It automaticaly downloads new CWL tool descriptions converts them into XML/RDF format and makes available at https://sparql-test.commonwl.org. Each CWL tool becomes a graph that can be queried. 
Provided sample query all the graphs where foaf:name **"Dobin"** is present. 
```SPARQL
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX doap: <http://usefulinc.com/ns/doap#>

SELECT distinct ?file ?name
WHERE {
  graph ?file {
  ?P foaf:name ?name  .
  FILTER (regex(?name, "Dobin","i"))
  }
}
LIMIT 25
```


## Testing CWLs

Test directory includes:
* dm3_chr4.fa - Chromosome 4 of Drosophila genome
* dm3_chr4.gtf - Chromosome 4 RefSeq annotation file
* SRR1031972.fastq - The reduced raw reads file ( reads from Chromosome 4 only, RNA-Seq data)

To test tools run ```test/test.sh``` from the repository root.

