---
aliases:
- /machine-learning/2020/06/29/tracking-data-model-using-dvc
categories:
- machine-learning
date: '2020-06-29'
description: DVC as tool for model and data versioning and create reproducible ML
  project.
layout: post
title: Tracking Data and Model in Machine Learning projects
toc: true

---

## Why should you track everything

- Difference between software engineering and machine learning projects
- code, config, dependencies vs code, data, model, config, dependencies
- Myriad of Data files (raw, intermediate)
- Not in repository
- Reproducible ML models targeting metrics [Experiments Link]
- Tuning and Experimentation tracking
- Model Deployment & Revert

## DVC

- DVC is built to make ML models shareable and reproducible. It is designed to handle large files, data sets, machine learning models, and metrics as well as code.

- Experiments are tracked by combining the Code + Data files
- Data files
  - Local cache
  - Data remotes in S3, SSH etc
- Metrics per experiment
- Pipelines

### Install DVC on Fedora/RHEL/CentOS

``` bash
sudo wget https://dvc.org/rpm/dvc.repo -O /etc/yum.repos.d/dvc.repo
sudo yum update
sudo yum install dvc
```

Add DVC files in the Git (It is not required but it is good to have project and dvc config in the same directory)

## Workflow for Model Packaging

- Setup for connecting to model repository during training development process
- Push the trained model to the repository
- Pull the model from the repository during the build process

### Pushing model files

``` bash
dvc init
dvc remote add -d gss-rdu-remote ssh://msivanes@gss-rdu-repo.usersys.redhat.com:/var/www/html/repo/config/ulmfit

dvc add cases_small_sbr_08-06-2020.pkl
git commit -am “Add ulmfit model to project”
dvc push -v
```

### Pulling the model

```bash
git clone $REPO
git pull
dvc pull # Pulls the data from remote-storage. Equivalent to dvc fetch followed by dvc checkout
dvc checkout #Update model files
```

## Food for thought

- Rather than feature branches, think in terms of experiment branches targeting metrics.
- Keep Data and Model files stored outside the repository
- Model files built as part of build process.
- Smoke test should validate the constructed model using validation set.

## References

- [DVC](https://dvc.org/doc)
- [Cheatsheet](https://www.globalsqa.com/dvc-cheat-sheet/)
- https://christophergs.github.io/machine%20learning/2019/05/13/first-impressions-of-dvc/
- https://www.slideshare.net/DmitryPetrov15/pydata-berlin-2018-dvcorg
