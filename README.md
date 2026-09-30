# CI/CD Final Project: Continuous Integration and Continuous Delivery Pipeline

## Project Name: CI/CD Pipeline Automation for Hit Counter Service

This repository contains the completed Final Project implementation for the Coursera / IBM course **Continuous Integration and Continuous Delivery (CI/CD)** (Course Code: `IBM-CD0215EN`).

---

## Project Overview

The objective of this project is to implement a robust, end-to-end CI/CD pipeline for a Python Flask RESTful microservice (**Hit Counter API**). The solution combines:
1. **Continuous Integration (CI) with GitHub Actions:** Automated checking, linting with `flake8`, and unit testing with `nose` and coverage reporting on every push and pull request.
2. **Continuous Delivery (CD) with Tekton & Red Hat OpenShift Pipelines:** Automated workspace cleanup, Git repository cloning, linting, testing, container image building with `buildah`, and container deployment to an OpenShift Kubernetes cluster using `openshift-client`.

---

## Tech Stack & Tools

- **Programming Language:** Python 3.9
- **Web Framework:** Flask / Gunicorn
- **Linting & Code Quality:** Flake8, Pylint
- **Testing & Coverage:** Nose, Pinocchio, Coverage
- **Continuous Integration:** GitHub Actions
- **Continuous Delivery & Orchestration:** Tekton Pipelines, Red Hat OpenShift
- **Containerization & Registry:** Docker, Buildah, OpenShift Internal Image Registry

---

## Project Structure

```text
├── .github/
│   └── workflows/
│       ├── README.md
│       └── workflow.yml        # GitHub Actions CI Workflow definition
├── .tekton/
│   ├── README.md
│   ├── tasks.yml               # Reusable Tekton Tasks (cleanup, nose)
│   └── pipeline.yaml           # Full CD Pipeline (init, clone, lint, test, build, deploy)
├── bin/
│   └── setup.sh                # Environment initialization script
├── service/
│   ├── __init__.py             # Flask application initialization and logging
│   ├── routes.py               # REST API endpoints for counter operations
│   └── common/
│       ├── error_handlers.py   # HTTP error handlers
│       ├── log_handlers.py     # Logging setup
│       └── status.py           # HTTP status codes
├── tests/
│   └── test_routes.py          # Unit test suite for Counter API
├── Dockerfile                  # Production container image definition
├── Procfile                    # Process file for application execution
├── requirements.txt            # Python dependencies
├── setup.cfg                   # Flake8 and Nose test configuration
└── README.md                   # Project documentation and details
```

---

## Implemented Workflows and Pipelines

### 1. GitHub Actions Workflow (`.github/workflows/workflow.yml`)
Automates the CI process on all pushes and pull requests to `main`:
- **Checkout:** Checks out source code using `actions/checkout@v3`.
- **Install dependencies:** Upgrades `pip` and installs requirements from `requirements.txt`.
- **Lint with flake8:** Validates PEP8 compliance and syntax errors.
- **Run unit tests with nose:** Executes unit tests with coverage reporting.

### 2. Tekton Tasks (`.tekton/tasks.yml`)
Contains reusable Tekton task definitions:
- **`cleanup` Task:** Cleans the workspace volume to ensure isolated and repeatable builds.
- **`nose` Task:** Installs required dependencies in Python 3.9 and runs nosetests with custom arguments.

### 3. OpenShift CD Pipeline (`.tekton/pipeline.yaml`)
Orchestrates the continuous delivery stages:
- `init`: Runs workspace cleanup using the `cleanup` task.
- `clone`: Clones the GitHub repository using the `git-clone` catalog task.
- `lint`: Lints service code with the `flake8` catalog task.
- `tests`: Executes unit tests using the custom `nose` task.
- `build`: Builds and pushes the Docker container image to the OpenShift image registry using `buildah`.
- `deploy`: Deploys the service to OpenShift using the `openshift-client` task.

---

## Local Setup & Testing

### 1. Initialize Environment
```bash
bash bin/setup.sh
```

### 2. Run Linting
```bash
flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
```

### 3. Run Unit Tests
```bash
nosetests -v --with-spec --spec-color --with-coverage --cover-package=service
```

---

## License

Licensed under the Apache License 2.0. See [LICENSE](/LICENSE) for details.

## Author

**Aniket Bedwal** (GitHub: [@anikett35](https://github.com/anikett35))  
IBM Skills Network - Continuous Integration and Continuous Delivery (CI/CD)
