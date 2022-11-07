.. _setup-page:

*******************
Setup
*******************

Requirements
=================

Nextflow can be used on any POSIX compatible system (Linux, OS X, etc).
It requires Bash and `Java <https://www.oracle.com/java/technologies/downloads/>`_
11 (or later, up to 18) to be installed.

Optional requirements:

* `Docker <https://www.docker.com/>`_ [Docker] engine 1.10.x (or later) 
* `Singularity <https://github.com/sylabs/singularity>`_ 2.5.x (or later, optional) 
* `AWS Cloud 9 <https://aws.amazon.com/cloud9/>`_ environment properly configured (provided)

Steps to launch the AWS cloud 9 environment
============================================

For this tutorial, we will use the `AWS Cloud 9 virtual environment <https://aws.amazon.com/en/cloud9/>`_.

Below we included the list with all the users name created that you need together with the `CBCRG <https://www.crg.eu/en/cedric_notredame">`_ account ID to sign in the AWS console and launch the enviroment.

1. Open this address on your browser:  https://eu-west-1.console.aws.amazon.com/cloud9/home
2. Once you are in the AWS login page, select **IAM USER** and please fill the credentials below:

    .. Account ID: **885800555707**

    Account ID: To be provided

    IAM user name: (your username as listed above)

    Password: provided by the organization

3. When you access the AWS Cloud main page you will find your environment listed in the home page, just click on the **Open IDE** button and your environment will be launched.

Training materials download and environment setup
=================================================

Once you :ref:`launched the environment <Steps to launch the AWS cloud 9 environment>`, download the training materials
running the command below on the terminal window that you will find at the button of the IDE:

.. code-block:: console
    
        aws s3 sync s3://cbcrg-eu/atacseq-training-bovreg/setup ./setup/

        source setup/all.sh

        aws s3 sync s3://cbcrg-eu/atacseq-training-bovreg/config ./config/


.. $ aws s3 sync s3://cbcrg-eu/atacseq-training-bovreg/data ./data/

        $ aws s3 sync s3://cbcrg-eu/atacseq-training-bovreg/config ./config/

.. $ aws s3 sync s3://cbcrg-eu/atacseq-training-bovreg/data.tar.gz .

.. $ tar -xvf data.tar.gz

Nextflow Installation
=====================

Install the latest version of Nextflow copy & pasting the following snippet in a terminal window:

.. code-block:: console

    curl https://get.nextflow.io | bash
    mv nextflow ~/bin

Check the correct installation running the following command:

.. code-block:: console
    
    nextflow info

.. nf-core Installation
.. =====================

.. Install nf-core, a python package with helper tools provided by the nf-core community, using the command below:

.. .. code-block:: console

..     conda create -n py38_test python=3.8 nf-core -c bioconda -c conda-forge -y



.. -[nf-core/atacseq] Pipeline completed successfully-
.. Completed at: 05-Nov-2022 16:14:25
.. Duration    : 27m 12s
.. CPU hours   : 0.6
.. Succeeded   : 176

.. ANAIDR CONFIGURATION THE LA PIPELINE




.. https://eu-west-1.console.aws.amazon.com/cloud9/home

.. s3 bucket atacseq-training-bovreg

