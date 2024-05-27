_These administration instructions are adapted from pymatgen, credit to S. P. Ong
and the pymatgen dev team_

Introduction
============

This documentation provides a guide for beep administrators. The following 
assumes you are using miniconda or Anaconda.

Releases
========

The general procedure to releasing beep comprises the following steps:

1. Wait for all unittests to pass on CI.
2. Update and edit change log.
3. Release PyPI versions + doc.

Initial setup
-------------

Beep uses `invoke <http://www.pyinvoke.org/>`_ to automate releases. Invoke
tasks are contained in tasks.py and can be performed using `invoke TASK-NAME`.
Note that task names replace underscores '_' with hyphens '-', e. g.
update_changelog -> `invoke update-changelog`

Create an environments for py38 using conda

	conda create --yes -n py38 python=3.8

For each env, install some packages using conda followed by dev install for 
pymatgen

	conda activate py38
	git clone https://github.com/TRI-AMDD/beep
	cd beep
	pip install -r requirements.txt
	pip install invoke twine
	python setup.py develop

Add your PyPI username and password and GITHUB_RELEASE_TOKEN into your 
environment.  Register for PyPI on pypi.org to get a username/password there.
For the github token, go to github.com->Settings->Developer Settings->Personal 
access tokens.  You should be able to click only the public_repo box for your
token.

	export TWINE_USERNAME=PYPIUSERNAME
	export TWINE_PASSWORD=PYPIPASSWORD
	export GITHUB_RELEASES_TOKEN=TOKEN_YOU_GET_FROM_GITHUB


Doing the release
-----------------

Ensure appropriate environment variables are set including `TWINE_USERNAME`,
`TWINE_PASSWORD` and `GITHUB_RELEASES_TOKEN`.

First update the change log. The autogenerated change log is simply a list of 
commit messages since the last version.  Make sure to edit the log for brevity
and to attribute significant features to appropriate developers:

    conda activate py38
    invoke update-changelog

Then, do the release with the following sequence of commands (you can put them 
in a bash script in your PATH somewhere):

    conda activate py38
    invoke release --notest
    conda deactivate

Double check that the releases are properly done on Pypi. If you are releasing
on a Mac, you should see a beep.version.tar.gz and one wheel (py38). 