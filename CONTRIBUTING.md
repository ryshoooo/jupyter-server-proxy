# Contributing

Welcome! As a [Jupyter](https://jupyter.org) project, we follow the [Jupyter contributor guide](https://jupyter.readthedocs.io/en/latest/contributor/content-contributor.html).

To setup a local development environment and ru tests, see the small section in
the README.md file.

## Local development setup

### Python package

```bash
pip install -e .[test]

# explicit install needed with editable mode (-e) jupyter
jupyter serverextension enable --sys-prefix jupyter_server_proxy
jupyter server extension enable --sys-prefix jupyter_server_proxy
```

Before running tests, you need a server that we can test against.

```
JUPYTER_TOKEN=secret jupyter-lab --config=./tests/resources/jupyter_server_config.py --no-browser
```

Run the tests:

```bash
pytest --verbose
```

These generate test and coverage reports in `build/pytest` and `build/coverage`.

### Acceptance tests

If you have `robotframework-jupyterlibary` installed, the acceptance tests will run.

To install these in addition to the [Python package](#python-package) test
dependencies, run:

```bash
pip install -e .[test,acceptance]
```

In addition, compatible versions of:

- `geckodriver`
- `firefox`

Needs to be on your `$PATH` and compatible with each other.

To run _only_ the acceptance tests, use the `-k` switch:

```bash
pytest -k acceptance
```

These are slower than the rest of the `pytest` tests, and generate screenshots,
browser logs, server logs, and report HTML in `build/robot`.

### JupyterLab extension

The `jlpm` command is JupyterLab's pinned version of `yarn` that is
installed with JupyterLab.

> You may use `yarn` or `npm run` instead of `jlpm` below.

```bash
cd jupyterlab-server-proxy # Change to the root of the labextension
jlpm                       # Install dependencies (or `npm i`)
jlpm build:prod            # Build:
                           # - `jupyterlab-server-proxy/lib`
                           # - `jupyter_server_proxy/labextension`
jlpm install:extension     # Symlink into `{sys.prefix}/share/jupyter/labextensions`
```

You can watch the source directory and automatically rebuild the `lib` folder:

```bash
jlpm watch   # ... watch the source directory in another terminal tab
```

However, the built-in `jupyter labextension watch` does _not_ work with this repo,
as the `package.json` and `setup.py` would need to be at the same level.
