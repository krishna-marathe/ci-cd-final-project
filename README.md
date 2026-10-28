
# eCommerce Product Catalog Service

**Project Name:** eCommerce Product Catalog Service

**Project Type:** CI/CD Final Project

**Description:** A RESTful eCommerce product catalog microservice built with Python, Flask, and PostgreSQL, using GitHub Actions for Continuous Integration and Tekton with Red Hat OpenShift for Continuous Delivery.

[![CI Workflow](https://github.com/1241007/ci-cd-final-project/actions/workflows/workflow.yml/badge.svg)](https://github.com/1241007/ci-cd-final-project/actions/workflows/workflow.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Python 3.9](https://img.shields.io/badge/python-3.9-blue.svg)](https://www.python.org/downloads/release/python-390/)


## Project Overview

This repository contains the completed **CI/CD Final Project** for the IBM DevOps and Software Engineering Professional Certificate capstone. It implements an enterprise-grade Continuous Integration (CI) and Continuous Delivery (CD) workflow for a cloud-native RESTful eCommerce Product microservice.

The service provides REST APIs to manage products (Create, Read, Update, Delete, List, Query by category, availability, and name) backed by PostgreSQL and Flask.

---

## Architecture & DevOps Toolchain

The solution employs a modern, dual-layer cloud-native pipeline:

```text
               +-------------------------------------------------+
               |             Developer Git Push                  |
               +-----------------------+-------------------------+
                                       |
                   +-------------------+-------------------+
                   |                                       |
                   v                                       v
      +-------------------------+             +-------------------------+
      |      GitHub Actions     |             |    OpenShift Pipelines  |
      |       (CI Workflow)     |             |      (Tekton CI/CD)     |
      +-------------------------+             +-------------------------+
      | 1. Checkout Code        |             | 1. Cleanup Workspace    |
      | 2. Setup Python 3.9     |             | 2. Git Clone Repository |
      | 3. Install Dependencies |             | 3. Lint with Flake8     |
      | 4. Lint with Flake8     |             | 4. Unit Tests with Nose |
      | 5. Unit Tests with Nose |             | 5. Build Image (Buildah)|
      +-------------------------+             | 6. Deploy to OpenShift  |
                                              +-------------------------+
```

### 1. Continuous Integration (CI) — GitHub Actions
- **Trigger**: Every `push` and `pull_request` against the `main` branch.
- **Workflow File**: `.github/workflows/workflow.yml`
- **Key Stages**:
  - `actions/checkout@v4`: Clones the latest commit.
  - `actions/setup-python@v5`: Configures Python 3.9 environment.
  - `Install dependencies`: Upgrades pip and installs packages from `requirements.txt`.
  - `Lint with flake8`: Enforces PEP 8 syntax and formatting standards.
  - `Run unit tests with nose`: Executes unit test suites with code coverage analysis using PostgreSQL service container.

### 2. Continuous Delivery (CD) — Tekton & Red Hat OpenShift Pipelines
- **Pipeline Resource**: `cd-pipeline`
- **Tasks Definition**: `.tekton/tasks.yml`
- **Pipeline Definition**: `.tekton/pipeline.yml`
- **Pipeline Sequence**:
  1. `cleanup`: Employs an `alpine` container to scrub previous checkout artifacts from the persistent workspace.
  2. `git-clone`: Pulls the Git repository into the shared PVC workspace (`pipelinerun-pvc`).
  3. `flake8`: Runs style checks and syntax analysis inside a `python:3.9-slim` container.
  4. `nose`: Runs automated test suite (`nosetests -v`) with coverage.
  5. `buildah`: Builds the OCI-compliant container image from the repository `Dockerfile` and pushes to the internal OpenShift image registry.
  6. `deploy`: Performs a rolling update using `openshift-client` (`oc apply` and `oc rollout status`).

---

## Local Development & Testing

### Running Tests Locally
To execute the test suite and verify coverage:
```bash
nosetests -v --with-spec --spec-color --with-coverage --cover-package=service
```

### Running the Linter
To verify code formatting and styling:
```bash
flake8 service tests --count --select=E9,F63,F7,F82 --show-source --statistics
flake8 service tests --count --max-complexity=10 --max-line-length=127 --statistics
```

---

## Deliverables & Evidence Summary

- **GitHub Repository**: [https://github.com/1241007/ci-cd-final-project](https://github.com/1241007/ci-cd-final-project)
- **CI Workflow**: `.github/workflows/workflow.yml`
- **Tekton Pipeline Resources**: `.tekton/tasks.yml` & `.tekton/pipeline.yml`
- **OpenShift PVC Details**: `oc-pipelines-console-pvc-details.png`
- **OpenShift Pipeline Structure**: `oc-pipelines-oc-final.png`
- **OpenShift Successful PipelineRun**: `oc-pipelines-oc-green.png`

---

## Author

- **Rushikesh Pathare** ([@1241007](https://github.com/1241007))
- DevOps Engineer

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.


