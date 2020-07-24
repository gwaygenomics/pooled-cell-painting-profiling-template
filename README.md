# Pooled Cell Painting: Data Pipeline Welding Template :hammer_and_wrench:

This repository was derived from a [template repository](https://github.blog/2019-06-06-generate-new-repositories-with-repository-templates/) located at https://github.com/broadinstitute/pooled-cell-painting-profiling-template.
The purpose of the repository is to weld together a versioned data processing pipeline with versioned processed output data for a single Pooled Cell Painting experiment.

## Setup computational environment

First, install [conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/).
We use conda as an environment manager.

```bash
# Install computational environment
conda env create --force --file environment.yml

# Initialize the environment
conda activate pooled-cell-painting
```

## Perform the weld

The welding procedure is a three-step process.

1. Activate conda environment (see above)
2. Manually update the configuration yaml documents for your specific experiment
    * Yaml documents with reasonable default values are available in the [config/](config/) folder.
      * Do not change the location of these files.
    * Additional documentation for each of the parameters is available in the [config/docs/](config/docs/) folder.
3. Execute `weld.sh` (see below)

```bash
# After performing steps 1 and 2 above, perform step 3:
./weld.sh
```

## **AFTER GENERATING A NEW REPO, CHANGE OR DELETE ALL NONSPECIFIC DETAILS**

<p align="center">
<img src="https://raw.githubusercontent.com/broadinstitute/pooled-cp-profiling-template/a57cb7f9e36b89ff56acf094f18ca06b1a53b719/media/pipeline_weld.png" width="500">
</p>

## Setup

To correctly initialize the repository, we need to perform several manual steps.

### Step 0: Create a New Repository **using this Repository as a Template**

By spinning up a new repo using this repo as a template, you will retain all code, configuration files, computational environments, and directory structure that a standard Pooled Cell Painting workflow expects and produces.

### Step 1: Fork The Pooled Cell Painting Painting Recipe

We first want to [fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) the official pooled cell profiling recipe located at https://github.com/broadinstitute/pooled-cp-profiling-recipe.

* **Result:** The fork creates a copy of a recipe repository.
* **Goals:** 1) Remove the connection to official recipe updates to avoid unintended weld versioning reversal; 2) Enable independent updates to fork code that does not impact official recipe.
* **Execution:** See [forking instructions](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) and the image below.

![Step 1: Fork](media/step1_forkrecipe.png)

### Step 2: Create a Submodule inside this Repository of the Forked Recipe

Next, we will create a [submodule](https://gist.github.com/gitaarik/8735255) in this repo.

* **Result:** Adding a submodule initiates the weld.
* **Goals:** 1) Link the processing code (recipe) with the data (current repo); 2) Require a manual step to update the recipe to enable asynchronous development.
* **Execution:** See below

```bash
# In your terminal, clone the repository you just created (THIS REPO)
USER="INSERT-USERNAME-HERE"
REPO="INSERT-NAME-HERE"
git clone git@github.com:$USER/$REPO.git

# Navigate to this directory
cd $REPO

# Add the Recipe Submodule
git submodule add https://github.com/$USER/pooled-cp-profiling-recipe.git pooled-cp-profiling-recipe
```

Refer to ["Adding a submodule"](https://gist.github.com/gitaarik/8735255#adding-a-submodule) for more details.

### Step 3: Commit the Submodule

Lastly, we will [commit](https://help.github.com/en/desktop/contributing-to-projects/committing-and-reviewing-changes-to-your-project#about-commits) the submodule to github.

* **Result:** Committing this change finalizes the weld.
* **Goals:** 1) Track the submodule (recipe) version with the current repository.
* **Execution:** See below

```bash
# Add, commit, and push the submodule contents
git add pooled_cp_profiling_recipe
git add .gitmodules
git commit -m 'finalizing the recipe weld'
git push
```