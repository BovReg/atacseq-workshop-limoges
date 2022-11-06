.. _ATAC-seq_pipeline-page:

***************
nf-core/atacseq
***************

Introduction
============

nfcore/atacseq is a bioinformatics best-practice pipeline used for the analysis of ATAC-seq data. As discussed before, the pipeline is built using 
`Nextflow <https://www.nextflow.io/>`__, a workflow manager to run tasks across multiple compute infrastructures in a very portable manner. 
It uses Docker/Singularity containers making installation trivial and results highly reproducible. 
The `Nextflow DSL2 <https://www.nextflow.io/docs/latest/dsl2.html>`__ implementation of this pipeline uses one container per process which makes 
it much easier to maintain and update software dependencies. Where possible, these processes have been submitted to and installed from 
`nf-core/modules <https://github.com/nf-core/modules>`__ in order to make them available to all nf-core pipelines, and
to everyone within the Nextflow community. 

The 


nextflow run nf-core/atacseq -r dev -params-file ./config/nf-atacseq-params.json -profile docker -c ./config/nextflow.config -resume


.. Exercise
.. ********

.. Pull version ``3.8.1`` of the nf-core/atacseq pipeline, run it using the ``nf-core launch`` command and produce the ``nf-params.json``.

.. .. raw:: html

.. 	<details>
.. 	<summary><a>Solution</a></summary>

.. .. code-block:: console

.. 	nextflow pull nf-core/rnaseq -r 3.8.1
.. 	nf-core launch rnaseq -r 3.8.1

.. .. raw:: html

.. 	</details>
.. |
