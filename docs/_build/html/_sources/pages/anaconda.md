# [Anaconda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html)

## [Installation guide](https://conda.io/projects/conda/en/latest/user-guide/install/linux.html)

* Download the installer. Find the latest version for your os and get it. For my cases it was `wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh`
* Install using `bash Anaconda-latest-Linux-x86_64.sh`
* To update existing version `conda update conda`

## [Getting started](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html)
* Check if you have conda `conda --version`
* By default conda creates an environment called `base` with latest versions.
	* Activate it with `source /u/mi3se/anaconda3/bin/activate` then `conda activate base`. Sometimes you might need `conda init bash` before you can run `conda activate base`.
* You can add this directly to your vscode using `F1->Select interpreter->Find` if vscode doesn't recognise itself.
* Create a new environment named "snakes" that contains Python 3.9, `conda create --name snakes python=3.9`
* Activate the new environment: `conda activate snakes`
* Verify that the snakes environment has been added and is active `conda info --envs`
* Check is a package is installed `conda search beautifulsoup4`
* If not install it `conda install beautifulsoup4`
* Check to see if the newly installed program is in this environment `conda list`

## Conda channels 
https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-channels.html

* [solving-environment-failed-with-initial-frozen-solve-retrying-with-flexible-solve](https://exerror.com/solving-environment-failed-with-initial-frozen-solve-retrying-with-flexible-solve/#:~:text=Second%20solution%20is%20Just%20set,false%20and%20my%20error%20solved.&text=with%20flexible%20solve-,To%20Solve%20Solving%20environment%3A%20failed%20with%20initial%20frozen%20solve.,your%20error%20will%20be%20solved.)
	* conda config -–set channel_priority false

* See current channels `conda config --show channels`
* To make conda install the newest version of a package in any listed channel `conda config -–set channel_priority false`
* add a new channel with highest priority `conda config --add channels new_channel`, lowest priority `conda config --append channels new_channel`.
* * Default channel might not find some packages. Add conda-forge `conda config --add channels conda-forge`
* check channel priorities `conda config --describe channel_priority`

## Virtual environment
```
conda env list

# https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
# Create the environment from the environment.yml file
conda env create -f environment.yml

# Export your active environment to a new file
conda env export > environment.yml

#remove an environment
conda remove --name myenv --all

# verify it removed
conda info --envs

# create requirments.txt, https://stackoverflow.com/questions/50777849/from-conda-create-requirements-txt-for-pip3
conda list -e > requirements.txt
# can be used to create a conda virtual environment with
conda create --name <env> --file requirements.txt

# or using pip
pip freeze > requirements.txt
# In an activated conda environment I had to use
pip list --format=freeze > requirements.txt

# Then use the resulting requirements.txt to create a pip virtual environment:
python3 -m venv env
source env/bin/activate
pip install -r requirements.txt

# update the contents of your environment file
conda env update --prefix ./env --file environment.yml  --prune

```