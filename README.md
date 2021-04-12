# Tutorial on how to use DVC and MLFlow <!-- omit in toc -->

# TOC <!-- omit in toc -->
- [1. Introduction](#1-introduction)
  - [Environment setup](#environment-setup)
  - [Initialize DVC](#initialize-dvc)
- [2. References](#2-references)

# 1. Introduction

## Environment setup

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

## Initialize DVC

This guide assumes you are already inside a git repo. If not, please initialize a git repo by doing `git init`

```bash
dvc init
```

# 2. References 

1. [Data Versioning and Reproducible ML with DVC and MLflow - Youtube](https://www.youtube.com/watch?v=W2DvpCYw22o&t)