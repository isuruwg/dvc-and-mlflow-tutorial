# Tutorial on how to use DVC and MLFlow <!-- omit in toc -->

# TOC <!-- omit in toc -->
- [1. Introduction](#1-introduction)
  - [1.1. Environment setup](#11-environment-setup)
  - [1.2. Initialize DVC](#12-initialize-dvc)
  - [Configure remote storage](#configure-remote-storage)
- [2. References](#2-references)

# 1. Introduction

## 1.1. Environment setup

Create virtual environment:

```bash
python -m venv venv
. ./venv/bin/activate
```

Install dependencies:
```bash
pip install mlflow
pip install dvc[all]
```

Freeze dependencies:
```bash
pip freeze > requirements.txt
```

## 1.2. Initialize DVC

This guide assumes you are already inside a git repo. If not, please initialize a git repo by doing `git init` or some other method.

Run: 

```bash
dvc init
```
This initializes dvc and also adds some of the newly created fils to git staging.

If you run `git status` it'll show something like: 

```bash
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .dvc/.gitignore
        new file:   .dvc/config
        new file:   .dvc/plots/confusion.json
        new file:   .dvc/plots/confusion_normalized.json
        new file:   .dvc/plots/default.json
        new file:   .dvc/plots/linear.json
        new file:   .dvc/plots/scatter.json
        new file:   .dvc/plots/smooth.json
        new file:   .dvcignore
```

## Configure remote storage

[dvc add reference](https://dvc.org/doc/command-reference/remote/add)

The remote storage in dvc can be `s3`, `gs`, `gdrive`, etc. For this example we'll use a local folder `~/tmp/dvc-storage` for simplicity. 

```bash
# We'll add a location inside a local ~/tmp folder for testing 
dvc remote add -d dvc-remote ~/tmp/dvc-storage
```

This adds the following to the .dvc/config file

```ini
[core]
    remote = dvc-remote
['remote "dvc-remote"']
    url = /home/isuru/tmp/dvc-storage
```

Let's now copy some data to a local folder `data/`

```bash
mkdir data
```

Copy an example file;

For this we'll copy [wine-quality.csv example file from mlflow](https://github.com/mlflow/mlflow/blob/master/examples/sklearn_elasticnet_wine/wine-quality.csv) into the [data/](data/) folder.

Add the file to dvc:

```bash
dvc add data/wine-quality.csv
```


# 2. References 

1. [Data Versioning and Reproducible ML with DVC and MLflow - Youtube](https://www.youtube.com/watch?v=W2DvpCYw22o&t)