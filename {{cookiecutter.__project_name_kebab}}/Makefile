NAME={{ cookiecutter.__project_name_snake }}
.DEFAULT_GOAL := help

define BROWSER_PYSCRIPT
import os, webbrowser, sys

from urllib.request import pathname2url

webbrowser.open("file://" + pathname2url(os.path.abspath(sys.argv[1])))
endef
export BROWSER_PYSCRIPT

define PRINT_HELP_PYSCRIPT
import re, sys

for line in sys.stdin:
	match = re.match(r'^([a-zA-Z_-]+):.*?## (.*)$$', line)
	if match:
		target, help = match.groups()
		print("%-20s %s" % (target, help))
endef
export PRINT_HELP_PYSCRIPT

BROWSER := python -c "$$BROWSER_PYSCRIPT"

.PHONY: help
help:
	@python -c "$$PRINT_HELP_PYSCRIPT" < $(MAKEFILE_LIST)

.PHONY: clean
clean: clean-build clean-pyc clean-test ## remove all build, test, coverage and Python artifacts

.PHONY: clean-build
clean-build: ## remove build artifacts
	rm -fr build/
	rm -fr dist/
	rm -fr .eggs/
	find . -name '*.egg-info' -exec rm -fr {} +
	find . -name '*.egg' -exec rm -f {} +

.PHONY: clean-pyc
clean-pyc: ## remove Python file artifacts
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f {} +
	find . -name '__pycache__' -exec rm -fr {} +

.PHONY: clean-test
clean-test: ## remove test and coverage artifacts
	rm -fr .tox/
	rm -f .coverage
	rm -fr htmlcov/
	rm -fr .pytest_cache

.PHONY: prepare-env
prepare-env: ## install dependencies and project in editable mode
	pip install -e .[dev]

.PHONY: lint
lint: type-check ipynb-check ## verify the code style
	pylint ${NAME} tests

.PHONY: type-check
type-check: ## run type checker for Python code
	mypy ${NAME}

.PHONY: ipynb-check
ipynb-check: ## check if jupyter notebook have no outputs
	@output="$$(git ls-files --cached --modified --other --exclude-standard | grep -E '.ipynb$$' | xargs nbstripout --dry-run)"; \
	if [ -n "$${output}" ]; then \
		echo 'Some notebooks have outputs. Strip the outputs with `make ipynb-strip` before committing:'; \
		echo "$${output}"; \
		exit 1; \
	fi

.PHONY: ipynb-strip
ipynb-strip: ## remove outputs from ipynb noteboooks
	git ls-files --cached --modified --other --exclude-standard | grep -E '.ipynb$$' | xargs nbstripout

.PHONY: test
test: ## run tests quickly with the default Python
	pytest ${PYTEST_FLAGS}

.PHONY: coverage
coverage: ## check code coverage quickly with the default Python
	coverage run --source ${NAME} -m pytest
	coverage report -m --omit=${NAME}/_version.py
	coverage html

.PHONY: coverage-web
coverage-web: coverage ## check code coverage and open report in web browser
	$(BROWSER) htmlcov/index.html

.PHONY: format
format: ## format the code using black
	black .

.PHONY: dist
dist: clean ## builds source and wheel package
	python -m build
	ls -l dist
