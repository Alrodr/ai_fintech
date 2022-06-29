---
title: "Setting up the environment"
description: "Setting up the environment"
lead: "Setting up the environment with Conda"
date: 2022-06-13
lastmod: 2022-06-13
draft: false
images: []
menu:
  docs:
    parent: "introduction"
weight: 10
toc: true
---
{{< alert icon="💻" context="info" text="To run the notebooks provided in Data Doks we recommend you to have the following working environment configured." />}}

Git clone the repo onto the target OS, and create a conda environment from it as follows:

```bash
cd datadoks
conda env create -f conda.yml
```

If you want to setup the environment for jupyter notebooks type

```bash
python -m ipykernel install --user --name datadoks --display-name "datadoks"
ipython kernel install --name "datadoks" --user
jupyter notebook
```

### :snake: Installation using conda from scratch

The recommended way of configuring your system is by using a conda environment. We recommend that you install the latest version of Miniconda from [here.](https://docs.conda.io/en/latest/miniconda.html)

You can then create a conda environment for these notes using

```bash
conda create -n datadoks
```
To activate this environment, use
```bash
conda activate datadoks
```

Install all of the required packages:

```bash
conda install numpy
conda install matplotlib
conda install scipy
conda install tqdm
conda install scikit-learn
conda install pandas
conda install ipykernel
```

If you want to setup the environment for jupyter notebooks type

```bash
python -m ipykernel install --user --name datadoks --display-name "datadoks"
ipython kernel install --name "datadoks" --user
jupyter notebook
```

Now, you are ready to start :tada:!

---
 [:pencil2: Edit this document on Github](https://github.com/Alrodr/datadoks/blob/main/content/statistics_ml/contents/setting_up.md)