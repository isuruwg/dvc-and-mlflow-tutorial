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

NEXT TODO: At 10:54 of video. Continue. [youtube](https://youtu.be/W2DvpCYw22o?t=654)

# 2. References 

1. [Data Versioning and Reproducible ML with DVC and MLflow - Youtube](https://www.youtube.com/watch?v=W2DvpCYw22o&t)