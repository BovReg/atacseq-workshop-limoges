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

To launch the environment, you just need to sign in the AWS console with the 
`CBCRG <https://www.crg.eu/en/cedric_notredame">`_ account and specify
your (first) name as the IAM user. Below we included the list with all the users name created:

1. Open this address on your browser:  https://eu-west-1.console.aws.amazon.com/cloud9/home

2. Once you are in the AWS login page, select **IAM USER** and please fill the credentials below:

..    Account ID: **885800555707**

..    IAM user name: (your username as listed above)

..    Password: provided by the organization

Training materials download
===========================

Once you :ref:`launched the environment <Steps to launch the AWS cloud 9 environment>`, download the training materials running the command below
on the terminal window that you will find at the botton of the IDE:

.. code-block:: console
    
        $ aws s3 sync s3://cbcrg-eu/atacseq-training-bovreg/setup ./setup/

        $ source setup/all.sh








https://eu-west-1.console.aws.amazon.com/cloud9/home

s3 bucket atacseq-training-bovreg

