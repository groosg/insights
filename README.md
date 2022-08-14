# Insights - Software Development Insights by Groosg

This repo hosts the code for https://groosg.github.io/insights/.

It uses [MkDocs](https://www.mkdocs.org/getting-started/) and [Material for
MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started/).

## Local development

- Add content to `/docs`.
- Edit configurations and add your content to the navigation by changing
`mkdocs.yml`

### Evaluate your changes locally

Get the code

```shell
git clone git@github.com:groosg/insights.git
cd insights
```

Create a virtual environment

```shell
python3 -m venv venv
source venv/bin/activate
```

Install dependencies

```shell
pip install \
    "mkdocs-git-revision-date-localized-plugin>=1.0" \
    "mkdocs-minify-plugin>=0.3" \
    "mkdocs-redirects>=1.0"
```

Install mkdocs and material for mkdocs

```shell
pip install mkdocs-material
```

Run

```shell
venv/bin/mkdocs serve
```

Check the [Material for MkDocs installation](https://squidfunk.github.io/mkdocs-material/getting-started/#installation) for a variaty of installation methods.

**TODO:** Create and push a Docker image with the `mkdocs-git-revision-date-localized-plugin`.