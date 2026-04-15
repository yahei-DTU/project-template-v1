# 🍪 My personal project template

It is adapted from [MLOps template](https://github.com/SkafteNicki/mlops_template).

## ✋ Requirements to use the template:

* Python 3.11 or higher
* [cookiecutter](https://github.com/cookiecutter/cookiecutter) version 2.4.0 or higher
* Using `uv` as deps-manager

## 🆕 Start a new project

Start by creating a repository either using the GitHub GUI in the web browser or alternatively you can use the
[GitHub command line interface](https://cli.github.com/) if you have set it up:

```bash
gh repo create <repo_name> --public --confirm
```
Afterwards on your local machine run

```bash
cookiecutter https://github.com/SkafteNicki/mlops_template
```

You will be prompted with the following questions:

```txt
    [1/8] repo_name (repo_name):
    [2/8] project_name (project_name):
    [3/8] author_name (Your name (or your organization/company/team)):
    [4/8] author_email: (Your email address):
    [5/8] description (A short description of the project.):
    [6/8] keywords: (comma-separated, list, of, keywords):
    [7/8] python_version (3.12):
    [8/8] Select open_source_license
        1 - No license file
        2 - MIT
        3 - BSD-3-Clause
        Choose from [1/2/3] (1):
```

Where you should input starting values for the project. A couple of notes regarding the different options:

1. When asked for the `repo_name` e.g. the repository name, this should be the same as when you created the Github
    repository in the beginning.

2. When asked for the `project_name` this should be a
    [valid Python package name](https://peps.python.org/pep-0008/#package-and-module-names). This means that the name
    should be all lowercase and only contain letters, numbers and underscores. The project name will be used as the name
    of the Python package. This will automatically be validated by the template.

To commit to the remote repository afterwards execute the following series of commands:

```bash
cd <repo_name>
git init
git add .
git commit -m "init cookiecutter project"
git remote add origin https://github.com/<username>/<repo_name>
git push origin master
```

## 🗃️ Repository structure

The repository will look like
something like this:

```txt
├── .devcontainer
│   ├── devcontainer.json
│   └── post_create.sh
├── .env
├── .github
│   ├── dependabot.yaml
│   └── workflows
│       ├── data-tests.yaml
│       ├── deploy_docs.yaml
│       ├── linting.yaml
│       ├── pre-commit-update.yaml
│       └── tests.yaml
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── AGENTS.md
├── configs
│   ├── config_dev.yaml
│   ├── config_prod.yaml
│   ├── config_test.yaml
│   ├── datasets
│   │   └── data.yaml
│   ├── experiments
│   │   └── exp.yaml
│   └── models
│       └── model.yaml
├── dockerfiles
│   ├── api.dockerfile
│   └── train.dockerfile
├── docs
│   ├── mkdocs.yaml
│   ├── README.md
│   └── source
│       └── index.md
├── LICENSE
├── models
│   └── .gitkeep
├── notebooks
│   └── .gitkeep
├── pyproject.toml
├── README.md
├── reports
│   ├── figures
│   │   └── .gitkeep
│   └── .gitkeep
├── requirements.txt
├── requirements_dev.txt
├── src
│   └── project_name
│       ├── __init__.py
│       ├── api.py
│       ├── data.py
│       ├── evaluate.py
│       ├── model.py
│       ├── train.py
│       ├── utils
│       │   └── __init__.py
│       └── visualize.py
├── tasks.py
└── tests
    ├── __init__.py
    ├── test_api.py
    ├── test_data.py
    └── test_model.py
```

In particular lets explain the structure of the `src` folder as that is arguably the most important part of the
repository. The `src` folder is where the main code of the project is stored. The template divides the code into five
files, shown in the diagram below with their respective connections:

<img src="diagram.drawio.png" alt="diagram" width="1000"/>

* `data.py`: this file is responsible for everything related to the data. This includes loading, cleaning, and splitting
    the data. If the data needs to be pre-processed then running this file should process raw data in the `data/raw`
    folder and save the processed data in the `data/processed` folder.
* `model.py`: this file contains one or model definitions.
* `train.py`: this file is responsible for training the model. It should import the training/validation data interface
    from `data.py` and the model definition from `model.py`.
* `evaluate.py`: this file is responsible for evaluating the model. It should import the test data interface from
    `data.py` and load the trained model from the `models` folder. Output should be performance metrics of the trained
    model.
* `api.py`: this file is responsible for serving the model. It should import the trained model from the `models` folder
    and provide an interface for making predictions.
* `visualize.py`: this file is responsible for visualizing the data and model. It should import the training/validation/
    test data interface from `data.py` and the trained model from the `models` folder. Output should be visualizations
    of the data and model.

At some point one or more of the files may have grown too large and complicated. At this point it is recommended to
split the file into multiple files and move into a folder of the same name. As an example consider the `model.py`
containing many models. In this case it would be a good idea to refactor into

```txt
src/
└── project_name/
    ├── __init__.py
    ├── models/
    │   ├── __init__.py
    │   ├── model1.py
    │   └── model2.py
    ├── ...
```

## 📚 The stack

🐍 Python projects using `pyproject.toml`

🔥 Models in [Pytorch](https://pytorch.org/)

📦 Containerized using [Docker](https://www.docker.com/)

📄 Documentation with [Material Mkdocs](https://squidfunk.github.io/mkdocs-material/)

👕 Linting and formatting with [ruff](https://docs.astral.sh/ruff/)

✅ Checking using [pre-commit](https://pre-commit.com/)

🛠️ CI with [GitHub Actions](https://github.com/features/actions)

🤖 Automated dependency updates with [Dependabot](https://github.com/dependabot)

📝 Project tasks using [Invoke](https://www.pyinvoke.org/)
