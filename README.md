# Tutorial on how to use DVC and MLFlow <!-- omit in toc -->

# TOC <!-- omit in toc -->
- [1. An example using DVC and MLFlow](#1-an-example-using-dvc-and-mlflow)
  - [1.1. Environment setup](#11-environment-setup)
  - [1.2. Initialize DVC](#12-initialize-dvc)
  - [1.3. Configure remote storage](#13-configure-remote-storage)
  - [1.4. Copy some data and let dvc manage it](#14-copy-some-data-and-let-dvc-manage-it)
  - [1.5. Fetching data from remote](#15-fetching-data-from-remote)
  - [1.6. Modifying data](#16-modifying-data)
  - [1.7. Usage with MLFlow](#17-usage-with-mlflow)
- [2. References](#2-references)

# 1. An example using DVC and MLFlow 

This is based on [Data Versioning and Reproducible ML with DVC and MLflow - Youtube](https://www.youtube.com/watch?v=W2DvpCYw22o&t).

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

# Dependencies for the MLFlow, DVC combined tutorial
pip install scikit-learn
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

## 1.3. Configure remote storage

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

## 1.4. Copy some data and let dvc manage it

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
DVC creates a [wine-quality.csv.dvc](data/wine-quality.csv.dvc) file and also adds a [.gitignore](data/.gitignore) file inside the `data/` folder.

Let's also add a git tag to make it easier to track the data versions through git:

```bash
# Add and commit:
git add data/wine-quality.csv.dvc data/.gitignore
git commit -m "data: track"

git tag -a 'v1' -m 'raw data'
```

Now let's sync our local data with our remote:

```bash
dvc push
```

Let's now see what's inside our remote

```bash
ls -lR ~/tmp/dvc-storage/
```

This gives:

```bash
/home/user1/tmp/dvc-storage/:
total 4
drwxrwxr-x 2 user1 user1 4096 Apr 13 14:47 5d

/home/user1/tmp/dvc-storage/5d:
total 260
-r--r--r-- 1 user1 user1 264426 Apr 13 14:47 6f24258e3c50bb01a61194b5401f5d
```

Now we can remove the local data:

```bash
rm -rf data/wine-quality.csv
# Let's remove the data from .dvc/cache too
rm -rf .dvc/cache
```

## 1.5. Fetching data from remote

Since we deleted our data in the section above, we can bring them back from remote using `dvc pull`

```bash
dvc pull
```

## 1.6. Modifying data

```bash
# Let's do a simple modification to our csv file
sed -i '2,1001d' data/wine-quality.csv
# let's check dvc status
dvc status
```
`dvc status` command shows that our file was changed:

```bash
data/wine-quality.csv.dvc:                                            
        changed outs:
                modified:           data/wine-quality.csv
```

Now let's add the new data file to dvc:

```bash
dvc add data/wine-quality.csv
```

Now let's do a git commit:
```bash
git add data/wine-quality.csv.dvc
git commit -m "data: remove 1000 lines"
```

Let's also add a git tag:

```bash
git tag -a 'v2' -m 'removed 1000 lines'
```

Let's also push our data to remote storage:

```bash
dvc push
```

Also remember to push your tag to the remote repo by doing `git push --tags`.

## 1.7. Usage with MLFlow

For this, we'll use [this file from mlflow examples](https://github.com/mlflow/mlflow/blob/master/examples/sklearn_elasticnet_wine/train.py). 

This file has been downloaded to [src/](src/). ( [exact same version of file used can be accessed through this link](https://github.com/mlflow/mlflow/blob/d743a40426d5dedbde395a4e6bbdeebadbccd4dc/examples/sklearn_elasticnet_wine/train.py) )




# 2. References 

1. [Data Versioning and Reproducible ML with DVC and MLflow - Youtube](https://www.youtube.com/watch?v=W2DvpCYw22o&t)