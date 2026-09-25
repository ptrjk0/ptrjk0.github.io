# ptrjk0.github.io

This repository contains the source for my personal website and blog, built with
[Quarto](https://quarto.org/) and hosted on GitHub Pages at
<https://ptrjk0.github.io>. It includes two computational blog posts, one written
in R and one in Python, each with its own reproducible environment.

## What to install first

Install these tools before building the site. The versions listed are the ones
I used.

| Tool | Version | Install from |
|------|---------|--------------|
| Git | 2.50.1 | <https://git-scm.com/downloads> |
| Quarto | 1.10.18 | <https://quarto.org/docs/get-started/> |
| uv | 0.12.5 | <https://docs.astral.sh/uv/getting-started/installation/> |
| R | 4.6.1 | <https://cran.r-project.org/> |

You do not need to install Python or renv yourself:

- **Python 3.14** is pinned in `.python-version`. `uv` downloads and installs
  it automatically in step 2 below if you do not already have it.
- **renv** installs itself the first time R starts in this repository, via
  `.Rprofile`.

## Build the site

Run every command below in a terminal, starting from the directory where you
want the repository to live. On Windows, use Git Bash.

**1. Clone the repository and move into it**

```bash
git clone https://github.com/ptrjk0/ptrjk0.github.io.git
cd ptrjk0.github.io
```

All remaining commands are run from this top-level folder (the one containing
`_quarto.yml`).

**2. Install the Python environment**

```bash
uv sync
```

This reads `pyproject.toml` and `uv.lock`, installs Python 3.14 if needed, and
creates a `.venv/` folder with the exact package versions used.

**3. Install the R environment**

```bash
Rscript -e "renv::restore(prompt = FALSE)"
```

This starts R in the repository, where `.Rprofile` activates renv (installing
renv itself if needed), then installs the exact package versions recorded in
`renv.lock`. The first run can take several minutes.

**4. Render the site**

```bash
uv run quarto render
```

`uv run` makes Quarto use the Python environment from step 2. Always run this
from the top-level folder so R picks up `.Rprofile` and the renv environment.

## Where the built site lands and how to view it

The rendered site is written to the `docs/` folder, which is what GitHub Pages
serves.

To view it locally, run this from the top-level folder:

```bash
uv run quarto preview
```

This renders the site, starts a local web server, and opens the site in your
browser. Press `Ctrl + C` in the terminal to stop it.

You can also open `docs/index.html` directly in a browser, but some features
(such as site search) only work when the site is served through a web server.

## Data

Both computational posts use the gapminder dataset by Jennifer Bryan
(<https://jennybc.github.io/gapminder/>, CC0), based on free material from
[GAPMINDER.ORG](https://www.gapminder.org/data/), CC-BY LICENSE.

- **R post:** the data comes from the `gapminder` R package, which is installed
  by `renv::restore()` in step 3. No separate download is needed.
- **Python post:** the data is read at render time from the package's TSV file
  on GitHub:
  <https://raw.githubusercontent.com/jennybc/gapminder/refs/heads/main/inst/extdata/gapminder.tsv>

**Network access:** Building the site requires an internet connection. Steps 2
and 3 download packages, and step 4 downloads the Python post's data from
GitHub. The Python post will only render while that GitHub file remains
available.

## Troubleshooting

If the Python post fails with `ModuleNotFoundError`, Quarto may be using the
wrong Python. From the top-level folder, run:

```bash
rm -r .quarto
uv run quarto render
```