# Simple command line utilities 

## Install

- create a folder and run $ git clone this_repo_url
- make a python virtual environment (optional but recommended)
- cd repo_dir
- pip install -e .


## List of command mapping

                'cli_convert_jupyter = cli_code.cli_convert_ipynb:main',
                'cli_env_autoinstall = cli_code.cli_env_autoinstall:main',
                'cli_github_search = cli_code.cli_github_search:main',
                'cli_env_module_parser = cli_code.cli_module_parser:main',
                'cli_download=cli_code.cli_download:main',
                'cli_repo_check = cli_code.cli_check_repo:main',
                'cli_env_conda_merge = cli_code.cli_conda_merge:main',
                
                

## 1) Convert Notebook

convert all IPython notebooks inside a directory to python scripts and save them to another directory. This also tests python scripts for potential syntax errors.

Usage:

`convert_ipynb -i /path/to/notebooks -o path/to/python-scripts`

`-i` argument is required and `-o` is optional. If you don't specify an output path, all results will be saved in `py-scripts` directory.

## 2) Search Github

Search on github for keyword(s) with optional parameters to refine your search and get all results in a CSV file.

Usage:

`github_srch amazon scraper`

or refine your search

`github_srch keyword1 keyword2 -c >2019-11-10 -p 2019-11-01..2019-11-10 -o results`

These are optional arguments:

`-c` or `--created` specify the period of repository creation
`-p` or `--pushed` specify the period of pushing to repo
`-o` or `--output` specify the output folder for storing results (default value is `results`)

## 3) Auto Create Conda Environment

Automatically create conda virtual environment for a specified repository. It also autodetects all required packages and install them into the newly created environment.

Usage:

`env_autoinstall test -n notebook_cvt`

or

`env_autoinstall test -n notebook_cvt -py 3.6 -p tensorflow pandas`

`-n` or `--conda_env` specify name of our conda environment (if not specified, all required packages are installed in a default `test` environment)
`-py` or `--python_version` specify the python version of the target environment (default is 3.6)
`-p` or `--packages` specify any extra packages in addition to required ones to install (default is numpy)

## 4) Parse python modules

This Python code parser fetches variable information from the given source. The source may be given either as a .py filepath or as a directory path containing multiple .py source files.

The output is written to standard output in CSV format with the following columns; it starts with a header row: `filepath,function_or_class_name,variable_name,is_local`.
Function_or_class_name defaults to (global) when there's no enclosing function nor class.

Usage:

`mod_parser /path/to/module(s) or package(s) -o module_parsed.csv`

`-o` or `--output` option is optional and if not specified, results will be shown on stdout.

## 5) Easy Merge Conda Environmetns

This is a very simple piece of script that allows user to merge multiple YAML files generated by anaconda into a single unified YAML. The output of this script is two filetypes, one is the merged YAML file while the other is the requirements.txt file ready to be scanned by pip.

- Four yamls are saved in a directory:
  - One with default entries
  - One with versions of all entries
  - One with versions of only prioritized entries (defined in `getPiorityList()` function)
  - One with only the names of the packages
- Three .txts are saved in the same directory:
  - One with versions of all entries
  - One with versions of only prioritized entries (defined in `getPiorityList()` function)
  - One with only the names of the packages

Usage:

`conda_merge /path/to/env1.yaml /path/to/env2.yaml`

The output of the script will be saved in a directory

## 6) Get Github Repository and Check in a Newly created environment

Goal is to automate code check using a python script. This scripts do the following
For a given github repo url:

- Clone the repo with `git clone` and assign either a user specified name or repo's default name
- Build a conda env with packages required to use the repo
- Check if python code is running i.e., run main.py
- Generate signature docs of python source code

Usage:

`check_repo https://www.github.com/{username}/{reponame}.git`

`-o` or `--output` specify the name of target directory to clone the repo (default is {reponame})
`-n` or `--conda_env` specify name of our conda environment (if not specified, `{reponame}_env` will be used)
`-py` or `--python_version` specify the python version of the target environment (default is 3.6)
`-p` or `--packages` specify any extra packages in addition to required ones to install (default is numpy)

## 7) Automate Downloading from Dropbox, Google Drive, and Github

This script automates downloading of bulk files from github, google drive and dr0pbox. You can either provide a single url or a file containg multiple urls, one per line and an optional directory to store download results.

Usage:

`cli_download -u a_valid_url`

`cli_download -f /path/to/a_valid_urls_file -o my_download_dir`

`-u` or `--url` specify a valid url for the file to download
`-f` or `--file` specify path to a file containing a list of valid urls, one per line
`-o` or `--output` specify the output directory (default is `downloaded`)

What is a valid url?

- Github - Url of a _file_ in a github repo e.g., `https://github.com/Chhekur/amazon-scraper/blob/master/README.md`
- Google Drive - Share link of a file on google drive (share setting must be set to `anyone with the link`) e.g., `https://drive.google.com/file/d/1FPn4Q4PClobHgEU4DglyF2Xbs5Boe1r_/view?usp=sharing`
- Dropbox - TO BE TESTED, DON't OWN A DROPBOX ACCOUNT

TODOs

- Add an option for creating normal python virtual environment in cli_env_autoinstall module
- Check if the user specified environment already exits before creating a new one
- Resolve the problem `in cli_env_autoinstall's` function `get_missing` when package name is different from import name
- Find a better way to check root files, because sometimes running some scripts without required arguments also throws error
